# Lab 2 — Threat Modeling with Threagile

## Task 1 — Baseline Risk Analysis

### Risk Ranking Methodology

Composite Score = Severity*100 + Likelihood*10 + Impact

Severity weights:
- critical = 5
- elevated = 4
- high = 3
- medium = 2
- low = 1

Likelihood weights:
- very-likely = 4
- likely = 3
- possible = 2
- unlikely = 1

Impact weights:
- high = 3
- medium = 2
- low = 1

---

### Top 5 Risks (Baseline)

| Rank | Category | Asset | Severity | Likelihood | Impact | Score |
|------|----------|--------|----------|------------|--------|-------|
| 1 | unencrypted-communication | User Browser → Juice Shop | elevated | likely | high | 433 |
| 2 | unencrypted-communication | Reverse Proxy → Juice Shop | elevated | likely | medium | 432 |
| 3 | missing-authentication | Reverse Proxy → Juice Shop | elevated | likely | medium | 432 |
| 4 | cross-site-scripting | Juice Shop Application | elevated | likely | medium | 432 |
| 5 | cross-site-request-forgery | Juice Shop Application | medium | very-likely | low | 241 |

### Analysis

The highest risks are related to unencrypted communication and missing authentication.
The absence of HTTPS significantly increases the risk of credential interception.

The model also highlights application-level vulnerabilities such as XSS and CSRF,
which are common web application risks.

The lack of transport encryption represents the most critical architectural weakness.

## Task 2 — HTTPS Variant & Risk Comparison

### Risk Category Delta Table

| Category | Baseline | Secure | Δ |
|---|---:|---:|---:|
| container-baseimage-backdooring | 1 | 1 | 0 |
| cross-site-request-forgery | 2 | 2 | 0 |
| cross-site-scripting | 1 | 1 | 0 |
| missing-authentication | 1 | 1 | 0 |
| missing-authentication-second-factor | 2 | 2 | 0 |
| missing-build-infrastructure | 1 | 1 | 0 |
| missing-hardening | 2 | 2 | 0 |
| missing-identity-store | 1 | 1 | 0 |
| missing-vault | 1 | 1 | 0 |
| missing-waf | 1 | 1 | 0 |
| server-side-request-forgery | 2 | 2 | 0 |
| unencrypted-asset | 2 | 1 | -1 |
| unencrypted-communication | 2 | 0 | -2 |
| unnecessary-data-transfer | 2 | 2 | 0 |
| unnecessary-technical-asset | 2 | 2 | 0 |

### Delta Analysis Explanation

The secure variant introduced the following architectural improvements:

- HTTPS enabled for all communication links
- Encryption enabled for Persistent Storage

Observed impact:

- All "unencrypted-communication" risks were eliminated (2 → 0)
- One "unencrypted-asset" risk was reduced due to storage encryption (2 → 1)

This demonstrates that enabling HTTPS effectively mitigates transport-layer interception risks.
However, application-layer risks (XSS, CSRF, SSRF, missing authentication controls)
remain unchanged because they are unrelated to transport security.

Conclusion:

Transport encryption significantly improves the security posture,
but does not replace the need for application security controls and hardening.
