# Week 6 — Performance Evaluation & Analysis (Phase 6)

## Week Overview

Week 6 was the most technical and data-driven phase of the coursework. This was the point where all planning and preparation from Weeks 3 to 5 came together through **controlled performance testing, quantitative data collection, visualisation, and optimisation**.

During this week, I evaluated the system across multiple performance dimensions, including:
- CPU utilisation
- Memory usage
- Disk I/O performance
- Network throughput and latency
- Service response times

Rather than simply executing commands, this phase required careful measurement, interpretation, comparison, and optimisation. Each test was designed to isolate specific resource pressures so that operating system behaviour could be analysed accurately.

---

## Reflection

This week felt the most like real-world system administration. Instead of focusing on configuration alone, I was responsible for designing experiments, collecting reliable metrics, and interpreting results to identify bottlenecks and trade-offs.

The process of comparing baseline performance against stressed and optimised scenarios helped me understand how operating system components interact under load. It also reinforced the importance of evidence-based decision-making, where optimisation choices are justified using measurable improvements rather than assumptions.

###Week Overview (Reflective Summary)

At the start of Week 6, I designed a structured and repeatable performance testing strategy to ensure that each workload category was evaluated consistently and fairly. Every application selected earlier in the coursework (CPU, memory, disk I/O, network, and server workloads) was tested under the same staged conditions.

Each workload was evaluated across the following phases:
- **Baseline testing** (idle system state)
- **Load testing** (stress or workload applied)
- **Bottleneck identification**
- **Optimised configuration runs**

This approach allowed me to compare system behaviour before and after optimisation and to identify how different operating system subsystems responded under pressure.

---

### Tools and Techniques Used

To collect performance data, I relied on a combination of standard Linux utilities, benchmarking tools, and custom scripts:

- `top` / `htop` for CPU utilisation and load monitoring  
- `free -m` for memory usage and swap activity  
- `iostat` and `dd` for disk throughput and I/O behaviour  
- `iperf3` for network throughput testing  
- `ping` and application-level response tests for latency measurement  
- The remote monitoring script developed in **Week 5**  
- Custom logging scripts to record and timestamp output  

All monitoring and testing was performed remotely over SSH, maintaining the headless-server administration model.

---

### Reflection

By the end of the week, I had not only collected raw performance data but also transformed it into visual evidence. I plotted the results into charts (generated earlier using Python) to clearly demonstrate system behaviour, bottlenecks, and the impact of optimisation steps.

This process reinforced the value of structured testing and visualisation. Rather than relying on isolated command output, I was able to analyse trends, compare scenarios, and justify optimisation decisions using quantitative evidence. This made the performance analysis more rigorous, defensible, and aligned with real-world system administration practices.


## 6.1 Detailed Testing Approach

To ensure consistency and reliability across all performance tests, I followed the same structured methodology for every application and workload type.

---

### Step 1 — Start from an Idle State

Before applying any workload, I allowed the virtual machine to remain idle for **1–2 minutes**. During this period, I monitored system activity to confirm that CPU usage, memory consumption, and background processes had stabilised.

This idle phase established a clean **baseline state**, ensuring that subsequent performance measurements were not influenced by residual load from previous tests.

Typical checks performed during this stage included:
- Verifying low CPU utilisation using `top`
- Confirming stable memory usage with `free -m`
- Ensuring no unexpected background processes were running

Establishing a consistent idle baseline was critical for accurately comparing baseline, load, and optimised performance results later in the testing process.

---

### Step 2 — Run Baseline Measurement Commands

Once the system had stabilised in an idle state, I captured baseline performance metrics using a consistent set of command-line tools. These measurements represent the server’s normal operating behaviour without any applied workload.

The following commands were executed to collect baseline data:

```bash
top -bn1

```

### Step 3 — Start the Application Load

After recording baseline metrics, I applied a controlled workload depending on the resource being tested. Each workload was designed to stress a specific subsystem while keeping other variables as stable as possible.

---

#### CPU Load Test
```bash
stress --cpu 4

```
### Step 4 — Run Remote Monitoring Script

While each workload was running, I executed the remote monitoring script created in **Week 5** from the workstation. This script automatically collected live performance metrics from the server over SSH.

```bash
./monitor-server.sh

```

### Step 5 — Record Results into a Performance Table

After each test run, I recorded the collected metrics into a structured performance table. This ensured that results from baseline, load, and optimised runs could be compared consistently across all workload types.

For each application test, I documented:
- Average and peak CPU utilisation
- Memory usage and swap activity
- Disk throughput and I/O wait
- Network throughput and latency
- Service response times (where applicable)

Recording results in tabular form allowed me to:
- Clearly compare baseline vs load vs optimised performance
- Identify trends and bottlenecks across different resources
- Use the data directly for chart generation and analysis

This structured data collection step was critical for producing meaningful visualisations and for supporting evidence-based optimisation decisions later in Week 6.

---

### Step 6 — Apply Optimisation Changes

After identifying performance bottlenecks from the baseline and load test results, I applied a set of targeted optimisation changes. These adjustments were selected to improve performance across storage, memory, networking, and kernel behaviour while maintaining system stability.

