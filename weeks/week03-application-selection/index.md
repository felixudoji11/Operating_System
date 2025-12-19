WEEK 3 — Application Selection for Performance Testing (Phase 3)
(Personal, detailed, reflective technical journal)
Week Overview
This week was all about choosing the applications and workloads I would use later during performance testing. I wanted a realistic mix of CPU, memory, disk, network, and server-based applications so that my Week 6 experiments would show meaningful differences across various resource types.

My Reflection:
At first, I thought I’d just pick random applications, but the more I looked into enterprise performance testing, the more I realised I needed a balanced workload set. Each application had to represent a distinct resource type, otherwise the testing would be shallow and unstructured. So I treated this selection process almost like preparing benchmarks for a real data-centre environment.

I spent a good amount of time researching open-source tools that were lightweight enough for my VM but powerful enough to generate measurable workloads. I also practised installing them via SSH to make sure they worked properly in a headless environment.

3.1 Application Selection Matrix
(Workload types with justification)
Below is the final Application Matrix I created. These are the applications I selected for the performance evaluation in Week 6.

Workload Type	Application	Why I Chose It	Expected Impact
CPU-intensive	stress-ng	Designed for CPU benchmarking, controllable load generation	High CPU usage, temperature increase, clear load patterns
Memory-intensive	stress-ng (memory mode)	Same tool, allows memory allocation and pressure testing	RAM saturation, potential swap usage
Disk I/O intensive	fio	Industry-standard I/O benchmarking tool	Reads/writes, throughput, I/O latency
Network-intensive	iperf3	Reliable network performance testing between systems	High bandwidth usage, packet behaviour monitoring
Server/Application workload	Nginx web server	Realistic service workload simulating real server behaviour	Concurrent requests, response time under load
Reflection:
This matrix became one of the most important pieces of planning I’ve done so far. It made the testing phase smoother because I already understood what each tool represented and why I needed it.

3.2 Installation Documentation (SSH-Based Installations)
All installations were performed exclusively through SSH from my workstation, maintaining the required headless-server setup.

Below are the installation steps I executed, with commands and explanations.

1. Installing stress-ng
sudo apt update
sudo apt install stress-ng -y
Why: This tool allows me to create predictable, repeatable stress tests for CPU, memory, and I/O.

2. Installing fio
sudo apt install fio -y
Why: fio produces measurable disk I/O results such as throughput and latency — crucial for analysing disk behaviour.

3. Installing iperf3
sudo apt install iperf3 -y
Why: I needed this for network throughput tests, with my workstation acting as the iperf client/server depending on test direction.

4. Installing nginx
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
Why: Running a real service lets me analyse server response times, concurrency behaviour, and resource usage under load.

3.3 Expected Resource Profiles
Before running any tests, I wrote out what I expected each application to do. This helped me compare the predicted behaviour with actual Week 6 results.

CPU-Intensive (stress-ng)
Expected:

CPU usage spikes to 90–100%

System load increases above number of available cores

Minimal disk or memory usage unless configured

Reasoning: CPU stress loops typically execute pure computation operations.

Memory-Intensive (stress-ng)
Expected:

RAM usage increases according to allocated amount (e.g., 1–3 GB)

Possible swap usage if tests exceed physical RAM

CPU usage moderately elevated

Disk I/O-Intensive (fio)
Expected:

High read/write throughput

Significant disk latency variations

I/O-wait percentage increases in CPU stats

Network-Intensive (iperf3)
Expected:

High incoming/outgoing bandwidth depending on test direction

Increased packet processing load

Potential network saturation depending on VM limits

Server/Service Workload (nginx)
Expected:

Moderate CPU usage per request

Higher memory usage during concurrency tests

Lower response times at baseline

Higher response times under heavy load

3.4 Monitoring Strategy
This is how I planned to measure each application’s behaviour during testing:

CPU
Tools:

top
vmstat 1
mpstat -P ALL 1
Metrics:

CPU utilisation %

Load averages

Idle time

Memory
Tools:

free -h
vmstat 1
cat /proc/meminfo
Metrics:

Used / free RAM

Swap usage

Cache buffers

Disk I/O
Tools:

iostat -x 1
dstat -d
fio --output=result.json
Metrics:

Throughput (MB/s)

IOPS

I/O wait %

Network
Tools:

iftop
ss -s
ip -s link
Metrics:

Bandwidth

Packet drops

Transfer rates

Service Response Times
Tools:

curl -w "time_total: %{time_total}\n" -o /dev/null -s http://server
ab -n 1000 -c 50 http://server/
Metrics:

Response time

Requests/sec

Failed requests

Week 3 Reflection
Week 3 felt like the calm before the storm. I was laying down the technical foundation for the heavy-duty testing phase coming later in Week 6. I realised that picking the right applications was just as important as running them. If I had picked poorly, the results would have been inconsistent or uninteresting. This week forced me to think analytically about workload diversity, how servers behave under different resource pressures, and how I would later justify my analysis with real data.
