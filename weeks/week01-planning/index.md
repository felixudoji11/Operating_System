# Week 1 — Phase 1: System Planning & Distribution Selection

## Overview
This week marked the beginning of my Operating Systems coursework. The primary objective was to plan and prepare the environment for deploying a secure, headless Linux server. I focused on designing a functional two-system architecture that supports remote administration entirely through SSH, as required by the assessment.

This journal entry documents the architectural decisions made, the reasoning behind them, and the technical considerations that shaped the foundation for all subsequent configuration, security, and performance testing work.

---

## 1. System Architecture Diagram

To begin, I mapped out the overall system architecture to clearly understand how the server and workstation would communicate. Since the coursework emphasises command-line proficiency and remote administration, I designed a dual-system setup using VirtualBox virtual machines.

### Architecture Overview

```text
┌────────────────────────┐        SSH        ┌──────────────────────────┐
│   Workstation System   │ ───────────────▶ │   Headless Linux Server   │
│   (Ubuntu Desktop VM)  │ ◀─────────────── │     (Ubuntu Server)       │
└────────────────────────┘     Return SSH    └──────────────────────────┘
           ▲
           │
           └──────── Host-Only + NAT Networking ────────┘


### 2. Distribution Selection Justification

Before deploying the server, I compared multiple Linux server distributions to determine which best met the technical and learning requirements of the coursework. The comparison focused on stability, security features, documentation quality, and suitability for a structured learning environment.

---

### Distribution Comparison

| Distribution | Pros | Cons | Suitability |
|---|---|---|---|
| **Ubuntu Server** | Large community support, excellent documentation, AppArmor enabled by default, stable LTS releases | Slightly larger package base | ⭐ Best balance for learning and stability |
| **CentOS Stream** | SELinux enforced by default, enterprise-focused tooling | Rolling-release model may introduce unexpected changes | Suitable, but less predictable |
| **Debian Server** | Extremely stable, minimal installation | Older package versions | Good for production, less ideal for learning |

---

### Final Choice: Ubuntu Server 22.04 LTS

After evaluating the available options, I selected **Ubuntu Server 22.04 LTS** as the server operating system.

#### Reasons for selection:
- Long-term support ensures fewer disruptive changes during the coursework lifecycle
- Native AppArmor integration supports mandatory access control requirements
- Straightforward package management using APT
- Extensive documentation and community resources
- Well-suited to a structured educational and experimental environment

---

### Reflection
I chose Ubuntu Server because it provides an ideal balance between stability and accessibility. Nearly all tools required for this module are well supported on Ubuntu, and the abundance of troubleshooting documentation reduces friction when diagnosing issues. This allows me to focus on understanding operating system behaviour and security concepts rather than distribution-specific complications.
