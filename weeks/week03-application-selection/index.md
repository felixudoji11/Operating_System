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
Why: Running a real service lets me analyse server response times, concurrency behaviour, and resource usage under load.

## 3.3 Expected Resource Profiles

Before running any tests, I wrote out what I expected each application to do. This helped me compare the predicted behaviour with actual Week 6 results.

### CPU-Intensive (stress-ng)
**Expected:**

- CPU usage spikes to 90–100%
- System load increases above number of available cores
- Minimal disk or memory usage unless configured

**Reasoning:** CPU stress loops typically execute pure computation operations.

### Memory-Intensive (stress-ng)
**Expected:**

- RAM usage increases according to allocated amount (e.g., 1–3 GB)
- Possible swap usage if tests exceed physical RAM
- CPU usage moderately elevated

### Disk I/O-Intensive (fio)
**Expected:**

- High read/write throughput
- Significant disk latency variations
- I/O-wait percentage increases in CPU stats

### Network-Intensive (iperf3)
**Expected:**

- High incoming/outgoing bandwidth depending on test direction
- Increased packet processing load
- Potential network saturation depending on VM limits

### Server/Service Workload (nginx)
**Expected:**

- Moderate CPU usage per request
- Higher memory usage during concurrency tests
- Lower response times at baseline
- Higher response times under heavy load

## 3.4 Monitoring Strategy

This is how I planned to measure each application’s behaviour during testing:

### CPU
**Tools:**

- top
- vmstat 1
- mpstat -P ALL 1

**Metrics:**

- CPU utilisation %
- Load averages
- Idle time

### Memory
**Tools:**

- free -h
- vmstat 1
- cat /proc/meminfo

**Metrics:**

- Used / free RAM
- Swap usage
- Cache buffers

### Disk I/O
**Tools:**

- iostat -x 1
- dstat -d
- fio --output=result.json

**Metrics:**

- Throughput (MB/s)
- IOPS
- I/O wait %

### Network
**Tools:**

- iftop
- ss -s
- ip -s link

**Metrics:**

- Bandwidth
- Packet drops
- Transfer rates

### Service Response Times
**Tools:**

- curl -w "time_total: %{time_total}\n" -o /dev/null -s http://server
- ab -n 1000 -c 50 http://server/

**Metrics:**

- Response time
- Requests/sec
- Failed requests

# Week 3 Reflection

Week 3 felt like the calm before the storm. I was laying down the technical foundation for the heavy-duty testing phase coming later in Week 6. I realised that picking the right applications was just as important as running them. If I had picked poorly, the results would have been inconsistent or uninteresting. This week forced me to think analytically about workload diversity, how servers behave under different resource pressures, and how I would later justify my analysis with real data.
