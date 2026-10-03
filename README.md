# Module 6: Dependency Vulnerability Report

## Project Overview
This project is a small Node.js app called OWASP NodeGoat. It is a sample app that is intentionally outdated and has lots of old dependencies.

## How I Checked It
I used the Node.js audit tool:

```bash
npm install
npm audit --json
```

This checks the project dependencies against known security issues.

## What the Scan Found
There were 145 total vulnerabilities.

| Severity | Number |
|---|---:|
| Critical | 38 |
| High | 66 |
| Moderate | 33 |
| Low | 8 |

This means the project has a lot of security problems, especially in old libraries.

## Important Problems Found

| Package | Severity | Affected Versions | Problem | Reference |
|---|---|---|---|---|
| `underscore` | Critical | `>=1.3.2 <1.12.1`, `<=1.13.7` | Can allow code execution | GHSA-cf4h-3jhx-xvhq |
| `lodash` | Critical | `<=4.17.23` | Prototype pollution and code execution risk | GHSA-35jh-r3h4-6jhm |
| `minimist` | Critical | `<=0.2.3`, `1.0.0 - 1.2.5` | Prototype pollution | GHSA-xvch-5gv4-984h |
| `request` | Critical | Older versions | Deprecated and unsafe HTTP library | GHSA-p8p7-x288-28g6 |
| `set-value` | Critical | `<=2.0.0` | Prototype pollution | GHSA-4g88-fppr-53pp |

### 1) `underscore`
- This package is very old and has critical security issues.
- It can allow code execution in vulnerable versions.
- Fix: update it to a safe version or remove it if it is not needed.

### 2) `lodash`
- This library is also very outdated.
- It has serious vulnerabilities that can affect app data and sometimes execution.
- Fix: upgrade to a patched version or replace it with a safer alternative.

### 3) `minimist`
- This is a common dependency issue in old Node apps.
- It can be exploited through prototype pollution.
- Fix: update to a safe release or remove the dependency if it is not necessary.

## Simple Fixes
1. Update all old packages.
2. Remove unused or deprecated libraries.
3. Regenerate the lock file.
4. Run dependency checks regularly.
5. Use GitHub Dependabot or Snyk for automation.

## Conclusion
This project has a high risk because it uses many outdated packages. The biggest problems are in older libraries like `underscore`, `lodash`, and `minimist`. The app should not be used as-is without updating dependencies and testing again.

This project shows why it is important to keep dependencies updated and check them often.
