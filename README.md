# MyInternshipProjects

Project reports from my **Cyber Security Internship at Extion Infotech (2024)**.

The repository has two reports. One is a hands-on **vulnerability assessment** with Tenable Nessus. The other is a **data breach investigation** case study of a fictional financial institution, *ABC Secure Bank*. Both reports cover the full workflow: setup, analysis, findings, remediation and lessons learned.

---

## 📑 Table of Contents

- [Repository Contents](#-repository-contents)
- [Project 1 – Identifying and Mitigating Network Vulnerabilities using Nessus](#-project-1--identifying-and-mitigating-network-vulnerabilities-using-nessus)
  - [Objective](#objective)
  - [About Nessus](#about-nessus)
  - [Lab Setup](#lab-setup)
  - [Scan Configuration Workflow](#scan-configuration-workflow)
  - [Scan Summary](#scan-summary)
  - [Top 5 Vulnerabilities and Mitigations](#top-5-vulnerabilities-and-mitigations)
  - [Outcome and Future Work](#outcome-and-future-work)
- [Project 2 – Investigation of a Data Breach at ABC Secure Bank](#-project-2--investigation-of-a-data-breach-at-abc-secure-bank)
  - [Objective](#objective-1)
  - [Scenario](#scenario)
  - [Incident Analysis](#1-incident-analysis)
  - [Forensic Analysis](#2-forensic-analysis)
  - [Attack Timeline](#attack-timeline)
  - [Data Recovery Strategy](#3-data-recovery-strategy)
  - [Communication and Notification Plan](#4-communication-and-notification-plan)
  - [Post-Incident Review](#5-post-incident-review)
  - [Key Lessons](#key-lessons)
- [Skills Demonstrated](#-skills-demonstrated)
- [Tools and Technologies](#-tools-and-technologies)
- [Disclaimer](#%EF%B8%8F-disclaimer)
- [About the Author](#-about-the-author)
- [License](#-license)

---

## 📂 Repository Contents

| File | Description |
|------|-------------|
| [`Project Report on Identifying and Mitigating Network Vulnerabilities1.pdf`](Project%20Report%20on%20Identifying%20and%20Mitigating%20Network%20Vulnerabilities1.pdf) | Vulnerability assessment of a Metasploitable target using Nessus Essentials, with a mitigation plan for the top 5 critical findings (18 pages). |
| [`Project Report on Investigation of a Data Breach ABC Secure Bank.pdf`](Project%20Report%20on%20Investigation%20of%20a%20Data%20Breach%20ABC%20Secure%20Bank.pdf) | Incident response and digital forensics case study covering breach analysis, recovery, communication and post-incident review (17 pages). |
| [`LICENSE`](LICENSE) | MIT License. |
| `README.md` | This file. |

---

## 🛡 Project 1 – Identifying and Mitigating Network Vulnerabilities using Nessus

### Objective

Use **Tenable Nessus** to find and remediate network vulnerabilities, covering the whole process: environment setup, vulnerability scanning, result analysis and mitigation.

**Scan goals:**
- Identify **at least five** vulnerabilities in the target network.
- Produce a detailed report for each vulnerability.
- Give actionable recommendations to fix each one.

**Why it matters:** Network vulnerabilities can lead to data breaches, unauthorized access and denial-of-service attacks. Finding them early is key to keeping infrastructure secure.

### About Nessus

- **History:** Renaud Deraison created Nessus in **1998** as an open-source project. **Tenable, Inc.** took it commercial in **2005**. A free edition, **Nessus Essentials**, is still available for personal and educational use.
- **What it does:** It scans systems, networks and applications for known security weaknesses before attackers can exploit them.

**Key features**

| Feature | Description |
|---------|-------------|
| Vulnerability Scanning | Scans OSes, applications and network devices against a continuously updated vulnerability database. |
| Extensive Plugin Library | Regularly updated plugins, each checking for a specific vulnerability or misconfiguration. |
| Customizable Scan Policies | Lets you set scan parameters, plugins and settings to fit specific needs or compliance requirements. |
| Detailed Reporting | Reports give severity, impact and remediation guidance for each finding. |
| Integration & Automation | Works with SIEMs, ticketing systems and network tools, and has an API for automation. |
| User-Friendly Interface | A web UI for setting up scans, reviewing results and exporting reports. |

**Advantages:** broad detection coverage, ease of use, customizability, integration and automation, low cost, and regular updates with vendor support.

### Lab Setup

1. **Download** Nessus for your OS (Windows / Linux / macOS) from the Tenable website.
2. **Install** it with the OS installer or package manager.
3. **Open the web interface** at `https://localhost:8834` (or the server's IP).
4. **Create an administrator account** with a strong password.
5. **Activate the license**: choose **Nessus Essentials** for non-commercial use or enter a license key.
6. **Update plugins**: Nessus downloads them automatically after setup, and you can also trigger updates from the UI.

### Scan Configuration Workflow

```
Create Policy ─► Define Targets ─► Apply Policy ─► Run Scan ─► Review Results ─► Remediate ─► Rescan
```

- **Policies:** Choose the scan type (e.g. Basic Network Scan, Web App Scan) and the plugins, then set schedules, credentials and advanced options.
- **Scans:** Enter the target IPs or hostnames, attach the policy and launch the scan.
- **Reports:** Review findings by severity and impact, then apply the recommended fixes.

### Scan Summary

| Field | Value |
|-------|-------|
| Scan Date | 2024-12-30 (17:18:47 – 17:34:48) |
| Target | `192.168.211.128` (Metasploitable) |
| Host OS | Linux Kernel 2.6 on Ubuntu 8.04 (Hardy) |
| Scan Type | Basic Network Vulnerability Scan |
| Plugin Set | Standard |

**Findings by severity**

| 🟥 Critical | 🟧 High | 🟨 Medium | 🟩 Low | 🟦 Info |
|:-----------:|:-------:|:---------:|:------:|:-------:|
| **8** | **5** | **23** | **5** | **115** |

### Top 5 Vulnerabilities and Mitigations

| # | Plugin ID | Vulnerability | CVE | CVSS | Severity |
|---|-----------|---------------|-----|------|----------|
| 1 | 134862 | Apache Tomcat AJP Connector Request Injection (**Ghostcat**) | CVE-2020-1938 | v3: 9.8 / v2: 7.5 | Critical |
| 2 | 32314 | Debian OpenSSH/OpenSSL Package Random Number Generator Weakness | CVE-2008-0166 | v2: 10.0 | Critical |
| 3 | 20007 | SSL Version 2 and 3 Protocol Detection | — | v3: 9.8 / v2: 10.0 | Critical |
| 4 | 46882 | UnrealIRCd Backdoor Detection | — | v2: 10.0 | Critical |
| 5 | 61708 | VNC Server `'password'` Password | — | v2: 10.0 | Critical |

<details>
<summary><b>1. Apache Tomcat AJP Connector Request Injection (Ghostcat), Plugin 134862</b></summary>

**Issue:** The Apache JServ Protocol (AJP) connector is vulnerable. A remote, unauthenticated attacker can read sensitive web application files and, if file upload is enabled, upload malicious JSP code to achieve **remote code execution**.

**Impact:** Compromises confidentiality, integrity and availability.

**Mitigation:**
1. Upgrade Tomcat to **7.0.100 / 8.5.51 / 9.0.31** or later, then restart.
2. Secure the AJP connector in `server.xml`:
   ```xml
   <Connector protocol="AJP/1.3" address="127.0.0.1"
              secretRequired="true" secret="StrongSecret" port="8009" />
   ```
3. Disable it if it isn't used:
   ```xml
   <!-- <Connector protocol="AJP/1.3" port="8009" /> -->
   ```
4. Block external access to port **8009** at the firewall.
5. Rescan to verify the fix.

</details>

<details>
<summary><b>2. Debian OpenSSH/OpenSSL RNG Weakness, Plugin 32314</b></summary>

**Issue:** The SSH host keys were generated on a Debian/Ubuntu system whose patched OpenSSL had almost no entropy, so the keys are **predictable**.

**Impact:** An attacker can recover private keys, decrypt traffic and perform man-in-the-middle attacks.

**Mitigation:**
1. Upgrade OpenSSL to an unaffected version.
2. Treat all SSH, SSL and OpenVPN keys from the host as compromised and regenerate them:
   ```bash
   rm -f /etc/ssh/ssh_host_*
   dpkg-reconfigure openssh-server
   ```
3. Distribute the new public keys to dependent systems and users.
4. Review the system's cryptographic configuration.

**Risk:** CVSS v2 Base 10.0, Temporal 8.3. Reference: BID 29179.

</details>

<details>
<summary><b>3. SSL Version 2 and 3 Protocol Detection, Plugin 20007</b></summary>

**Issue:** The service accepts SSL 2.0 and/or SSL 3.0. Both have insecure CBC padding and weak renegotiation and resumption, and are open to downgrade attacks such as **POODLE**. NIST considers SSL 3.0 inadequate, and PCI DSS v3.1 no longer counts SSL as strong cryptography.

**Impact:** Man-in-the-middle attacks and decryption of sensitive traffic.

**Mitigation:**
1. Disable SSL 2.0 and 3.0 in the service configuration.
2. Require **TLS 1.2 or higher** with secure cipher suites.
3. Follow the application's documentation for the exact configuration steps.

**References:** RFC 7507 (POODLE mitigation), RFC 7568 (deprecating SSL 3.0).

</details>

<details>
<summary><b>4. UnrealIRCd Backdoor Detection, Plugin 46882</b></summary>

**Issue:** The IRC server runs a trojaned UnrealIRCd build with a **backdoor** that allows arbitrary code execution.

**Exploitability:** Public exploits exist in Metasploit and CANVAS.

**Mitigation:**
1. Download UnrealIRCd again from the official source.
2. Check the download against the published MD5/SHA1 checksums.
3. Reinstall from the verified package.

**Risk:** CVSS v2 Base 10.0, Temporal 8.3.

</details>

<details>
<summary><b>5. VNC Server 'password' Password, Plugin 61708</b></summary>

**Issue:** The VNC server accepts the default password `password`. Nessus logged in with it.

**Impact:** A remote, unauthenticated attacker can take full control of the system.

**Mitigation:** Set a strong, complex VNC password and enforce a strong password policy.

**Risk:** CVSS v2 Base 10.0.

</details>

### Outcome and Future Work

- **Outcome:** The scan found and analyzed several critical vulnerabilities, giving a clear view of the host's security posture and a prioritized remediation plan.
- **Future work:** Schedule regular vulnerability assessments, monitor for new threats continuously, and keep strengthening the network's security.

---

## 🏦 Project 2 – Investigation of a Data Breach at ABC Secure Bank

### Objective

Investigate a data breach at **ABC Secure Bank**, a fictional, highly trusted financial institution. The investigation covers attack methods, the extent of the damage, forensic analysis, data recovery, regulatory compliance, stakeholder communication and the post-incident review. The scenario tests the forensic and investigative skills needed to handle a major breach.

### Scenario

| Item | Detail |
|------|--------|
| Organization | ABC Secure Bank (fictional) |
| Discovery | Routine security audit found anomalies in customer database access logs |
| Data exposed | Customer names, account numbers, transaction histories (millions of records) |
| Dwell time | About **3 months** undetected |
| Goal | Find the root cause, assess the damage, recommend remediation, and ensure regulatory compliance |

### 1. Incident Analysis

- **Identification:** A routine audit flagged unusual access to customer databases. The breach had likely been going on for a long time, which exposed gaps in monitoring and alerting.
- **Initial response:** The incident response plan was activated, the IR team contained the breach and assessed the damage, and customers, regulators and law enforcement were contacted.
- **Point of entry:** **Phishing emails** made to look like internal communications led employees to a compromised website that installed malware, giving attackers remote access and a foothold to escalate privileges.
- **Attack vector:** Attackers exploited an **unpatched web application** (a fix was available but not applied) with **SQL injection** to read customer data.
- **Impact:** Stolen data was likely sold on dark web marketplaces or used for fraud.

### 2. Forensic Analysis

| Step | Findings |
|------|----------|
| **Evidence collection** | Forensic disk images of the compromised systems were made to preserve evidence, including deleted and hidden data. |
| **Malware analysis** | A **Remote Access Trojan (RAT)** built to evade antivirus, talking to an overseas **command-and-control (C2)** server. It was used for exfiltration, privilege escalation and lateral movement. |
| **Log analysis** | Firewall logs, server access logs and IDS alerts showed: initial access → lateral movement → exploitation → exfiltration. The attackers had **deleted log entries**, which were recovered with specialized tools. |

### Attack Timeline

| When | Event |
|------|-------|
| Day 1 | Phishing emails sent to employees |
| Day 2 | An employee clicks a malicious link and the RAT is installed |
| Day 5 | Attackers reach the internal network and escalate privileges |
| Day 7 | Web application vulnerability exploited (SQLi), giving database access |
| Day 10 – Month 3 | Customer data steadily exfiltrated |
| Month 4 | Breach detected during a routine security audit |

### 3. Data Recovery Strategy

**Containment**
- Isolate compromised systems from the network.
- Patch the exploited web application.
- Scan the network for remaining malware.
- Enforce **multi-factor authentication (MFA)** and add advanced monitoring.

**Communication**
- Notify affected customers, work with law enforcement, and report to regulators.

**Customer support**
- Account monitoring, identity theft protection (credit monitoring, fraud alerts), and reimbursement of unauthorized transactions.

**Legal and regulatory compliance**
- Legal review of liability, compliance with breach notification laws such as **GDPR** and **CCPA**, and cooperation with regulators.

**Long-term mitigation**
- Employee security awareness training
- More frequent and deeper security audits
- **SIEM** and **EDR** deployment
- Third-party / vendor risk management
- Encryption of sensitive data **at rest and in transit**

### 4. Communication and Notification Plan

| Audience | Channel | Key content |
|----------|---------|-------------|
| **Customers** | Email and physical letters | Incident summary, what data was exposed, recommended actions (monitor accounts, enable fraud alerts, secure personal info) |
| **Stakeholders** (executives, board, investors) | Meetings | Business impact, containment actions, long-term prevention measures |
| **Regulatory bodies** | Formal incident report | Full timeline, containment and remediation steps, security improvements |

### 5. Post-Incident Review

**Weaknesses identified**
1. **Insufficient employee training:** staff couldn't recognize phishing.
2. **Inadequate network segmentation:** attackers could move laterally.
3. **Limited monitoring:** suspicious activity went unnoticed for months.

**Recommendations**

| Area | Actions |
|------|---------|
| Employee Training | Regular organization-wide training, a focus on phishing and social engineering, and simulated phishing exercises |
| Network Segmentation | Isolate sensitive systems, use role-based access control, and review permissions regularly (least privilege) |
| Enhanced Monitoring | Deploy IDS and SIEM, set up automated alerts for anomalies, and keep a dedicated monitoring team |
| Regular Audits | More frequent audits, vulnerability assessments and penetration testing, and independent third-party audits |

### Key Lessons

The breach began with **phishing** and was made worse by an **unpatched, known vulnerability**. It shows the value of:
- regular security audits and timely patching,
- continuous employee awareness training,
- advanced threat detection (SIEM, EDR, IDS),
- a tested incident response and communication plan.

---

## 🎯 Skills Demonstrated

- Vulnerability assessment and management
- Risk scoring with CVSS v2 / v3
- Remediation planning and system hardening
- Incident response (identification, containment, eradication, recovery)
- Digital forensics (disk imaging, malware analysis, log analysis, timeline reconstruction)
- Regulatory awareness (GDPR, CCPA, PCI DSS)
- Security communication and technical report writing

## 🧰 Tools and Technologies

| Category | Tools / Concepts |
|----------|------------------|
| Vulnerability Scanning | Tenable Nessus Essentials |
| Lab Environment | Metasploitable (intentionally vulnerable Linux VM) |
| Exploitation Frameworks (referenced) | Metasploit, CANVAS |
| Detection and Monitoring | IDS, SIEM, EDR, firewall logs |
| Attack Techniques Studied | Phishing, RAT / C2, SQL Injection, privilege escalation, lateral movement |
| Protocols and Services | Apache Tomcat (AJP), OpenSSH/OpenSSL, SSL/TLS, UnrealIRCd, VNC |

---

## ⚠️ Disclaimer

These projects were done for **educational purposes** during an internship.
- The Nessus scan targeted **Metasploitable**, an intentionally vulnerable VM, in an isolated lab network that I own.
- **ABC Secure Bank is fictional.** The breach scenario is a simulated case study.

Only scan or test systems you own or have explicit written permission to assess.

---

## 👤 About the Author

**Archit Yadav**, B.Tech in Electronics and Communication Engineering, Pranveer Singh Institute of Technology.

- Experienced with C++ and Python
- Holds a design patent for a **Free Space Optics (FSO)** project, and won ₹10,000 at the college Tech-Xpo
- Research: *"Asymptotic Analysis of Kappa-Mu Shadowed Model with Application to Error Probability"* (verified with MATLAB simulations)
- Interested in ethical hacking and cybersecurity since class 10, starting with Kali Linux and Termux

**Connect:**
[LinkedIn](https://www.linkedin.com/in/archit-yadav-42491321a) ·
[LeetCode](https://leetcode.com/u/ArchitYadav) ·
[Instagram](https://www.instagram.com/yaduvanshi_archit)

---

## 📄 License

This repository is licensed under the [MIT License](LICENSE). © 2025 Archit Yadav
