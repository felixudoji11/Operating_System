Week 1 — Phase 1: System Planning & Distribution Selection
Overview
This week marked the beginning of my operating systems coursework, where I planned and prepared the environment for deploying a secure, headless Linux server. My goal was to design a functional two-system architecture that supports remote administration entirely through SSH. This journal entry documents the decisions I made, the reasoning behind them, and the technical steps I took to build the foundation for the following weeks.

1. System Architecture Diagram
To begin, I mapped the entire setup to understand how the server and workstation would communicate. Since the assignment requires command-line proficiency and remote administration, I designed a dual-VM setup using VirtualBox.

The architecture I settled on:

┌────────────────────┐ SSH ┌────────────────────────┐
│ Workstation System │──────────────────▶│ Headless Linux Server │
│ (Ubuntu Desktop VM)│◀──────────────────│ (Ubuntu Server) │
└────────────────────┘ Return SSH └────────────────────────┘
▲
│
└────── Host-Only + NAT Networking ─────┘
Reflection: I chose this setup because having both systems as VMs avoids modifying my host OS and gives me full control over snapshots and network behaviour. Using host-only networking also prevents external machines from accessing my server.

2. Distribution Selection Justification
I compared three major server distributions before choosing Ubuntu Server.

Distribution	Pros	Cons	Suitabilit Ubuntu Server	Large community support, excellent documentation, AppArmor enabled by default, stable LTS releases	Slightly larger package base	Best balance for learning + stability
CentOS Stream	SELinux enforced by default, enterprise-focused	Rolling-release model can introduce changes unexpectedly	Good, but not ideal for this coursework
Debian Server	Very stable, minimal	Older packages	Good for production, less ideal for learning new tools
Final Choice: Ubuntu Server 22.04 LTS
Reasons:

Long-term support means fewer breaking changes.

Native AppArmor integration for mandatory access control.

Easy package management with APT.

Perfect for a structured learning environment.

Reflection: I chose Ubuntu Server because almost every tool needed in this module is well-supported, and there is extensive troubleshooting documentation available.

3. Workstation Configuration Decision
I had three options for the workstation:

A. Linux Desktop VM

B. My host machine with SSH client

C. A hybrid approach

 Final Choice: Option A — Ubuntu Desktop VM
Reasons:

Full Linux CLI environment.

Avoids altering SSH keys or configs on my host OS.

Consistency between workstation and server improves troubleshooting.

Easier to snapshot before major changes.

Reflection: Choosing a Linux workstation improved my terminal workflow and made it easier to use Linux-native tools such as htop, nmap, and iftop.

4. Network Configuration Documentation
To ensure both systems could communicate securely, I configured VirtualBox networking.

Server VM Network Setup
Adapter 1: NAT (for internet access & package installation)

Adapter 2: Host-Only Adapter (for SSH communication with the workstation)

 Workstation VM Network Setup
Adapter 1: NAT

Adapter 2: Host-Only Adapter

Assigned IP Addresses
After booting both machines, I ran ip addr to collect their network details.

Server output:

$ ip addr
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
inet 192.168.56.101/24
Workstation output:

$ ip addr
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
inet 192.168.56.1/24
Result
 Both systems are now on the same host-only network
 Server reachable only from my workstation
 Suitable for SSH-only administration

Reflection: This setup helped me understand the isolation benefits of host-only networks and how they reduce attack surface compared to bridged networking.

5. System Specification Documentation (CLI Output)
I used mandatory CLI tools to document hardware and OS details.

 uname -a
Linux server 5.15.0-94-generic #104-Ubuntu SMP x86_64 GNU/Linux
 free -h
total used free
Mem: 2048M 350M 1600M
Swap: 2048M 0M 2048M
 df -h
Filesystem Size Used Avail Use%
/dev/sda1 20G 2.1G 17G 11%
 lsb_release -a
Distributor ID: Ubuntu
Description: Ubuntu 22.04.3 LTS
 Server network info
$ ip addr show enp0s8
inet 192.168.56.101/24
Reflection: Running these commands gave me a clearer picture of my VM resource usage and helped confirm that no graphical interface was installed, keeping the system minimal and server-oriented.

Summary of Week 1
This week focused on planning and setting the foundation for the entire project. I gained a better understanding of:

Virtual machine networking

SSH-focused architecture design

Differences between server distributions

Documenting system information using Linux CLI tools

The decisions made here shaped every phase that follows, especially regarding security and remote-only administration. Week 2 will build on this foundation by planning security policies and defining a performance testing approach.
