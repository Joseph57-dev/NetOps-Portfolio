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
- Ping verification for connectivity testing  

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
  `ping PC2 evidence.png` (success verification)

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


## ✅ Verification - Lab Success Criteria

The lab is considered successful when:
- All router interfaces are configured with correct IP addresses
- All static routes are correctly configured on R1, R2, and R3
- **PC1 can successfully ping PC2 (192.168.3.1)** - This is the primary verification of end-to-end connectivity
- The ping response shows zero packet loss, confirming the routing path is complete

### Evidence of Success
A successful ping from PC1 to PC2 will display output similar to:
```
Reply from 192.168.3.1: bytes=32 time=XX ms TTL=XX
```

This confirms that:
- PC1's packets reach PC2 through the static routing configuration
- Return packets successfully traverse back through the routing path
- The inter-LAN communication objective has been achieved

*Ping test screenshot saved as: `ping PC2 evidence.png`*


