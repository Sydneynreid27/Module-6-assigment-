# Module 6: Dependency Vulnerability Assessment

## Project Overview
This assessment reviewed a small public Node.js project, OWASP NodeGoat, which is a deliberately insecure sample application used to teach web application security issues. The project includes a legacy dependency tree with many outdated packages and a large number of transitive vulnerabilities.

## Vulnerability Scanning Method
The project was scanned using the standard Node.js dependency audit tool:

```bash
npm install
npm audit --json
```

This method checks the dependency tree against the npm advisory database and reports package names, severities, affected versions, and remediation guidance.

## Summary of Scan Results
The audit found 145 vulnerabilities in total:

| Severity | Count |
|---|---:|
| Critical | 38 |
| High | 66 |
| Moderate | 33 |
| Low | 8 |
| Info | 0 |
| Total | 145 |

The most significant findings are concentrated in older library versions, especially utilities and transitive packages that are vulnerable to prototype pollution, code execution, and denial-of-service issues.

## Detailed Findings

| Vulnerable Package | Severity | Affected Versions | Description | Reference ID |
|---|---|---|---|---|
| `underscore` | Critical | `>=1.3.2 <1.12.1`, `<=1.13.7` | Arbitrary code execution and DoS risk in older versions. | GHSA-cf4h-3jhx-xvhq |
| `lodash` | Critical | `<=4.17.23` | Prototype pollution and command execution issues. | GHSA-35jh-r3h4-6jhm; GHSA-4xc9-xhrj-v574 |
| `minimist` | Critical | `<=0.2.3`, `1.0.0 - 1.2.5` | Prototype pollution can alter object behavior and enable exploitation paths. | GHSA-xvch-5gv4-984h |
| `request` | Critical | Older versions | Deprecated HTTP library with SSRF and memory exposure issues. | GHSA-p8p7-x288-28g6 |
| `set-value` | Critical | `<=2.0.0` | Prototype pollution through deep object assignment. | GHSA-4g88-fppr-53pp |

### 1) `underscore` — Critical
- Vulnerable package: `underscore`
- Description: Older versions of `underscore` are affected by arbitrary code execution and recursive DoS conditions.
- Severity: Critical
- Affected versions: `>=1.3.2 <1.12.1` and `<=1.13.7`
- Reference: GHSA-cf4h-3jhx-xvhq
- Remediation: Upgrade to a patched version and remove any unnecessary imports. If the package is not essential, replace it with a maintained utility library.

### 2) `lodash` — Critical
- Vulnerable package: `lodash`
- Description: `lodash` has multiple critical advisories involving prototype pollution and command injection. These issues can lead to unsafe object mutation and execution paths.
- Severity: Critical
- Affected versions: `<=4.17.23`
- Reference: GHSA-35jh-r3h4-6jhm, GHSA-4xc9-xhrj-v574
- Remediation: Upgrade to a secure maintained release, review all usage patterns, and replace functions with safer modern alternatives where possible.

### 3) `minimist` — Critical
- Vulnerable package: `minimist`
- Description: This package is vulnerable to prototype pollution, which can result in unsafe object modifications and application logic compromise.
- Severity: Critical
- Affected versions: `<=0.2.3` and `1.0.0 - 1.2.5`
- Reference: GHSA-xvch-5gv4-984h
- Remediation: Upgrade to a patched version or remove the dependency if it is no longer needed. Audit any package-lock overrides and transitive versions.

## Mitigation Recommendations
1. Upgrade vulnerable packages to patched versions immediately.
2. Remove deprecated packages such as `request` where possible.
3. Refresh the project dependency tree and regenerate the lockfile.
4. Add automated dependency scanning to CI/CD using `npm audit` or a tool such as Snyk.
5. Replace legacy dependencies with safer maintained alternatives whenever feasible.

## Conclusion
This project has a high overall security risk due to the large number of vulnerable packages and multiple critical advisories. Dependency upgrades should be prioritized before production use. The most urgent action is to update or remove vulnerable packages such as `underscore`, `lodash`, `minimist`, `request`, and `set-value`, then retest the app to confirm compatibility and stability.

This assessment demonstrates the importance of regular dependency maintenance and automated security review in Node.js projects.
