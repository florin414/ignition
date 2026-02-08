# Ignition ⚡🔥

![Performance](https://img.shields.io/badge/Cold%20Start-%3C100ms-brightgreen)
![Security](https://img.shields.io/badge/Isolation-MicroVM-red)
![Technology](https://img.shields.io/badge/Tech-Firecracker%20%2B%20Go-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

**Ignition** is a Serverless Compute Platform designed for speed and absolute security.

While traditional containers share the host kernel (a security risk), Ignition utilizes **Firecracker MicroVMs** to provide the isolation of a virtual machine with the startup time of a container.

## 🏗️ Architecture & Scheduling

To achieve the strict **<100ms latency goal**, Ignition abandons the traditional centralized queue model.

### 1. Distributed Scheduling (Work Stealing)
Instead of a single global bottleneck, Ignition employs a decentralized scheduling architecture:
* **Local Queues:** Each worker node manages its own queue of pending function invocations.
* **Work Stealing:** When a node is idle, it randomly probes other nodes to "steal" tasks, ensuring efficient load balancing without lock contention on a central scheduler.

### 2. MicroVM Provisioning
The system maintains a pool of "warm" MicroVM slots. When a function is invoked, Ignition attaches the user code and network interface via a Tap device in milliseconds.

### 3. Ephemeral Storage & Networking
Every execution occurs in a sterile environment. Egress traffic is strictly controlled via iptables policies to prevent data exfiltration.

## 🛠️ Tech Stack

* **Hypervisor:** Firecracker (KVM)
* **Control Plane:** Go (Golang)
* **Communication:** gRPC
* **Networking:** CNI (Container Network Interface)

## 🚀 Benchmark

```bash
# Run the cold-start benchmark
go run cmd/benchmark/main.go --concurrency 1000

# Output:
# P50 Cold Start: 85ms
# P99 Cold Start: 110ms
