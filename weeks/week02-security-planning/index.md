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

#### Example commands tested
```bash
top
vmstat 2 10
iostat -x 2 10
dstat -tcmnd
