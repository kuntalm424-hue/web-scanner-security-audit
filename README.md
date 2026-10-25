# Automated Web Scanner — Secure Headers & CORS Audit

**IBM SkillsBuild Cyber Security 2026 · BharatCares · Project #2**

A Python tool that passively audits websites for missing/weak **HTTP security headers** and **CORS misconfigurations**, scores each site (0–100, grade A–F) and exports JSON, CSV and HTML reports.

## Project Description
Misconfigured response headers and over-permissive CORS policies are common, easy-to-fix web vulnerabilities (OWASP *Security Misconfiguration*). This scanner sends safe, read-only requests (`GET` / `OPTIONS`) and checks:

| Area | What is checked |
|---|---|
| Security headers | HSTS, Content-Security-Policy, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy, COOP, CORP (present *and* well configured) |
| Information leakage | `Server` version banners, `X-Powered-By`, `X-AspNet-Version` |
| Cookies | `Secure`, `HttpOnly`, `SameSite` flags |
| CORS | Reflected arbitrary origin, `null` origin trusted, wildcard `*` (with credentials), suffix-trust bypass (`target.evil.example`), missing `Vary: Origin`, permissive preflight |

Each finding includes severity (High / Medium / Low), an explanation and a recommended fix.

## Technologies Used
- Python 3.9+
- `requests` — HTTP requests
- `pandas` — result tables and CSV export
- `matplotlib` — score chart
- `http.server` (standard library) — built-in local test lab
- Jupyter Notebook

## Dataset
No external dataset is required. The notebook starts **two local demo servers** (one deliberately vulnerable, one hardened) and scans them, so it works offline and never touches third-party sites.
You may add your own sites to the `TARGETS` list in the notebook (only sites you own or have written permission to test).

## Setup & Run
```bash
# 1. (optional) create a virtual environment
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# 2. install dependencies
pip install -r requirements.txt

# 3. open the notebook
jupyter notebook KuntalMondal_WebScanner.ipynb
```
Then choose **Kernel → Restart & Run All**. Reports are saved in the `reports/` folder (`.json`, `.csv`, `.html`, `score_chart.png`).

To scan your own site, edit the `TARGETS` list in section 6:
```python
TARGETS += ["https://your-own-site.com"]
```

## Scoring
`score = 100 − penalties` (High −15, Medium −8, Low −3).
Grades: **A** ≥ 90 · **B** ≥ 75 · **C** ≥ 60 · **D** ≥ 40 · **F** < 40.

## Sample Result (local lab)
| Target | Score | Grade | High | Medium | Low |
|---|---|---|---|---|---|
| Vulnerable demo | 0 | F | 4 | 4 | 9 |
| Hardened demo | 100 | A | 0 | 0 | 0 |

## Key Information
- **Passive only:** no exploitation, fuzzing, or brute force; polite delay between requests.
- **Ethical use:** scan only systems you are authorised to test.
- **Limitations:** scans a single URL per site (not a full crawl); header checks reflect that response only; results are indicators, not proof of exploitability.
- **Future work:** bulk scanning from a file, TLS certificate checks, CI/CD security gate, historical comparison.

## Files
| File | Purpose |
|---|---|
| `KuntalMondal_WebScanner.ipynb` | Complete project code |
| `requirements.txt` | Python dependencies |
| `KuntalMondal_ProjectReport.docx` | Project documentation |
| `README.md` | This file |

## Author
Kuntal Mondal — IBM SkillsBuild Cyber Security 2026, BharatCares
