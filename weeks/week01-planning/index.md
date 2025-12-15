# Week 1 – System Planning and Distribution Selection

## Overview
This week focuses on planning the operating system deployment for the CMPN202 Operating Systems coursework. The primary objective is to design a secure, headless Linux server environment that is administered exclusively via Secure Shell (SSH) from a separate workstation system. 

Early architectural and configuration decisions made during this phase directly influence the system’s security posture, performance characteristics, scalability, and long-term maintainability.

The activities undertaken this week include:
- System architecture design
- Linux distribution selection and technical justification
- Workstation choice and rationale
- Network configuration planning
- Baseline system specification collection using command-line tools

---

## System Architecture
The coursework adopts a dual-system architecture consisting of:
- A **headless Linux server virtual machine**
- A **separate workstation system** used exclusively for remote administration via SSH

All system administration tasks are performed remotely using command-line tools. This approach enforces command-line proficiency and closely mirrors professional cloud and data-centre operational practices, where servers are rarely accessed locally or via graphical interfaces.

![System Architecture Diagram](../../diagrams/architecture-diagram.png)


---

## Distribution Selection
A comparative analysis of multiple Linux server distributions was conducted to justify the final distribution choice. The evaluation focused on the following criteria:
- Length and reliability of security support lifecycle
- Stability and suitability for server environments
- Package management ecosystem
- Availability and quality of official documentation
- Strength of community and industry adoption

Further details and the full comparison are provided here:  
[Distribution Comparison](distro-comparison-table.md)

---

## Workstation Configuration
The workstation system functions as the sole administrative access point to the server. This design decision supports strong secur
