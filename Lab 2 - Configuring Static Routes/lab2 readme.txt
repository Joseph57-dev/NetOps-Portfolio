# 🧪 Lab 2 — Static Routing Between Two LANs

## 📅 Date
2026-01-22

---

## 🎯 Objective

To configure end devices and routers from scratch and implement **static routing** that enables communication between two separate LANs connected through an intermediate router.

This lab demonstrates a core **Layer 3 routing concept** required for enterprise networks and foundational for dynamic routing and security policies.

---

## 🧠 Concepts Covered

- IP addressing and default gateway configuration  
- Router interface configuration  
- Point-to-point router connections  
- Static route configuration  
- End-to-end connectivity verification  
- Basic routing troubleshooting  

---

## 🗺 Network Topology

### Devices Used
- 3 × Routers  
  - R1 (LAN 1 gateway)  
  - R2 (Transit / Core router)  
  - R3 (LAN 2 gateway)  
- 2 × Switches  
  - SW1 (LAN 1)  
  - SW2 (LAN 2)  
- 2 × PCs  
  - PC1 (LAN 1)  
  - PC2 (LAN 2)  

### Logical Topology Description
- PC1 connects to SW1, which connects to R1  
- PC2 connects to SW2, which connects to R3  
- R1 and R3 connect through R2  
- Static routes are used to allow inter-LAN communication  

*topology diagram attached*

---

## 📂 Files

- **Packet Tracer File:**  
  `Lab 2 - Configuring Static Routes.pkt`

- ** Config screenshots include:**  
  `pc1 gateway(R1)config.png`  
  `R1 static route config.png`  
  `R2 static route Config.png`

---

## 📐 IP Addressing Plan

### LAN 1
- Network: `192.168.1.0/24`  
- R1 (Gateway): `192.168.1.254`  
- PC1: `192.168.1.1`  

### LAN 2
- Network: `192.168.3.0/24`  
- R3 (Gateway): `192.168.3.254`  
- PC2: `192.168.3.1`  

### Router Interconnections
- R1 ↔ R2: `192.168.12.0/24`  
- R2 ↔ R3: `192.168.13.0/24`  

---


### Required: Configure Hostnames too and interface configurations.

On each router:
```bash
enable
configure terminal
hostname R1
........


