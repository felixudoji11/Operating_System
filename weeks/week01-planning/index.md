# Week 1 – System Planning and Distribution Selection

## Overview
This week focuses on planning the operating system deployment for the CMPN202 coursework. The objective is to design a secure, headless Linux server environment administered exclusively via SSH from a separate workstation. Decisions made at this stage directly influence security posture, performance characteristics, and long-term maintainability.

This week addresses:
- System architecture design
- Linux distribution selection and justification
- Workstation choice and rationale
- Network configuration planning
- Baseline system specification collection using CLI tools

---

## System Architecture
The coursework uses a dual-system architecture consisting of:
- A **headless Linux server VM**
- A **separate workstation system** used for all administration via SSH

This design enforces command-line proficiency and mirrors real-world cloud and data-centre operational practices.

![System Architecture Diagram](architecture-diagram.png)

---

## Distribution Selection
A comparison of multiple Linux server distributions was conducted to justify the final choice. Key evaluation criteria included security support lifecycle, package availability, documentation quality, and community support.

See: [Distribution Comparison](distro-comparison-table.md)

---

## Workstation Configuration
The workstation acts as the sole administrative access point to the server. The configuration decision is justified based on security isolation, tooling availability, and professional relevance.

See: [Workstation Configuration Decision](network-configuration.md)

---

## Network Configuration
VirtualBox networking was planned to ensure:
- Isolated communication between server and workstation
- No exposure to external networks
- Full support for SSH-based remote administration

Details are documented in the network configuration section below.

---

## System Specification Evidence
Baseline system specifications were captured using standard Linux CLI tools. These metrics provide a reference point for later performance evaluation and optimisation.

See: [System Specifications](system-specs.txt)

---

## Reflection
This planning phase highlights the importance of architectural decisions in operating system deployment. Selecting a headless server and enforcing SSH-only access introduces early security constraints that influence later configuration, monitoring, and performance trade-offs.
