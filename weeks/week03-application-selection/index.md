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

### 3.3 Expected Resource Profiles

Before running any performance tests, I documented the expected behaviour of each selected application. Defining these expectations in advance provides a baseline for comparison with the actual results collected in Week 6 and supports structured analysis of operating system behaviour under different workloads.

---

### CPU-Intensive Workload (`stress-ng`)
**Expected behaviour:**
- CPU usage spikes to **90–100%**
- System load increases beyond the number of available CPU cores
- Minimal disk or memory usage unless explicitly configured

**Reasoning:**  
CPU stress workloads typically execute continuous computation loops, placing sustained pressure on the CPU scheduler with little interaction with memory or storage subsystems.

---

### Memory-Intensive Workload (`stress-ng`)
**Expected behaviour:**
- RAM usage increases according to the allocated amount (e.g. **1–3 GB**)
- Possible swap usage if memory pressure exceeds physical RAM
- Moderately elevated CPU usage due to memory allocation and management operations

---

### Disk I/O-Intensive Workload (`fio`)
**Expected behaviour:**
- High read and write throughput
- Significant disk latency variations under sustained load
- Increased **I/O wait** percentage visible in CPU statistics

---

### Network-Intensive Workload (`iperf3`)
**Expected behaviour:**
- High incoming or outgoing bandwidth depending on test direction
- Increased packet processing load
- Potential network saturation depending on virtual machine and host limitations

---

### Server / Service Workload (`nginx`)
**Expected behaviour:**
- Moderate CPU usage per request
- Increased memory usage during concurrency testing
- Low response times under baseline conditions
- Higher response times under heavy load
