# Week 5 — Advanced Security & Monitoring Infrastructure (Phase 5)

## Week Overview

By Week 5, I felt significantly more confident working exclusively over SSH. After establishing a secure access baseline in Week 4, the focus shifted to **advanced server hardening and monitoring automation**. This week was the most security-focused phase so far, as it involved working with mandatory access control (MAC), intrusion detection, automated patching, and custom scripting.

Rather than only configuring services, I began **building administrative tools** that verify and monitor the system. This marked a transition from basic system configuration to more professional system administration practices.

---

## Key Implementations This Week

During this phase, I implemented and documented the following advanced security and monitoring components:

- Mandatory Access Control verification using **AppArmor** (or SELinux depending on distribution)
- Configuration of **automatic security updates**
- Deployment of **fail2ban** to protect against SSH brute-force attacks
- Development of a **security baseline verification script** (`security-baseline.sh`)
- Development of a **remote monitoring script** (`monitor-server.sh`)

---

## Reflection

The scripting work in particular made this week feel like a major step forward in my technical ability. Instead of manually checking configurations each time, I created scripts that could automatically verify security settings and collect performance data. This approach mirrors real-world system administration, where automation is essential for maintaining secure and reliable systems.

Week 5 effectively tied together the planning work from Week 2 and the practical hardening completed in Week 4. The system was no longer just secure—it was now **verifiable, monitorable, and maintainable**, setting a strong foundation for the security audit in Week 7 and the performance evaluation in Week 6.

---

## 5.1 Implementing Access Control (AppArmor)

The Ubuntu-based server used for this coursework relies on **AppArmor** as its mandatory access control (MAC) framework. Since AppArmor is enabled by default on Ubuntu, I focused on verifying its status and understanding how it enforces security policies at the application level.

---

### Step 1 — Checking AppArmor Status
```bash
sudo aa-status

```

## 5.1.1 Demonstrating AppArmor Profile Management

To better understand how AppArmor enforces security policies, I examined an active profile and reviewed system logs for access control events. This allowed me to observe how mandatory access control operates in practice.

---

### Viewing an AppArmor Profile
```bash
cat /etc/apparmor.d/usr.bin.sshd

```

## 5.2 Configure Automatic Security Updates

Keeping a server up to date with the latest security patches is a critical aspect of system hardening. To reduce reliance on manual maintenance, I configured **unattended security updates** on the server.

---

### Installing and Configuring Unattended Upgrades
```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades

```

## 5.3 Intrusion Detection with Fail2ban

Fail2ban provides an additional layer of protection by monitoring authentication logs for repeated failed login attempts and dynamically banning IP addresses that exhibit suspicious behaviour. This helps defend against brute-force attacks that bypass static firewall rules.

---

### Step 1 — Installing Fail2ban
```bash
sudo apt install fail2ban -y

```

### Step 2 — Creating Local Jail Configuration

To customise Fail2ban behaviour without modifying default configuration files, I created a local jail configuration file.

```bash
sudo nano /etc/fail2ban/jail.local

```

### Step 3 — Restarting Fail2ban

After configuring the local jail settings, I restarted the Fail2ban service to apply the changes.

```bash
sudo systemctl restart fail2ban

```

### Step 4 — Status Check

After restarting Fail2ban, I verified that the SSH jail was active and functioning as expected.

```bash
sudo fail2ban-client status sshd

```

## 5.4 Creating the `security-baseline.sh` Script

To verify that all core security controls implemented in **Week 4 and Week 5** were correctly configured, I created a security baseline verification script. This script provides a single command that audits the system’s critical security settings.

---

### Creating the Script
```bash
nano security-baseline.sh

```

## 5.5 Creating the `monitor-server.sh` Remote Monitoring Script

To support remote performance monitoring, I created a script that runs from the **workstation**, connects to the server via SSH, and collects key system metrics. This approach avoids repeated manual logins and demonstrates automated remote administration.

---

### Creating the Script
```bash
nano monitor-server.sh

```