---

#### Enabling SSD-Style I/O Scheduler

To improve disk I/O performance, I configured the I/O scheduler to a modern, SSD-optimised option suitable for virtualised environments.

This optimisation aimed to reduce I/O latency and improve throughput during disk-intensive workloads.

---

#### Increasing File Descriptor Limits

I increased file descriptor limits to allow the system and services to handle a higher number of concurrent open files and network connections.

This optimisation was particularly relevant for server workloads and high-concurrency scenarios, helping prevent resource exhaustion under load.

---

#### Enabling zRAM Compression

To reduce memory pressure and avoid excessive swapping, I enabled **zRAM**, which provides compressed RAM-based swap space.

This change helped improve performance during memory-intensive workloads by keeping frequently accessed data in compressed memory rather than writing it to disk.

---

#### Kernel Parameter Tuning

I adjusted selected kernel parameters using `sysctl` to optimise system behaviour under load. These changes focused on improving memory management, reducing unnecessary swapping, and optimising network performance.

Kernel tuning was applied cautiously to avoid destabilising the system while still improving responsiveness and throughput.

---

#### Optimising Network Buffers

Network buffer sizes were increased to improve throughput and reduce packet loss during high-bandwidth network tests.

This optimisation was particularly effective during `iperf3` testing, where larger buffers allowed the system to handle sustained traffic more efficiently.

---

### Reflection

Applying these optimisations reinforced the importance of evidence-based tuning. Rather than applying generic tweaks, each change was driven by observed bottlenecks and validated through re-testing. This iterative optimisation process closely mirrored real-world system administration practices, where performance improvements must be justified with measurable results.

---

### Step 7 — Re-run Tests and Measure Improvements

After applying the optimisation changes, I re-ran the **same set of tests** using the **exact methodology and commands** as the original baseline and load runs. This ensured that any observed performance differences were attributable to the optimisations rather than changes in testing conditions.

The re-testing process followed these principles:
- Identical workloads and stress levels were used
- The same monitoring tools and scripts were executed
- Measurements were taken over comparable time windows
- Results were recorded in the same performance tables

---

### Measuring Improvements

During re-testing, I focused on identifying measurable improvements such as:
- Reduced CPU load and more stable scheduling behaviour
- Lower memory pressure and reduced swap activity
- Improved disk throughput and reduced I/O wait times
- Higher network throughput with fewer packet drops
- Improved application response times under load

The post-optimisation results were compared directly against baseline and pre-optimisation data to quantify performance gains.

---

### Reflection

Re-running the tests validated the effectiveness of the optimisation process. Seeing measurable improvements confirmed that performance tuning should be driven by data rather than assumptions. This step reinforced the importance of controlled experimentation and repeatable testing when evaluating system performance in real-world environments.

### Memory Performance

| Test Type     | Memory Used (MB) |
|--------------|------------------|
| Baseline     | 800              |
| Stress Load  | 1900             |
| Optimised    | 1500             |

The memory results show a significant increase in usage under stress conditions. After optimisation, memory consumption was reduced compared to the stressed state, indicating improved memory management and reduced pressure on physical RAM.

### Disk I/O Performance

| Test Type     | IOPS / Throughput (MB/s) |
|--------------|---------------------------|
| Baseline     | 120                       |
| Stress Load  | 60                        |
| Optimised    | 85                        |

The disk I/O results demonstrate a clear performance drop under heavy write activity. After applying storage-related optimisations, throughput improved significantly compared to the stressed state, indicating reduced I/O contention and improved scheduling behaviour.

### Network Performance

| Test Type     | Throughput (Mbps) |
|--------------|--------------------|
| Baseline     | 80                 |
| Stress Load  | 940                |
| Optimised    | 970                |

The network performance results show a substantial increase in throughput under load. After optimisation, throughput improved further, indicating more efficient network buffer utilisation and reduced overhead during high-bandwidth transfers.

### Application-Level Response Time

| Test Type     | Response Time (ms) |
|--------------|---------------------|
| Baseline     | 120                 |
| Stress Load  | 350                 |
| Optimised    | 95                  |

The response time results highlight the impact of system load on application performance. Under stress, response times increased significantly due to resource contention. After optimisation, response times improved beyond the baseline level, indicating more efficient resource allocation and improved service responsiveness.

### 6.3 Performance Charts

To visually analyse system behaviour and performance improvements, I generated a series of charts based on the raw measurement data collected during testing. These charts were produced earlier using Python and are included as part of the project evidence.

Each chart corresponds directly to the datasets presented in Section 6.2 and illustrates performance differences between **baseline**, **stress**, and **optimised** states.

---

### CPU Usage Chart
This chart visualises average CPU utilisation across baseline, stress load, and optimised configurations. It highlights the impact of CPU stress testing and the effectiveness of scheduling and kernel-level optimisations.

---

### Memory Usage Chart
The memory usage chart shows how RAM consumption increased under load and how optimisation techniques (such as improved memory handling and compression) reduced overall memory pressure.

---

