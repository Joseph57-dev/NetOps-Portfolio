# 🧪 Lab 5 - VLAN Trunking & Router-on-a-Stick (ROAS)

## 📅 Date
2026-01-22

---

## 🎯 Objective

To configure **VLAN trunking using IEEE 802.1Q**, implement **Router-on-a-Stick (ROAS)** for inter-VLAN routing, and verify end-to-end connectivity across multiple switches and VLANs.

---

## 🧠 Key Concepts Covered

- Trunk ports and their purpose  
- IEEE 802.1Q VLAN tagging  
- Native VLAN configuration  
- Router-on-a-Stick (ROAS)  
- VLAN propagation across switches  
- Inter-VLAN communication  

---

## 🔁 What Is a Trunk Port?

A **trunk port** is a switch interface that:
- Carries traffic for **multiple VLANs**
- Uses **VLAN tags** to identify frames
- Commonly connects:
  - Switch-to-switch
  - Switch-to-router (ROAS)

Unlike access ports, trunk ports **do not belong to a single VLAN**.

---

## 🏷 IEEE 802.1Q Encapsulation

**802.1Q** is the standard used to identify VLANs across trunk links.

- Inserts a **VLAN tag** into Ethernet frames
- Tag contains:
  - VLAN ID
- Frames belonging to the **native VLAN** are **not tagged**
- All other VLANs are tagged

This allows multiple VLANs to share one physical link securely.

---

## 🔀 Router-on-a-Stick (ROAS)

**Router-on-a-Stick** is an inter-VLAN routing technique where:
- One physical router interface is used
- Multiple **subinterfaces** are created
- Each subinterface:
  - Belongs to a VLAN
  - Has an IP address (default gateway)
  - Uses 802.1Q encapsulation

ROAS reduces hardware requirements while enabling full VLAN routing especially it a point to note that the router has gotten very limited  interfaces.

---

## 🗺 Network Topology

### Devices Used
- 1 × Router (**R1**)
- 2 × Switches (**SW1**, **SW2**)
- 6 × PCs

---

## 📐 VLAN & IP Addressing Plan

### VLAN 10 — Engineering (`10.0.0.0/26`)
- Gateway: `10.0.0.62`

| Device | IP |
|------|----|
| PC1 | 10.0.0.1 |
| PC2 | 10.0.0.2 |
| PC6 | 10.0.0.4 |
| PC7 | 10.0.0.3 |

---

### VLAN 20 — HR (`10.0.0.64/26`)
- Gateway: `10.0.0.126`

| Device | IP |
|------|----|
| PC5 | 10.0.0.65 |

---

### VLAN 30 — Sales (`10.0.0.128/26`)
- Gateway: `10.0.0.190`

| Device | IP |
|------|----|
| PC3 | 10.0.0.129 |
| PC4 | 10.0.0.130 |

---

### Native VLAN
- VLAN **1001** (unused)

---

## 📂 Files

- **Packet Tracer File:**  
  `trunking-roas.pkt`

---

## ⚙️ Configuration Steps

---

### Step 1: Create VLANs on Both Switches

```bash
enable
configure terminal
vlan 10
 name Engineering
vlan 20
 name HR
vlan 30
 name Sales
vlan 999
 name Native_VLAN

### Step 2:Configure Access Ports (PC Connections)

SW1

interface range f0/1 - 2
 switchport mode access
 switchport access vlan 10

interface range f0/3 - 4
 switchport mode access
 switchport access vlan 30

SW2

interface range f0/1 - 2
 switchport mode access
 switchport access vlan 10

interface f0/3
 switchport mode access
 switchport access vlan 20

### Step 3: Configure Trunk Between SW1 and SW2

interface g0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 switchport trunk native vlan 1001

### Step 4: Configure Trunk Between SW2 and Router (ROAS)

interface g0/0
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 switchport trunk native vlan 1001

### Step 5: Configure Router-on-a-Stick (R1)

interface g0/0.10
 encapsulation dot1Q 10
 ip address 10.0.0.62 255.255.255.192

interface g0/0.20
 encapsulation dot1Q 20
 ip address 10.0.0.126 255.255.255.192

interface g0/0.30
 encapsulation dot1Q 30
 ip address 10.0.0.190 255.255.255.192

interface g0/0
 no shutdown


✅ Verification

Switch Checks
show vlan brief
show interfaces trunk

Router Checks
show ip interface brief

End-to-End Testing
ping <destination PC IP>
ie. ping 10.0.0.65






