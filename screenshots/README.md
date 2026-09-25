# CC-Experiment-01: Hypervisor Performance Analysis

A performance comparison benchmark evaluating a **Type-1 Hypervisor (Proxmox VE)** against a **Type-2 Hypervisor (VMware Workstation)** using `sysbench` CPU workloads on Ubuntu Linux guest virtual machines.

---

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
