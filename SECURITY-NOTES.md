# Log4Shell (CVE-2021-44228) Security Notes

## Core Facts

- **What it is:** Log4Shell is a critical remote code execution (RCE) vulnerability in Apache Log4j 2, a widely used Java logging library. An attacker who can control log messages or log message parameters can exploit JNDI features to execute arbitrary code loaded from LDAP and other JNDI-related endpoints.
- **CVE ID:** CVE-2021-44228
- **Affected Log4j 2 versions:** 2.0-beta9 through 2.14.1 (version ranges per Apache: [2.0-beta9, 2.3.1) ∪ [2.4, 2.12.2) ∪ [2.13.0, 2.15.0))
- **Fixed versions:** 2.3.1 (Java 6), 2.12.2 (Java 7), and 2.15.0 (Java 8 and later). Recommended final target: 2.17.1+ (2.17.1 for Java 8+, 2.12.4 for Java 7, 2.3.2 for Java 6), because the 2.15.0 fix was incomplete (CVE-2021-45046) and subsequent releases (2.16.0, 2.17.0, 2.17.1) addressed further related issues.
- **Severity:** CVSS 3.x = 10.0 CRITICAL (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H); CVSS 2.x = 7.5. Rated Critical by NVD and Apache.

## Remediation Actions

1. **Upgrade path:** Upgrade log4j-core to a fixed version (2.3.1 / 2.12.2 / 2.15.0+; target 2.17.1+). Only the log4j-core JAR is impacted; log4j-api-only applications are not affected.
2. **Detecting vulnerable deployments:** Scan dependencies, container images, fat jars, and legacy applications for log4j-core versions in the affected range (2.0-beta9–2.14.1).
3. **Hardening — disable JNDI lookups:** Set `log4j2.formatMsgNoLookups=true` (or `LOG4J_FORMAT_MSG_NO_LOOKUPS=true`) on Java 8u121+/2.10.0+, or remove the `JndiLookup` class from the log4j-core JAR. For Java 6/7, upgrade to 2.3.1/2.12.2.
4. **Automated dependency scanning:** Integrate OWASP Dependency-Check, Snyk, Trivy, or GitHub Dependabot alerts into CI/CD; fail builds on critical CVEs and pin log4j-core to >= 2.17.1 (Java 8+), 2.12.4 (Java 7), or 2.3.2 (Java 6).

## Sources

- Apache Logging Services Security page: https://logging.apache.org/log4j/2.x/security.html
- NVD CVE-2021-44228: https://nvd.nist.gov/vuln/detail/CVE-2021-44228
- CISA / Australian Cyber Security Centre advisory (CVE-2021-44228, CVE-2021-45046, CVE-2021-45105, CVE-2021-44832): https://www.cyber.gov.au/sites/default/files/2023-02/2021-007%20-%20Apache%20Log4j2%20vulnerability%20%E2%80%93%20advice%20and%20mitigations.pdf
- Huntress threat library entry for CVE-2021-44228: https://www.huntress.com/threat-library/vulnerabilities/cve-2021-44228
