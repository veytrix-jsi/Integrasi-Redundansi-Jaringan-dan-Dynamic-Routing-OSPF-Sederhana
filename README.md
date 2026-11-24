# 🚀 Integrasi Redundansi Jaringan dan Dynamic Routing OSPF Sederhana (Cisco Packet Tracer)

## 🧠 Overview

This lab demonstrates a fault-tolerant, multi-router network integrating **Network Redundancy** and **Dynamic Routing OSPF** using Cisco Packet Tracer.  
The project includes:

- 🔁 Full-mesh triangle topology across 3 routers (`Router0`, `Router1`, `Router2`) to eliminate Single Point of Failure (SPoF)
- ⚖️ Custom **OSPF Cost** configuration to prioritize primary and backup paths
- ⚡ Automatic failover & failback routing when a primary link goes down
- 🚫 Passive interfaces on LAN ports (`Fa0/1/0`) for routing security
- 💻 End-to-end PC-to-PC communication across different subnets

---

## 🖥️ Topology

```text
               [Router0] (Backup Path)
             Fa0/0 / \ Fa0/1
       (Cost: 50) /   \ (Cost: 50)
                 /     \
    [PC0]---[Router1]---[Router2]---[PC1]
             Fa0/1         Fa0/0
            (Primary Path - Cost: 10)

## 🖥️ Topology

[PC1]---[SW1]---[Router2]---[Router1]---[Router3]---[SW3]---[PC3] | DHCP Server


---
## 📁 Project Structure
integrasi-redundansi-ospf/
├── Redundansi-OSPF.pkt    ← Packet Tracer project file
├── README.md              ← This file
├── topology/
│   └── topologi-ospf.png  ← Topology screenshot
└── configs/
    ├── Router0.txt        ← Backup transit router config
    ├── Router1.txt        ← Left edge router config (LAN PC0)
    └── Router2.txt        ← Right edge router config (LAN PC1)

---

##🛠️ Technologies Used
Cisco Packet Tracer 8.x

Cisco 1841 Routers (with HWIC-4ESW module)

OSPFv2 (Single-Area - Area 0)

OSPF Interface Cost Manipulation (ip ospf cost)

Passive Interface for LAN ports (Fa0/1/0)

---

##🔢 IP Addressing Table
Device Interface  IP Address Subnet Mask Role / OSPF Cost
Router0 Fa0/0 10.10.10.2 255.255.255.252 Backup link to Router1 (Cost: 50)
Fa0/1 10.10.20.2 255.255.255.252 Backup link to Router2 (Cost: 50)
Router1 Fa0/010.10.10.1255.255.255.252 Backup link to Router0 (Cost: 50)
Fa0/1 10.10.12.1 255.255.255.252 Primary link to Router2 (Cost: 10)
Fa0/1/0 192.168.1.1255.255.255.0 Gateway for PC0 (Passive)
Router2 Fa0/0 10.10.12.2255.255.255.252 Primary link to Router1 (Cost: 10)
Fa0/1 10.10.20.1 255.255.255.252 Backup link to Router0 (Cost: 50)
Fa0/1/0 192.168.2.1 255.255.255.0 Gateway for PC1 (Passive)
PC0 Fa0 192.168.1.2 255.255.255.0 Gateway: 192.168.1.1
PC1 Fa0 192.168.2.2 255.255.255.0 Gateway: 192.168.2.1

## 🧪 How to Test
1. Open Redundansi-OSPF.pkt in Cisco Packet Tracer.
2. Normal Condition Test:
• Open Command Prompt on PC0 (192.168.1.2) and run tracert 192.168.2.2.
• Verify traffic takes the direct primary path (Router1 $\rightarrow$ Router2) with total Cost 10.
3. Failover Test (Link Failure):
• Disconnect the cable between Router1 (Fa0/1) and Router2 (Fa0/0) or run shutdown on Fa0/1.
Run tracert 192.168.2.2 again on PC0.Verify traffic automatically reroutes via Router0 (Router1 $\rightarrow$ Router0 $\rightarrow$ Router2) with total Cost 100.On routers, verify:show ip ospf neighborshow ip route ospfshow ip ospf interface brief`

---

## 📄 Configuration Notes

| Router | Key Features |
|--------|--------------|
| Router1 | OSPF with `router-id 1.1.1.1`, DHCP relay not needed |
| Router2 | **DHCP Server**, OSPF with `router-id 2.2.2.2`, passive LAN |
| Router3 | OSPF with `router-id 3.3.3.3`, receives routes via OSPF |

---

✅ Project built and tested by [Shahin Akbari] using Cisco Packet Tracer
