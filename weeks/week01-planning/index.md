# Week 1 — Phase 1: System Planning & Distribution Selection

## Overview
This week marked the beginning of my Operating Systems coursework. The primary objective was to plan and prepare the environment for deploying a secure, headless Linux server. I focused on designing a functional two-system architecture that supports remote administration entirely through SSH, as required by the assessment.

This journal entry documents the architectural decisions made, the reasoning behind them, and the technical considerations that shaped the foundation for all subsequent configuration, security, and performance testing work.

---

## 1. System Architecture Diagram

To begin, I mapped out the overall system architecture to clearly understand how the server and workstation would communicate. Since the coursework emphasises command-line proficiency and remote administration, I designed a dual-system setup using VirtualBox virtual machines.

### Architecture Overview

┌────────────────────────┐        SSH        ┌──────────────────────────┐
│   Workstation System   │ ───────────────▶ │   Headless Linux Server   │
│   (Ubuntu Desktop VM)  │ ◀─────────────── │     (Ubuntu Server)       │
└────────────────────────┘     Return SSH    └──────────────────────────┘
           ▲
           │
           └──────── Host-Only + NAT Networking ────────┘
---

### 2. Distribution Selection Justification

Before deploying the server, I compared multiple Linux server distributions to determine which best met the technical and learning requirements of the coursework. The comparison focused on stability, security features, documentation quality, and suitability for a structured learning environment.

---

### Distribution Comparison

| Distribution | Pros | Cons | Suitability |
|---|---|---|---|
| **Ubuntu Server** | Large community support, excellent documentation, AppArmor enabled by default, stable LTS releases | Slightly larger package base |  Best balance for learning and stability |
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
--- 
## 3. Workstation Configuration Decision

To support secure remote administration of the server, I evaluated three possible workstation configurations:

- **Option A:** Linux Desktop Virtual Machine  
- **Option B:** Host machine with SSH client  
- **Option C:** Hybrid approach (host + VM)

---

### Final Choice: Option A — Ubuntu Desktop VM

After evaluating all options, I selected **Option A: Ubuntu Desktop Virtual Machine** as my workstation environment.

#### Reasons for selection:
- Provides a full Linux command-line environment
- Avoids modifying SSH keys or configuration files on the host operating system
- Ensures consistency between workstation and server, simplifying troubleshooting
- Allows snapshot creation before major configuration changes
- Supports native Linux administration and monitoring tools

---

### Reflection
Choosing a Linux-based workstation significantly improved my workflow when administering the server. Using a consistent operating system on both the workstation and server made it easier to interpret command output and reduced unexpected compatibility issues. It also allowed seamless use of Linux-native tools such as `htop`, `nmap`, and `iftop`, which proved valuable throughout the coursework.
---
## 4. Network Configuration Documentation

To ensure secure and reliable communication between the workstation and the server, I configured VirtualBox networking to support SSH-only administration while minimising external exposure.

---

### Server VM Network Setup

The server virtual machine was configured with two network adapters:

- **Adapter 1: NAT**  
  Used to provide internet access for system updates and package installation.

- **Adapter 2: Host-Only Adapter**  
  Used exclusively for SSH communication with the workstation.

This configuration allows the server to receive updates while remaining isolated from external networks for administrative access.

---

### Workstation VM Network Setup

The workstation virtual machine was also configured with two network adapters:

- **Adapter 1: NAT**  
  Provides internet connectivity for documentation, tool installation, and updates.

- **Adapter 2: Host-Only Adapter**  
  Enables direct SSH communication with the server over a private network.

---

### Assigned IP Addresses

After booting both virtual machines, I used the `ip addr` command to verify network interfaces and assigned IP addresses.

#### Server output
```bash
ip addr
---
### 5. System Specification Documentation (CLI Output)

To establish a baseline understanding of the server environment, I used mandatory command-line tools to document the operating system, hardware resources, storage usage, and network configuration. All commands were executed directly on the server via the terminal.
---
### Kernel and System Information
```bash
uname -a
---
