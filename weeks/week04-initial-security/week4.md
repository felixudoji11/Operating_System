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
