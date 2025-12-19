# Week 2 — Security Planning & Testing Methodology (Phase 2)

## Week Overview
This week focused on designing the security baseline for my Linux server and developing a structured performance testing methodology that would later be implemented during the configuration and evaluation phases of the coursework. All work completed this week forms the foundation for the hands-on security implementation in Week 4 and the performance evaluation in Week 6.

I approached this phase as if planning a real production server deployment. Rather than focusing only on commands, I concentrated on understanding **why** each security control matters and how different layers of security interact. This involved researching SSH hardening practices, firewall strategies, access control models (AppArmor vs SELinux), and core performance testing principles such as baselining, controlled load generation, and metric analysis.

Although this week was primarily planning-focused, I tested and documented the relevant command-line tools via SSH to ensure a smooth transition into later implementation phases.

---

## 2.1 Performance Testing Plan

To prepare for the performance-intensive testing in Week 6, I designed a structured approach for collecting and analysing performance data remotely via SSH. The goal was to mirror how system administrators and DevOps engineers evaluate server performance in real environments.

### Personal Reflection
Initially, I assumed performance testing involved simply running tools like `htop` and observing changes in resource usage. However, after reviewing professional documentation and performance guides, I realised that meaningful evaluation requires a systematic methodology. This includes establishing baselines, applying controlled workloads, collecting metrics consistently, and analysing trends rather than isolated values. This section became the blueprint for all later performance testing.

---

### Remote Monitoring Methodology

All monitoring was planned to be performed remotely via SSH using lightweight command-line tools.

| Tool | Purpose | Reason for Selection |
|---|---|---|
| `top` / `htop` | Live CPU and memory monitoring | Provides immediate real-time feedback |
| `vmstat` | CPU, memory, and interrupts | Lightweight and suitable for repeated sampling |
| `iostat` | Disk throughput and I/O wait | Essential for analysing disk-intensive workloads |
| `ss` / `iftop` | Network monitoring | Useful for observing throughput and socket activity |
| `systemctl` | Service status under load | Helps assess service behaviour during stress |
| `dstat` | Multi-metric logging | Suitable for timestamped performance logging |

## 2.2 Security Configuration Checklist

This section represents the core planning output of Week 2. I created a comprehensive security checklist covering all mandatory security controls required by the coursework. Each control is accompanied by a justification to demonstrate understanding of *why* the measure is necessary, not just how it is implemented.

---

### SSH Hardening Plan

| Security Task | Planned Action | Why It Is Important |
|---|---|---|
| Disable password authentication | Set `PasswordAuthentication no` in `sshd_config` | Prevents brute-force password attacks |
| Enable key-based authentication | Generate SSH key pair and install public key | Keys are cryptographically stronger and cannot be brute-forced |
| Change default SSH port (optional) | Move from port 22 to a custom port | Reduces automated scan noise |
| Disable root login | Set `PermitRootLogin no` | Prevents attackers targeting the root account |
| Restrict SSH to workstation IP | Firewall rule allowing SSH only from workstation IP | Provides strong network-based access control |

---

### Firewall Configuration (UFW)

The firewall forms the first line of defence by controlling which traffic is allowed to reach the server.

#### Planned firewall actions:
- Set default policy to deny all incoming traffic
- Allow SSH access only from the workstation IP
- Allow all outgoing traffic
- Enable logging at a low verbosity level


### 2.3 Threat Model (Reflective Documentation)

This section documents the threat model developed for my server deployment. Rather than listing generic threats, I identified realistic risks specific to my configuration and mapped each threat to concrete mitigation strategies planned or implemented throughout the coursework.

The goal of this threat model is to demonstrate an understanding of how attackers might target the system and how layered security controls work together to reduce risk.

---

### Threat 1 — Brute-Force SSH Attacks

**Risk:**  
Attackers attempt to gain access by repeatedly guessing user credentials over SSH.

**Likelihood:**  
Very high. SSH is a common attack vector and is frequently targeted by automated bots and scanning tools.

**Mitigation strategies:**
- Disable password authentication and enforce key-based authentication
- Restrict SSH access to a single trusted workstation IP using firewall rules
- Disable root login over SSH to reduce the attack surface
- Enable `fail2ban` to automatically block repeated failed authentication attempts (planned for Week 5)

---

### Threat 2 — Unauthorised Service Access

**Risk:**  
Misconfigured or unnecessary services expose network ports, allowing unauthorised access or exploitation.

**Likelihood:**  
Medium to high, particularly during early configuration stages when services may be installed with default settings.

**Mitigation strategies:**
- Configure the firewall with a default-deny policy for incoming traffic
- Allow only explicitly required services and ports
- Perform regular port scanning from the workstation using tools such as `nmap`
- Apply AppArmor confinement to network-facing services to limit the impact of potential compromise

---

### Threat 3 — Privilege Escalation

**Risk:**  
An attacker exploits a vulnerable binary, misconfiguration, or weak permission model to gain elevated privileges.

**Likelihood:**  
Medium. Privilege escalation often follows initial access and can have severe consequences if not mitigated.

**Mitigation strategies:**
- Apply the principle of least privilege for all users and services
- Enforce AppArmor profiles to restrict application capabilities
- Keep the system up to date with regular security updates
- Use `sudo` only when required and avoid routine operation with elevated privileges
### Reflection
Developing this threat model helped me understand that security is not achieved through a single control, but through multiple overlapping layers. Each mitigation on its own reduces risk slightly, but when combined, they significantly limit an attacker’s ability to compromise the system. This model directly informed the security checklist in Week 2 and the practical hardening work carried out in Week 4 and Week 5.
