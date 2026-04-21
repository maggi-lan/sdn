# SDN Bandwidth Measurement using Mininet

## Problem Statement

This project demonstrates Software Defined Networking (SDN) concepts using Mininet and the Ryu controller. The objective is to analyze how controller-defined flow rules influence network behavior such as connectivity and bandwidth.

The project includes:
- A baseline learning switch controller
- A custom controller that forces traffic through a specific path
- Two network topologies for comparison

---

## Project Structure

```
controllers/
  baseline.py
  force_path.py

topologies/
  single_switch.py
  two_path_topo.py
```

---

## Setup / Execution Steps

### 1. Install Dependencies

```bash
sudo apt update
sudo apt install mininet python3-pip -y
pip3 install ryu
```

---

### 2. Clean Mininet

```bash
sudo mn -c
```

---

## Scenario 1: Baseline (Learning Switch)

### Start Controller

```bash
ryu-manager controllers/baseline.py
```

### Start Topology

```bash
sudo mn --custom topologies/single_switch.py \
--topo single --controller=remote \
--switch ovsk,protocols=OpenFlow13
```

### Test

```bash
pingall
iperf h1 h2
```

---

## Scenario 2: Forced Path Routing (SDN Control)

### Start Controller

```bash
ryu-manager controllers/force_path.py
```

### Start Topology

```bash
sudo mn --custom topologies/two_path_topo.py \
--topo twopath --controller=remote \
--switch ovsk,protocols=OpenFlow13
```

### Test

```bash
pingall
iperf h1 h2
```

---

## View Flow Tables

```bash
sh ovs-ofctl -O OpenFlow13 dump-flows s1
sh ovs-ofctl -O OpenFlow13 dump-flows s2
sh ovs-ofctl -O OpenFlow13 dump-flows s3
```

---

## Expected Output

### Connectivity

```
*** Ping: testing ping reachability
*** Results: 0% dropped
```

---

### Bandwidth

```
*** Iperf: testing TCP bandwidth between h1 and h2
Bandwidth: (value in Mbits/sec)
```

---

### Flow Table Example

```
cookie=0x0, priority=1,in_port=3 actions=output:2
```

---

## Explanation

- **baseline.py**: Implements a learning switch that forwards packets based on MAC address learning.
- **force_path.py**: Installs flow rules to force traffic through a specific path in the network.
- **single_switch.py**: Simple topology with one switch connecting two hosts.
- **two_path_topo.py**: Topology with two possible paths between hosts, used to demonstrate SDN routing control.

---

## Summary

This project shows how SDN controllers can control packet forwarding behavior. By comparing baseline switching with forced routing, the impact of SDN on network performance can be observed.
