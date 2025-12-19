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

#### Example commands planned for implementation:
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from <workstation_ip> to any port 22 proto tcp
sudo ufw enable
