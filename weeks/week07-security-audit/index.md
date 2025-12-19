# Week 7 — Security Audit and System Evaluation

## Objective

The primary objective of Week 7 was to perform a comprehensive **security audit and system evaluation** of the deployed Linux server. This phase focused on validating that all security controls implemented in earlier weeks were functioning as intended, identifying any remaining vulnerabilities, and assessing whether further improvements were required.

This final evaluation ensures that the system is both **secure and efficient** before submission, reflecting a production-ready server rather than a partially configured environment.

---

## Tasks Completed

### Preparation for Security Audit

Before running any audit or evaluation activities, I carried out a structured preparation phase to ensure accuracy, safety, and completeness.

#### Configuration Review
I reviewed all major system configurations implemented in previous weeks, including:
- Network configuration and isolation settings
- Firewall rules (UFW)
- SSH hardening and authentication controls
- User and privilege management
- Mandatory access control (AppArmor)
- Intrusion detection and monitoring (Fail2ban)
- Automatic update configuration

This review ensured that the audit was conducted against the **intended final configuration** rather than an intermediate or inconsistent state.

---

#### Security Checklist Compilation

I compiled a checklist of potential security risks and verification points based on established security guidance, including:
- **OWASP Top 10** (for common attack vectors and misconfigurations)
- **CIS Benchmarks for Linux** (for baseline hardening and best practices)

This checklist helped structure the audit process and ensured that the evaluation covered both configuration-level and operational security concerns.

---

#### System Backup and Risk Mitigation

Before running any audit tools or performing deeper inspection, I ensured that all critical system data and configuration files were backed up. This precaution reduced the risk of accidental data loss or service disruption during testing.

Taking backups before auditing reflects real-world operational practice, where security assessments must not compromise system integrity.

---

### Reflection

Preparing for the security audit reinforced the importance of validation and verification in system administration. Security is not achieved simply by applying controls—it must be **reviewed, tested, and confirmed**. This preparation phase helped me approach the audit methodically and ensured that the final evaluation was both thorough and defensible.

## Vulnerability Scanning

As part of the security audit, I conducted multiple vulnerability scanning activities to evaluate the system’s network exposure, service configuration, and overall host security posture. Industry-standard tools were used to ensure a realistic and thorough assessment.

---

### Network Scanning with Nmap

I used **Nmap** to perform a network scan of the server in order to identify open ports and active services.

The scan results confirmed that:
- **SSH (port 22)** was open for remote administration
- **HTTP (port 80)** was open for the web server
- No unexpected ports or services were exposed

This indicated a **minimal attack surface**, consistent with the firewall and service hardening implemented in earlier weeks.

---

### Web Server Scanning with Nikto

I conducted a **Nikto** scan against the web server to identify potential web-related vulnerabilities and misconfigurations.

Findings included:
- Minor server configuration warnings
- Informational issues related to default headers

No critical or high-severity vulnerabilities were detected. The results suggested that the web service was reasonably hardened but could benefit from minor configuration improvements.

---

### Host Security Audit with Lynis

To evaluate the overall security posture of the Linux host, I ran a **Lynis** security audit.

Lynis recommendations included:
- Applying updates to a small number of outdated packages
- Enforcing stronger password policies
- Reviewing `sudo` privilege assignments
- Ensuring automatic security updates are enabled

Many of these recommendations aligned with controls already implemented in previous weeks, confirming that the system was largely compliant with best-practice hardening guidelines.

---

### Reflection

Running multiple vulnerability assessment tools provided confidence that the system was both **secure and well-configured**. The absence of unexpected open ports and critical vulnerabilities demonstrated the effectiveness of the layered security approach applied throughout the coursework. The remaining recommendations were incremental improvements rather than fundamental weaknesses, reinforcing that the server was in a strong security state approaching final submission.

## Manual Configuration Review

In addition to automated vulnerability scanning, I conducted a **manual review of critical system configurations**. This step was essential to validate security controls that automated tools may not fully assess and to ensure configurations aligned with best practices.

---

### SSH Configuration Review

I manually verified the SSH configuration to ensure secure remote access controls were correctly enforced. This included confirming that:
- Root login over SSH was disabled
- SSH key-based authentication was enabled
- Password-based authentication was disabled or restricted
- Strong authentication policies were in place for administrative access

These checks confirmed that SSH access was limited to authorised users and resistant to brute-force attacks.

---

### Firewall Configuration Review (UFW)

I reviewed the firewall configuration using **UFW** to ensure that network access was tightly controlled.

The review confirmed that:
- Only necessary ports (e.g. SSH and HTTP) were permitted
- All other incoming traffic was denied by default
- Firewall rules matched the intended network security design

This ensured that the server maintained a minimal and controlled attack surface.

---

### User Permissions and File Ownership Review

I reviewed user accounts, group memberships, and file ownership to verify that sensitive files and directories were not globally accessible.

This included checking that:
- Administrative privileges were restricted to authorised users
- Sensitive configuration files were owned by root or appropriate system users
- World-readable or world-writable permissions were not applied to critical files

---

### Reflection

Conducting a manual configuration review reinforced the importance of human verification alongside automated tools. While scanners are valuable for identifying common issues, manual inspection ensured that security controls were implemented exactly as intended and that no subtle misconfigurations were overlooked. This step provided additional confidence in the overall security posture of the system.

