# 🧪 Lab 6 — Inter-VLAN Routing Using Multilayer Switching (SVIs)

## 📅 Date
2026-01-22


Overview

In the previous labs, inter-VLAN communication was achieved using a router. While those approaches worked, they also revealed clear limitations as the network grew.
This lab builds on Lab 5 and introduces multilayer switching as a more scalable and efficient solution for inter-VLAN routing.

In this topology, SW2 is upgraded to a multilayer switch and now performs Layer 3 routing internally, eliminating the need to route VLAN traffic through the router. The router (R1) is retained only for external/Internet connectivity.

This lab demonstrates:

How inter-VLAN routing evolves across different methods

How SVIs (Switched Virtual Interfaces) replace router sub-interfaces

How VLAN trunking and native VLAN design choices affect security and stability

How to configure and verify a multilayer switch for production-style networks

---

Note: this lab is a continuation of Lab 5 with some modifications to accommodate SW2 as a multilayer Switch and connection between R1 and SW 2 being a default route... so it will detail only these modifications and basics of configuring access port and trunks were already done in lab 5 

For basic configuration see lab 5 
https://github.com/Joseph57-dev/NetOps-Portfolio/tree/master/Lab%205%20-%20VLAN%20Trunking%20%26%20Router-on-a-Stick%20(ROAS)

## 🎯 Objective

To implement **inter-VLAN routing using a multilayer switch (Layer 3 switch)**, replacing Router-on-a-Stick, and to verify internal VLAN communication and external Internet connectivity.

This lab demonstrates the **most scalable and efficient enterprise inter-VLAN routing method**.

---

## 🧠From Routers to Multilayer Switching: Evolution of Inter-VLAN Routing

Before implementing multilayer switching, it is important to understand why it exists.

 Inter-VLAN Routing Methods 

### 1️⃣ One Physical Interface per VLAN (Legacy Method)
- Each VLAN connects to a **separate router interface**
- Simple but:
  - Router interfaces are limited
  - Not scalable for many VLANs
  - High hardware cost

This method quickly becomes impractical once VLAN count increases.
---

### 2️⃣ Router-on-a-Stick (ROAS)
To solve interface limitations, ROAS was introduced. A single trunk link connects the switch to the router, and sub-interfaces are created on the router—one per VLAN.

Therefore;
- Single trunk link between switch and router
- Router uses subinterfaces with 802.1Q tagging
- Saves interfaces but:
  - All inter-VLAN traffic must travel **to and from the router**
  - Creates **bottlenecks and congestion**
  - Performance limited by router throughput

This limitation becomes noticeable under heavy traffic loads.
---

### 3️⃣ Multilayer Switching (Best Practice ✅)
- Inter-VLAN routing occurs **inside the switch**
- Uses **SVIs (Switched Virtual Interfaces)**
- Eliminates router bottlenecks
- High-speed, wire-rate routing
- Scales efficiently for enterprise networks

> Multilayer switching combines **Layer 2 switching** and **Layer 3 routing** in a single device.

This lab focuses on implementing this final and optimal approach.
---

## 🧠 Understanding SVIs (Switched Virtual Interfaces)

With routing now moving to the multilayer switch, SVIs replace router sub-interfaces.

What Is an SVI?

An **SVI (Switched Virtual Interface)** is a **logical Layer 3 interface** associated with a VLAN on a multilayer switch.

It provides:

Default gateway for hosts in that VLAN

Routing between VLANs inside the switch

### SVI vs Access Port

| Feature | Access Port | SVI |
|------|-----------|----|
| OSI Layer | Layer 2 | Layer 3 |
| Purpose | Connects end devices | Acts as VLAN gateway |
| IP Address | ❌ No | ✅ Yes |
| Routes Traffic | ❌ No | ✅ Yes |

---

### ⚠️ Conditions for an SVI to Be `up/up`

An SVI will be **up/up** only if:
1. The VLAN exists on the switch  
2. At least **one access or trunk port** in that VLAN is **up/up**  
3. The switch is operating in **Layer 3 mode (`ip routing`)**

> Unlike access ports, creating an SVI for a **non-existent VLAN** will keep it **down/down**.

---

## 🔐 Native VLAN Change (VLAN 10)

In this lab, the **native VLAN is changed to VLAN 10**.

### Advantages:
- Avoids using default VLAN 1
- Reduces exposure to VLAN hopping attacks
- Improves security posture
- Matches enterprise best practices

---

## 🗺 Network Topology

### Devices Used
- 1 × Router (**R1**) — Internet connectivity
- 1 × Multilayer Switch (**SW2**) — Inter-VLAN routing
- 1 × Access Switch (**SW1**)
- 6 × PCs

---

## 📐 VLAN & IP Addressing Plan

### VLAN 10 — Engineering (`10.0.0.0/26`)
- Gateway (SVI): `10.0.0.62`

| Device | IP |
|------|----|
| PC1 | 10.0.0.1 |
| PC2 | 10.0.0.2 |
| PC6 | 10.0.0.4 |
| PC7 | 10.0.0.3 |

---

### VLAN 20 — HR (`10.0.0.64/26`)
- Gateway (SVI): `10.0.0.126`

| Device | IP |
|------|----|
| PC5 | 10.0.0.65 |

---

### VLAN 30 — Sales (`10.0.0.128/26`)
- Gateway (SVI): `10.0.0.190`

| Device | IP |
|------|----|
| PC3 | 10.0.0.129 |
| PC4 | 10.0.0.130 |

---

### Point-to-Point Link (SW2 ↔ R1)
- Network: `10.0.0.192/30`
- SW2: `10.0.0.193`
- R1: `10.0.0.194`

---

## 📂 Files

- **Packet Tracer File:**  
  `multilayer-switching-svi.pkt`

---

## ⚙️ Configuration Steps

---

## Step 1: Enable Layer 3 Routing on SW2

```bash
enable
configure terminal
ip routing

## Configure SVIs on SW2

interface vlan 10
 ip address 10.0.0.62 255.255.255.192
 no shutdown

interface vlan 20
 ip address 10.0.0.126 255.255.255.192
 no shutdown

interface vlan 30
 ip address 10.0.0.190 255.255.255.192
 no shutdown

## Configure Default Route on SW2

ip route 0.0.0.0 0.0.0.0 10.0.0.194

## Configure Layer 3 Link to Router

interface g1/0/2
 no switchport
 ip address 10.0.0.193 255.255.255.252
 no shutdown

