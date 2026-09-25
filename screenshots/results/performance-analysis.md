# 5.3 Performance Comparison Analysis

## Benchmark Overview
A CPU performance benchmark was conducted using `sysbench cpu --cpu-max-prime=20000 run` to compare the computational capability, throughput, and execution latency between a Type-1 Hypervisor (Proxmox VE) and a Type-2 Hypervisor (VMware Workstation).

---

## Benchmark Results

| Parameter | Type-1 Proxmox VE | Type-2 VMware Workstation |
| :--- | :--- | :--- |
| **Total Execution Time** | 10.0005s | 10.0008s |
| **Total Events** | 17,494 | 12,778 |
| **Events per Second** | 1,749.16 | 1,277.56 |
| **Average Latency** | 0.57 ms | 0.78 ms |

---

## Detailed Analysis

### 1. Throughput (Events per Second)
* **Proxmox VE (Type-1):** Generated **1,749.16 events/sec**, completing 17,494 events in ~10 seconds.
* **VMware Workstation (Type-2):** Generated **1,277.56 events/sec**, completing 12,778 events in ~10 seconds.
* **Key Finding:** Proxmox VE achieved **36.9% higher throughput**. Because Proxmox VE runs directly on physical bare metal, guest virtual machines interact directly with host CPU resources without passing through an intermediate host operating system layer.

### 2. Execution Latency
* **Proxmox VE (Type-1):** Average latency of **0.57 ms**.
* **VMware Workstation (Type-2):** Average latency of **0.78 ms**.
* **Key Finding:** Proxmox VE achieved **26.9% lower latency**. Type-2 hypervisors introduce extra context switches and scheduling latency caused by the underlying host OS (e.g., Windows or Linux desktop) competing for CPU cycles.

---

## Conclusion
The benchmark performance clearly highlights the efficiency advantage of bare-metal virtualization:
* **Type-1 (Proxmox VE)** is optimal for high-performance server workloads and enterprise production environments due to direct hardware access and reduced virtualization overhead.
* **Type-2 (VMware Workstation)** is better suited for desktop development and local testing environments, where ease of installation on a host desktop OS is prioritized over maximum hardware throughput.
