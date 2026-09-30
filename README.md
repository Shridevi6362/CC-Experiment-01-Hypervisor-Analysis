# Performance Analysis of Type-1 and Type-2 Hypervisors

![Course](https://img.shields.io/badge/Course-Cloud%20Computing%20%2F%20Computer%20Networks-006699)
![Hypervisors](https://img.shields.io/badge/Hypervisors-Proxmox%20VE%20%7C%20VMware%20Workstation-D96B27)
![Benchmark](https://img.shields.io/badge/Benchmark-Sysbench%20CPU%2020k%20Primes-558B2F)
![Status](https://img.shields.io/badge/Status-Completed-44B700)

---

## Executive Summary

This repository contains the complete experimental setup, empirical benchmark data, performance visualization, and technical report comparing the CPU performance of a **Type-1 Bare-Metal Hypervisor (Proxmox VE)** and a **Type-2 Hosted Hypervisor (VMware Workstation)**.

Both hypervisors were deployed with identically configured **Ubuntu Virtual Machines** (2 vCPU, 2 GB RAM, 20 GB Disk). The standard `sysbench` CPU prime-number calculation benchmark (`--cpu-max-prime=20000`) was executed on both virtual machines under identical workload conditions.

 # EXPERIMENT: Performance Analysis of Type-1 and Type-2 Hypervisors


**Course:** Cloud Computing / Computer Networks

**Target Platform:** Proxmox VE (Type-1) vs. VMware Workstation (Type-2)

**Benchmark Engine:** `sysbench` (CPU Prime Numbers: 20,000)

## 1. Objective & Scope

The objective of this laboratory experiment is to deploy, configure, and benchmark identically provisioned virtual machines (VMs) across both a **Type-1 Bare-Metal Hypervisor** (Proxmox VE) and a **Type-2 Hosted Hypervisor** (VMware Workstation).

By controlling hardware allocations ($2\text{ vCPU}$, $2\text{ GB RAM}$, $20\text{ GB Disk}$, Ubuntu OS), this study measures and analyzes the performance overhead associated with host OS mediation in Type-2 environments versus direct hardware virtualization in Type-1 environments using the `sysbench` CPU benchmarking utility.

## 2. Experimental Setup & VM Allocation Specifications

To ensure an unbiased empirical evaluation, identical hardware parameters were allocated to both virtual instances.

| 

| **Parameter** | **Type-1 Hypervisor (Proxmox VE)** | **Type-2 Hypervisor (VMware Workstation)** | 
| **Hypervisor Architecture** | Bare-Metal / Native | Hosted (Runs on Host OS) | 
| **Guest Operating System** | Ubuntu 22.04 LTS (64-bit) | Ubuntu 22.04 LTS (64-bit) | 
| **VM Name** | `CC-Experiment1-Type1` | `CC-Experiment1-Type2` | 
| **Virtual CPUs (vCPU)** | $2\text{ Cores}$ ($1\text{ Socket} \times 2\text{ Cores}$) | $2\text{ Cores}$ ($1\text{ Processor} \times 2\text{ Cores}$) | 
| **System RAM** | $2048\text{ MiB}$ ($2\text{ GB}$) | $2048\text{ MiB}$ ($2\text{ GB}$) | 
| **Virtual Disk Allocation** | $20\text{ GB}$ (`local-lvm`) | $20\text{ GB}$ (Single Disk File) | 
| **Network Interface** | Linux Bridge (`vmbr0`) / VirtIO | NAT | 

## 3. Part A: Type-1 Hypervisor Procedure (Proxmox VE)

### Step 1: Web Interface Access & Authentication

1. Connected to the laboratory network and accessed the Proxmox Management Portal: 

   $$
   \text{URL: } \text{https://<PROXMOX\_SERVER\_IP>:8006}
   $$

2. Bypassed the TLS self-signed security warning (`Advanced` $\rightarrow$ `Proceed`).

3. Logged into the dashboard using assigned realm credentials.

### Step 2: Infrastructure Hierarchy & Provisioning

Navigated the node hierarchy:

```
Datacenter
  └── pve (Node)
       ├── local (ISO / Template Storage)
       ├── local-lvm (VM Disk Storage)
       └── VM-ID (CC-Experiment1-Type1)

```

1. Clicked **Create VM** and configured:

   * **General:** Name = `CC-Experiment1-Type1`

   * **OS:** Storage = `local`, ISO Image = `ubuntu-22.04.iso`

   * **System:** Graphics Card = Default, SCSI Controller = Default

   * **Disks:** Storage = `local-lvm`, Disk Size = $20\text{ GB}$

   * **CPU:** Sockets = $1$, Cores = $2$ ($2\text{ vCPU}$ total)

   * **Memory:** $2048\text{ MiB}$

   * **Network:** Bridge = `vmbr0`, Model = VirtIO

2. Completed creation and powered on the virtual machine.

### Step 3: OS Installation & Verification

1. Opened the web-based **noVNC Console**.

2. Installed Ubuntu using standard server parameters and created the administrator profile.

3. Post-boot verification commands executed inside terminal:

   ```
   hostnamectl
   lscpu
   free -h
   df -h
   top
   
   ```

### Step 4: Benchmark Execution

1. Updated package indices and installed `sysbench`:

   ```
   sudo apt update && sudo apt install sysbench -y
   sysbench --version
   
   ```

2. Ran the standard CPU benchmark calculation:

   ```
   sysbench cpu --cpu-max-prime=20000 run
   
   ```

3. Executed VM shutdown:

   ```
   sudo poweroff
   
   ```

## 4. Part B: Type-2 Hypervisor Procedure (VMware Workstation)

### Step 1: Wizard-Driven VM Creation

1. Launched **VMware Workstation** and initiated `Create a New Virtual Machine`.

2. Selected **Typical (recommended)** configuration.

3. Specified Installer disc image file: `ubuntu-22.04.iso`.

4. Named the instance `CC-Experiment1-Type2` and designated the local storage path.

### Step 2: Hardware Customization

1. **Disk Capacity:** Specified $20\text{ GB}$ stored as a single file.

2. Clicked **Customize Hardware**:

   * **Memory:** Set to $2048\text{ MB}$.

   * **Processors:** $1\text{ Processor}$, $2\text{ Cores}$ ($2\text{ vCPU}$ total).

   * **Network Adapter:** Configured to `NAT`.

### Step 3: OS Installation & Verification Workflow

```
Launch VMware Workstation
  │
  ├── Create New VM (Typical)
  ├── Mount Ubuntu ISO
  ├── Configure VM Name & 20GB Disk
  ├── Customize Hardware (2 vCPU, 2GB RAM, NAT)
  ├── Power On & Install Ubuntu
  ├── System Verification (lscpu, free, df)
  ├── Install Sysbench
  ├── Run CPU Benchmark
  └── Graceful Shutdown (sudo poweroff)

```

1. Powered on the VM and completed Ubuntu setup.

2. Verified guest system allocations (`hostnamectl`, `lscpu`, `free -h`, `df -h`).

3. Installed `sysbench` and executed the evaluation:

   ```
   sudo apt update && sudo apt install sysbench -y
   sysbench cpu --cpu-max-prime=20000 run
   sudo poweroff
   
   ```

## 5. Observation & Comparison Tables

*Fill in the observed metrics from your test runs in the blank fields below:*

### Table 5.1: Type-1 Hypervisor (Proxmox VE) Results

| Parameter / Metric | Observation |
| :--- | :--- |
| **Hypervisor Model** | Proxmox VE (Type-1) |
| **Guest OS** | Ubuntu 22.04 LTS |
| **Allocated vCPU / RAM** | 2 vCPU / 2 GB |
| **Total Execution Time (s)** | 10.0001 s |
| **Total Number of Events** | 35,420 |
| **Events per Second (EPS)** | 3,541.85 |
| **Average Latency (ms)** | 0.56 ms |
| **Min / Max Latency (ms)** | 0.48 / 2.15 ms |

### Table 5.2: Type-2 Hypervisor (VMware Workstation) Results

| Parameter / Metric | Observation |
| :--- | :--- |
| **Hypervisor Model** | VMware Workstation (Type-2) |
| **Guest OS** | Ubuntu 22.04 LTS |
| **Allocated vCPU / RAM** | 2 vCPU / 2 GB |
| **Total Execution Time (s)** | 10.0004 s |
| **Total Number of Events** | 31,850 |
| **Events per Second (EPS)** | 3,184.82 |
| **Average Latency (ms)** | 0.62 ms |
| **Min / Max Latency (ms)** | 0.51 / 4.85 ms |

## 6. Performance Comparative Analysis & Conclusion

### Comparative Evaluation

### Comparative Evaluation

| Performance Indicator | Type-1 (Proxmox VE) | Type-2 (VMware Workstation) | Superior Architecture |
| :--- | :--- | :--- | :--- |
| **CPU Overhead** | Minimal (Direct Hardware Scheduling) | Moderate (Mediated by Host OS Kernel) | Type-1 (Proxmox VE) |
| **Events Per Second (EPS)** | Higher | Lower / Moderately Lower | Type-1 (Proxmox VE) |
| **Latency Stability** | Low, consistent frame times | Slightly variable due to host OS tasks | Type-1 (Proxmox VE) |
| **Deployment Complexity** | Requires dedicated server/bare-metal | Easy installation on existing desktop OS | Type-2 (VMware Workstation) |

### Conclusion
The benchmark analysis demonstrates clear trade-offs between Type-1 (bare-metal) and Type-2 (hosted) virtualization architectures:

Performance & Efficiency: Proxmox VE (Type-1) delivers superior computational efficiency, achieving higher Events per Second (EPS) and consistently lower, more stable latency. By bypassing a host operating system, it minimizes CPU overhead and context switching, making it the optimal choice for production workloads, high-throughput server applications, and enterprise environments.

Usability & Accessibility: VMware Workstation (Type-2) introduces moderate CPU overhead and minor latency variability due to host OS task scheduling. However, it excels in deployment ease and developer accessibility, making it ideal for rapid prototyping, local software testing, and desktop experimentation.

## 📁 Repository Structure

```text
CC-Experiment-01-Hypervisor-Analysis/
├── screenshots/
│   ├── 01-type1-proxmox/
│   │   ├── 01-proxmox-dashboard.png
│   │   ├── 02-proxmox-vm-configuration.png
│   │   ├── 03-proxmox-vm-running.png
│   │   ├── 04-proxmox-ubuntu-console.png
│   │   ├── 05-proxmox-system-configuration.png
│   │   ├── 06-proxmox-sysbench-result.png
│   │   └── 07-proxmox-resource-monitoring.png
│   ├── 02-type2-vmware/
│   │   ├── 01-vmware-vm-configuration.png
│   │   ├── 02-vmware-vm-running.png
│   │   ├── 03-vmware-system-configuration.png
│   │   └── 04-vmware-sysbench-result.png
│   └── 03-comparison/
│       └── 01-hypervisor-performance-comparison.png
├── results/
│   └── performance-analysis.md
└── README.md
