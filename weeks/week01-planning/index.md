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
