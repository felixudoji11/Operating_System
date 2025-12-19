WEEK 2 — Security Planning & Testing Methodology (Phase 2)
(Personal, detailed, reflective technical journal)
Week Overview
This week, I focused heavily on planning the security baseline for my Linux server and developing a structured performance testing methodology that I would later implement in the performance weeks. Everything I produced here forms the foundation for the hands-on configuration work I would complete in Week 4 and beyond.

I intentionally approached this week as if I were an actual systems administrator planning a secure deployment. I wanted to show that I understand why each security measure matters — not just the commands. So I spent a lot of time researching SSH hardening, firewall strategies, access control models (AppArmor vs SELinux), and performance testing principles such as CPU profiling, load monitoring, and baseline vs stress testing.

This week was planning-focused, but my journal documents the exact CLI commands I tested on my server so that later weeks connect cleanly.

2.1 Performance Testing Plan
(How I planned to measure performance, what metrics I chose, and why)
To prepare for the performance-heavy workload of Week 6, I designed a structured approach to collecting and analysing performance data remotely via SSH. I wanted my strategy to mirror what real sysadmins and DevOps engineers do when testing servers.

My Personal Reflection:
At first, I thought performance testing was just about running htop and watching numbers go up. But when I started reading documentation and guides, I realised proper performance evaluation requires a systematic plan — establishing baselines, applying controlled workloads, collecting metrics consistently, and analysing patterns. That shifted my mindset, so this section became my blueprint for all future testing.

Remote Monitoring Methodology
Tools I decided to use (all via SSH):

Tool	Purpose	Why I Chose It
top / htop	Live CPU & memory monitoring	Gives immediate real-time feedback
vmstat	CPU, memory, interrupts	Lightweight, good for logging
iostat	Disk throughput & I/O wait	Essential for I/O-intensive workloads
ss & iftop	Network monitoring	Helps track throughput under load
systemctl	Service behaviour under stress	Useful for server workloads
dstat	Multi-metric logging	Ideal for collecting timestamped logs
I recorded example commands I tested:

top
vmstat 2 10
iostat -x 2 10
dstat -tcmnd
These test executions helped me verify that all tools worked through SSH and that my server responded fast enough to maintain reliable monitoring.

Testing Approach
I designed a multi-stage testing methodology:

1. Baseline Testing
measures the system with no significant workload

I collected CPU idle %, memory usage, disk throughput, and network idle traffic

2. Application Load Testing
Each application selected in Week 3 will be tested with controlled workloads

Examples: stress tests, high network throughput tests, file copy loops, etc.

3. Bottleneck Identification
I planned to look for bottlenecks such as:

High CPU load (load > core count)

High memory pressure (swapping)

Disk I/O wait times

High network latency or dropped packets

4. Optimisation Testing
I also planned at least two improvements:

Adjusting kernel parameters (sysctl)

Improving service configurations (e.g., nginx worker processes)

Changing I/O scheduler

Updating firewall rules for performance

2.2 Security Configuration Checklist
(The plan for every security measure I intended to apply — with explanations)
This was the core of my Week 2 work. I built a full checklist covering all mandatory security areas and explained why each one matters, which is required for high marks.

SSH Hardening
Security Task	Planned Action	Why It’s Important
Disable password authentication	Set PasswordAuthentication no in sshd_config	Prevents brute-force password attacks
Enable key-based authentication	Generate SSH keys and install public key	Keys are far stronger and cannot be brute-forced
Change default port (optional)	Switch from 22 → custom port	Reduces noise from automated scans
Disable root login	PermitRootLogin no	Stops attackers targeting the root account
Restrict SSH to workstation IP	Firewall rule allowing SSH only from my workstation	Strong network-based access control
Firewall Configuration (UFW)
I planned to:

Set default policies to deny incoming

Allow SSH only from my workstation

Allow outgoing traffic

Enable logging (low level)

Example commands I intended to use later:

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from <workstation_ip> to any port 22 proto tcp
sudo ufw enable
Why it matters:
A firewall drastically reduces the attack surface. With SSH restricted to one IP, the server becomes extremely hard to probe or brute-force.

Mandatory Access Control (AppArmor or SELinux)
I evaluated both:

Feature	AppArmor	SELinux
Complexity	Easier	More complex
Default on Ubuntu	Yes	No
Control granularity	Medium	Very high
Learning curve	Low	High
My choice: AppArmor
Because:

Ubuntu uses it by default

Easier to audit and customise

Sufficient for my server’s workload

Planned commands:

aa-status
sudo aa-enforce /etc/apparmor.d/*
Automatic Updates
I planned to enable unattended security updates:

sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
Why: keeps the system safe even if I forget to update manually.

User & Privilege Management
Planned actions:

Create non-root admin user

Add to sudo group

Disable root login

Why: Minimises risk of accidental system damage and brute-force against root.

Network Security
I planned:

nmap scanning from workstation weekly

Logging SSH attempts

Enforcing secure sysctl settings (Week 4–5)

Monitoring open ports (ss -tulnp)

This ensures continuous awareness of my system’s exposure.

2.3 Threat Model (Reflective Documentation)
Here I identified real threats to my specific setup and matched each with a mitigation strategy.

Threat 1 — Brute Force SSH Attacks
Risk: Attackers attempt to guess user passwords
Likelihood: Very high
Mitigations:

Disable password authentication

Restrict IP-based SSH access

Enable fail2ban (week 5)

Disable root login

Threat 2 — Unauthorized Service Access
Risk: Misconfigured services exposing ports
Mitigations:

UFW firewall default deny

Regular port scanning from workstation

AppArmor confinement of services

Threat 3 — Privilege Escalation
Risk: Attacker exploits a vulnerable binary or misconfiguration
Mitigations:

Principle of least privilege

AppArmor enforced profiles

Regular security updates

Using sudo only when needed

WEEK 2 REFLECTION
By the end of Week 2, I felt much more confident about what I needed to implement in later weeks. Planning everything before touching configurations helped me avoid messy mistakes later. I also realised how layered security must be — firewall alone isn’t enough, SSH hardening alone isn’t enough, and updates alone aren’t enough. The checklist I created here became my roadmap for Week 4 and 5. This week was mostly planning on paper and testing basic commands, but it set the stage for a very secure overall deployment.
