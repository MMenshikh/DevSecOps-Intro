# Triage Report — OWASP Juice Shop

## Scope & Asset
- Asset: OWASP Juice Shop (local lab instance)
- Image: bkimminich/juice-shop:v19.0.0
- Release link/date: https://github.com/juice-shop/juice-shop/releases/tag/v19.0.0 — Sep 4, 2025

## Environment
- Host OS: Windows 11
- Docker: Docker version 28.0.4, build b8034c0

## Deployment Details
- Run command used:
docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop:v19.0.0

- Access URL: http://127.0.0.1:3000
- Network exposure: 127.0.0.1 only [x] Yes

## Health Check

### Page load
See screenshot: labs/screenshots/homepage.png

### API check

Command:
curl http://127.0.0.1:3000/api/Challenges

Output:
{"status":"success","data":[{"id":1,"key":"restfulXssChallenge","name":"API-only XSS","category":"XSS","tags":"Danger Zone,With Coding Challenge","description":"Perform a <i>persisted</i> XSS attack with <code>&lt;iframe src=\"javascript:alert(`xss`)\"&gt;</code> without using the frontend application at all. <em>(This challenge is <strong>potentially harmful</strong> on Docker!)</em>","difficulty":3,"mitigationUrl":"https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html","solved":false,"disabledEnv":"Docker", ...}

## Surface Snapshot

- Login/Registration visible: Yes
- Product listing/search present: Yes
- Admin/account area discoverable: Yes
- Client-side errors in console: No
- Security headers: missing CSP and HSTS

## Risks Observed

1) Broken Authentication — login form exposed publicly
2) Sensitive API endpoints accessible without authentication
3) Missing security headers — potential XSS and MITM risks
