# 04. Cybersecurity Risk Assessment & UK Legal Frameworks
## British Broadcasting Corporation

### Security Philosophy & Industry Standards
As a high-profile national broadcaster and critical UK digital infrastructure provider, the BBC is continuously targeted by nation-state cyber adversaries, hacktivists, and phishing fraudsters. The BBC operates a dedicated Cyber Security Operations Centre (SOC) enforcing Defence-in-Depth, UK GDPR compliance, and ISO/IEC 27001 standards.

### Compliance Standards & Frameworks
- UK General Data Protection Regulation (UK GDPR)
- Data Protection Act 2018
- ISO/IEC 27001 Information Security Management
- Ofcom Broadcasting Code Security Standards
- Cyber Essentials Plus Certification
- PCI-DSS Level 1 (Commercial BBC Shop Services)

### Threat Risk Matrix
---------------------------------------------------------
### Threat 1: Brand Impersonation & TV Licence Phishing Scams
- **Risk Severity Level:** CRITICAL
- **Attack Vector:** Email spoofing, SMS smishing, typo-squatted domain names (e.g. bbc-licence-renew.co.uk).
- **Operational & Reputational Impact:** Criminal syndicates send fraudulent emails and SMS messages claiming the recipient’s "BBC TV Licence has expired" or "Payment failed", directing victims to counterfeit lookalike sites that steal banking details.
- **Technical Mitigation Strategy:** Deployment of DMARC (Domain-based Message Authentication) with strict p=reject policy, active brand monitoring and legal takedown teams, customer education campaigns, and domain blocklisting via the National Cyber Security Centre (NCSC) Protective DNS.
- **Applicable UK Legal Statute:** Computer Misuse Act 1990 (Section 1 & 3), Fraud Act 2006.

---------------------------------------------------------
### Threat 2: Distributed Denial of Service (DDoS) on Election Nights
- **Risk Severity Level:** CRITICAL
- **Attack Vector:** SYN floods, UDP amplification, Layer 7 HTTP GET floods attacking un-cached search and live results endpoints.
- **Operational & Reputational Impact:** Malicious botnets attempt to flood BBC web servers with gigabits of garbage traffic during general elections or major royal broadcasts, aiming to censor news reporting and sow public panic.
- **Technical Mitigation Strategy:** Multi-layered DDoS protection via Fastly and Akamai Edge Scrubbing centres, rate limiting per IP, automatic CAPTCHA challenges for anomalous volumetric bursts, and direct peering with major UK ISPs.
- **Applicable UK Legal Statute:** Computer Misuse Act 1990 Section 3 (Unauthorised act with intent to impair computer operation).

---------------------------------------------------------
### Threat 3: Cross-Site Scripting (XSS) & Content Injection
- **Risk Severity Level:** HIGH
- **Attack Vector:** Reflected or stored XSS vulnerabilities in user-submitted comments or search input forms.
- **Operational & Reputational Impact:** Attackers attempt to inject malicious JavaScript into live news comment sections, feedback forms, or search query parameters, which could steal session tokens or deface reputable headlines.
- **Technical Mitigation Strategy:** Strict Content Security Policy (CSP) blocking unauthorized script execution, automated HTML entity sanitisation (DOMPurify), React JSX automatic escaping, and disabling eval() functions.
- **Applicable UK Legal Statute:** Data Protection Act 2018, OWASP Top 10 A03: Injection.

---------------------------------------------------------
### Threat 4: Account Takeover & Credential Stuffing on BBC ID
- **Risk Severity Level:** HIGH
- **Attack Vector:** Distributed credential stuffing attacks utilizing rotating residential proxy networks.
- **Operational & Reputational Impact:** Automated bots test millions of stolen username/password pairs obtained from third-party data breaches against the BBC ID login endpoint to hijack viewer accounts and scrape viewing habits.
- **Technical Mitigation Strategy:** Web Application Firewall (WAF) bot detection, biometric and behavioural analysis, rate limiting, mandatory multi-factor authentication (MFA) for staff accounts, and integration with "Have I Been Pwned" APIs to block compromised passwords.
- **Applicable UK Legal Statute:** UK GDPR Article 32 (Security of Processing), Data Protection Act 2018.

---------------------------------------------------------
### Threat 5: Third-Party Software Supply Chain Vulnerabilities
- **Risk Severity Level:** HIGH
- **Attack Vector:** Malicious dependencies in node_modules, compromised CI/CD build pipelines, outdated third-party scripts.
- **Operational & Reputational Impact:** Compromise of an open-source npm library or analytics SDK used on the BBC website could allow attackers to execute arbitrary code or spy on user sessions.
- **Technical Mitigation Strategy:** Automated Dependabot and Snyk vulnerability scanning in GitHub Actions, Software Bill of Materials (SBOM) tracking, Subresource Integrity (SRI) hashes on external assets, and strict vendor security audits.
- **Applicable UK Legal Statute:** ISO 27001 Annex A.15 (Supplier Relationships), NCSC Supply Chain Security Guidance.


### NCSC 5-Stage Incident Response Plan
The BBC CSIRT follows the NCSC 5-phase Incident Response Framework: 1) Identification & Triage (24/7 SOC automated alerting); 2) Containment (network isolation, edge CDN blocking); 3) Eradication (root-cause patch deployment, credential revocation); 4) Recovery (phased service restoration with active synthetic monitoring); 5) Post-Incident Lessons Learned and statutory reporting to the Information Commissioner’s Office (ICO) within 72 hours where personal data is implicated.
