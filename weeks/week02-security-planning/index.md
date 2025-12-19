# WEEK 2 — Security Planning & Testing Methodology (Phase 2)
*(Personal, detailed, reflective technical journal)*

## Week Overview

This week, I focused heavily on planning the security baseline for my Linux server and developing a structured performance testing methodology that I would later implement in the performance weeks. Everything I produced here forms the foundation for the hands-on configuration work I would complete in Week 4 and beyond.

I intentionally approached this week as if I were an actual systems administrator planning a secure deployment. I wanted to show that I understand why each security measure matters — not just the commands. So I spent a lot of time researching SSH hardening, firewall strategies, access control models (AppArmor vs SELinux), and performance testing principles such as CPU profiling, load monitoring, and baseline vs stress testing.

This week was planning-focused, but my journal documents the exact CLI commands I tested on my server so that later weeks connect cleanly.

## 2.1 Performance Testing Plan
*(How I planned to measure performance, what metrics I chose, and why)*

To prepare for the performance-heavy workload of Week 6, I designed a structured approach to collecting and analysing performance data remotely via SSH. I wanted my strategy to mirror what real sysadmins and DevOps engineers do when testing servers.

### My Personal Reflection:
At first, I thought performance testing was just about running htop and watching numbers go up. But when I started reading documentation and guides, I realised proper performance evaluation requires a systematic plan — establishing baselines, applying controlled workloads, collecting metrics consistently, and analysing patterns. That shifted my mindset, so this section became my blueprint for all future testing.

### Remote Monitoring Methodology
Tools I decided to use (all via SSH):
sudo ufw allow from <workstation_ip> to any port 22 proto tcp
