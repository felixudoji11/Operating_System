# Week 4 — Initial System Configuration & Security Implementation

## Overview
Week 4 was the turning point in the project. Up to this point, most work involved planning and preparation. This week was the first time I implemented security controls on the live server. A key constraint increased the difficulty: **all configuration had to be performed remotely via SSH**.

This constraint made the risk of lockout very real. Misconfiguring the firewall or disabling password authentication incorrectly can immediately remove access. To manage this risk, I applied changes incrementally, validated after each change, and captured evidence throughout (including “before and after” configuration states), as required.

### Security controls implemented this week
- SSH key-based authentication
- SSH hardening via `sshd_config`
- Firewall (UFW) restricted to a single workstation IP
- Non-root administrative user creation
- Privilege management using `sudo`
- Verification and testing of remote access

---

## 4.1 SSH Key-Based Authentication Implementation

### Step 1 — Generate an SSH key pair (on workstation)
```bash
ssh-keygen -t ed25519 -C "adminuser-key"

```

### Step 2 — Copy the public key to the server
```bash
ssh-copy-id adminuser@192.168.56.101

```

## 4.2 SSH Hardening (`sshd_config` Modification)

To reduce the attack surface of the server, I hardened the SSH configuration by modifying the SSH daemon configuration file. All changes were carried out remotely via SSH, in accordance with the coursework requirement for headless administration.

---

### Editing the SSH Configuration File
```bash
sudo nano /etc/ssh/sshd_config

```

## 4.3 Firewall Configuration (UFW)

To control network access to the server and reduce the attack surface, I configured a firewall using **UFW (Uncomplicated Firewall)**. All configuration was performed remotely via SSH.

---

### Installing UFW
```bash
sudo apt install ufw -y

```

## 4.4 User Management & Privilege Controls

To reduce risk and enforce the principle of least privilege, I created a non-root administrative user and restricted the use of the root account. All user management tasks were performed remotely via SSH.

---

### Creating a Non-Root Administrative User
```bash
sudo adduser adminuser

```

## 4.5 SSH Access Evidence (Successful Login)

To verify that SSH key-based authentication and access restrictions were configured correctly, I tested remote access to the server from the workstation using the non-root administrative account.

---

### Screenshot Evidence (Placeholder)

> **Screenshot to be inserted here**  
> This screenshot shows successful SSH access using the command:
> ```bash
> ssh adminuser@192.168.56.101
> ```
> The login occurs without a password prompt, confirming that key-based authentication is functioning correctly.

---

### Expected Terminal Output
```text
Welcome to Ubuntu 22.04 LTS
Last login: Fri Oct 25 12:33:44
adminuser@server:~$

```

## 4.6 Configuration File Before/After Documentation

To provide clear evidence of the security hardening applied, I captured the relevant configuration files before and after modification. This demonstrates exactly what changes were made and supports traceability of security decisions.

---

### `/etc/ssh/sshd_config` — Before
```text
#PasswordAuthentication yes
#PermitRootLogin yes
#AllowUsers

```
## 4.7 Remote Administration Evidence

All administrative actions on the server were performed **strictly through SSH** from the workstation system. No local console access or graphical tools were used, in full compliance with the coursework requirements.

The following administrative tasks were carried out remotely:
- System updates
- Package installation and management
- User creation and privilege management
- Firewall configuration and verification
- Service management and restarts

---

### Remote Administration Command Examples

The commands below demonstrate typical administrative actions executed remotely over SSH.

```bash
ssh adminuser@192.168.56.101 "sudo apt update && sudo apt upgrade -y"

```

## Week 4 Summary

This week focused on the practical implementation of core security controls on the live server. By the end of the week, I had successfully achieved the following:

- ✔ Fully hardened SSH configuration
- ✔ Key-only authentication enforced
- ✔ Firewall restricted to a single trusted IP address
- ✔ Root login disabled
- ✔ Non-root administrative user created
- ✔ All configurations performed **100% over SSH**
- ✔ Clear before-and-after configuration documentation collected

---

## Final Reflection

Week 4 was one of the most technically challenging phases of the entire project. For the first time, I felt like I was administering a real production server. Every configuration change carried the risk of locking myself out, which forced me to work carefully, plan changes in advance, and document each step thoroughly.

These foundational security measures established a strong baseline for the system. They also set the stage for the advanced security hardening in Week 5 and the performance testing and analysis in Week 6.