### Disk I/O Performance Chart
This chart illustrates changes in disk throughput and I/O efficiency under heavy write workloads. The improvement after optimisation demonstrates the benefits of using an SSD-appropriate I/O scheduler and tuning disk behaviour.

---

### Network Performance Chart
The network performance chart compares throughput across testing phases. It clearly shows how network buffer tuning improved sustained bandwidth during high-load `iperf3` testing.

---

### Application Response Time Chart
This chart presents application-level response times under baseline, stressed, and optimised conditions. The post-optimisation results demonstrate improved service responsiveness and reduced latency under load.

---

### Reflection
Visualising performance data made trends and improvements much easier to identify than raw command output alone. The charts provided clear evidence to support my optimisation decisions and allowed me to justify performance improvements using quantitative, visual data rather than assumptions.

## 6.4 Performance Analysis & Findings

This section analyses the collected performance data and charts to identify bottlenecks, evaluate optimisation effectiveness, and explain observed system behaviour under different workloads.

---

### CPU Analysis

During stress testing, CPU utilisation reached **95%**, confirming that the workload fully saturated the available vCPUs. This behaviour aligned with expectations for a CPU-bound stress test and validated that the test was effectively exercising the scheduler.

After applying optimisation steps such as CPU scheduler tuning and reducing unnecessary background services, average CPU utilisation under load stabilised at approximately **70%**. This represents an efficiency improvement of around **26%**, indicating that the system was able to handle the same workload with significantly less CPU overhead.

---

### Reflection

This analysis helped me understand how much impact background services and scheduler behaviour can have on CPU-bound applications. Even without changing the workload itself, careful system tuning produced measurable performance gains, reinforcing the importance of optimisation driven by evidence rather than assumptions.

### Memory Analysis

During the memory stress test, RAM usage increased to nearly **2 GB**, indicating significant memory pressure under load. This aligned with expectations for a memory-intensive workload and confirmed that the test was effectively stressing the system’s memory subsystem.

After enabling **zRAM** and tuning memory-related parameters such as swappiness, peak memory usage during load was reduced to approximately **1.5 GB**. This reduction indicates that compressed RAM-based swap was successfully absorbing pressure that would otherwise have resulted in heavier disk swapping.

### Reflection
zRAM proved to be a surprisingly effective optimisation. Under high memory pressure, the server remained noticeably more responsive, with fewer stalls and reduced performance degradation. This highlighted how memory compression can significantly improve system behaviour on resource-constrained virtual machines.

### Disk Performance Analysis

During disk-intensive testing, throughput dropped from a baseline of **120 MB/s** to approximately **60 MB/s** under sustained load. This confirmed that the storage subsystem became a bottleneck when handling synchronous write operations.

After switching to an SSD-optimised I/O scheduler (`none`), disk throughput under load improved to around **85 MB/s**. This represents a significant recovery in performance and demonstrates the impact of selecting an appropriate I/O scheduler for virtualised or SSD-backed storage.

### Reflection

This was my first practical experience working with Linux I/O schedulers. Observing the measurable improvement after changing the scheduler helped me understand how scheduling policies directly influence disk read/write behaviour and overall system responsiveness under I/O-heavy workloads.

### Network Performance Analysis

Network performance was evaluated using `iperf3` to measure throughput and latency under load. Prior to optimisation, the server achieved approximately **940 Mbps** during sustained network testing.

After tuning network buffer sizes and related kernel parameters, throughput increased to around **970 Mbps**. In addition to higher throughput, network latency under load was reduced by approximately **30 ms**, indicating more efficient packet handling and reduced buffering delays.


### Reflection
Network tuning produced the clearest and most immediately visible performance improvements. Adjusting a small number of kernel parameters resulted in measurable gains in both throughput and latency, highlighting how impactful targeted network optimisation can be in high-bandwidth scenarios.

### Response Time Analysis (End-to-End)

End-to-end application response time was the most important performance metric, as it reflects the actual user experience rather than individual subsystem behaviour.

During baseline testing, average response time was approximately **120 ms**. Under load, response time increased significantly to around **350 ms**, demonstrating the impact of CPU, memory, disk, and network contention on service responsiveness.

After applying system optimisations, response time improved to approximately **95 ms**, outperforming even the original baseline measurement.

---

### Reflection

This result demonstrated that optimisation efforts did more than improve individual operating system metrics—they directly enhanced real service performance. Reducing response time beyond the baseline confirmed that targeted tuning across multiple subsystems can produce meaningful improvements from an end-user perspective, not just at the OS level.

## 6.5 Optimisations Implemented

This section documents the specific optimisation changes applied after analysing performance bottlenecks. Each optimisation was selected based on observed behaviour during stress testing and was validated through re-testing.

---

### 1. Increased File Descriptor Limits

To support higher concurrency and prevent resource exhaustion under load, I increased the system-wide file descriptor limits. This is particularly important for network services and applications that handle many simultaneous connections.

#### Configuration Changes
```bash
sudo nano /etc/security/limits.conf

### 2. Enabled zRAM Compression

To reduce memory pressure and minimise disk swapping during memory-intensive workloads, I enabled **zRAM**, which provides compressed swap space in RAM.

#### Installation
```bash
sudo apt install zram-tools

