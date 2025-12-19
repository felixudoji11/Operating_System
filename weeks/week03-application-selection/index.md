# Week 3 — Application Selection for Performance Testing (Phase 3)

## Week Overview
This week focused on selecting the applications and workload types that will be used later during performance evaluation (Week 6). The goal was to build a realistic workload set that covers **CPU, memory, disk I/O, network**, and a **real server/service** scenario. This ensures the Week 6 tests produce meaningful, comparable results across distinct resource pressures.

### Personal Reflection
Initially, I considered selecting applications quickly, but after reviewing how performance testing is approached in enterprise environments, it became clear that an unbalanced workload set would produce shallow and unstructured findings. Each application needed to represent a **distinct resource profile** so that measurements could be mapped cleanly to OS behaviour (scheduling, memory management, disk I/O, and network stack performance).

To keep the project practical, I researched open-source tools that were:
- Lightweight enough to run inside a VM
- Mature and widely used (benchmark credibility)
- Suitable for headless use via SSH
- Capable of repeatable and controlled load generation

I also practiced installing and validating each tool via SSH to confirm it behaved reliably in a headless server environment.

---

## 3.1 Application Selection Matrix (Workload Types with Justification)

The following matrix defines the final workload tools selected for Week 6 performance testing.

| Workload Type | Application | Why I Chose It | Expected Impact |
|---|---|---|---|
| CPU-intensive | `stress-ng` | Purpose-built for CPU stress and benchmarking with controllable load profiles | Sustained high CPU usage, elevated system load, clear stress patterns |
| Memory-intensive | `stress-ng` (memory mode) | Supports memory allocation and pressure testing using the same toolchain | RAM saturation, possible swap usage, observable memory pressure effects |
| Disk I/O-intensive | `fio` | Industry-standard disk I/O benchmarking for throughput and latency analysis | Increased read/write throughput, higher I/O wait, latency variability |
| Network-intensive | `iperf3` | Reliable, measurable network throughput testing between systems | High bandwidth utilisation, packet processing load, possible saturation |
| Server/application workload | `nginx` | Realistic service workload reflecting typical server behaviour | Concurrency behaviour, response-time degradation under load, resource growth |

### Reflection
Creating this matrix became one of the most valuable planning outputs so far. It made the later testing design more structured because each tool has a defined purpose and an expected resource footprint. This also makes the Week 6 analysis easier to justify, because results can be linked back to specific OS subsystems and performance trade-offs.

---

## 3.2 Installation Documentation (SSH-Based Installations)

All application installations were performed **exclusively via SSH** from the workstation system. This maintained the required headless-server configuration and ensured that all deployment tasks were carried out using command-line tools only.

The following sections document the installation commands used and the technical rationale for selecting each application.

---

### 3.2.1 Installing `stress-ng`
```bash
sudo apt update
sudo apt install stress-ng -y
---
### 3.2.2 Installing `fio`
```bash
sudo apt install fio -y
