# 🧪 Lab 4 — VLANs (Access Ports & Inter-VLAN Communication)

## 📅 Date
2026-01-22

---

## 🎯 Objective

To implement **Virtual LANs (VLANs)** on a switch using **access ports**, configure IP addressing on end devices, and enable **inter-VLAN communication** via a router, while demonstrating how VLANs improve **network efficiency, performance, and security**.

---

## 🧠 What Is a VLAN?

A **VLAN (Virtual Local Area Network)** is a logical segmentation of a physical network that divides a switch into **multiple broadcast domains**.

Instead of all devices sharing one broadcast domain, VLANs:
- Limit broadcast traffic
- Improve performance
- Enhance security by isolating departments logically

Even if devices are connected to the **same physical switch**, VLANs ensure that traffic is separated unless routing is explicitly allowed.

---

## 🔐 Why VLANs Matter (Performance & Security)

### Without VLANs:
- Broadcast frames are flooded to **all switch ports**
- Unknown unicast frames are also flooded
- Sensitive traffic can reach unintended devices
- Excessive traffic reduces performance

### With VLANs:
- Broadcasts stay **within the VLAN**
- Unknown unicasts are limited to VLAN members
- Departments are logically isolated
- Security and efficiency are significantly improved

> VLANs reduce unnecessary traffic and prevent broadcast “bleeding” across the network.

---

## 🧠 Broadcasts & Unknown Unicasts Explained

- **Broadcast traffic** (e.g., ARP requests) is sent to all ports **within the same VLAN**
- **Unknown unicast traffic** (destination MAC not in the MAC table) is flooded only within the VLAN
- VLAN boundaries prevent this traffic from reaching other VLANs

This containment is a **key security control** in modern switched networks.

---

## 🗺 Network Topology

### Devices Used
- 1 × Router (**R1**)
- 1 × Switch (**SW1**)
- 6 × PCs (2 per VLAN)

### VLANs Configured

| VLAN ID | Name | Subnet |
|------|-----------|--------------|
| 10 | Engineering | 10.0.0.0/26 |
| 20 | HR | 10.0.0.64/26 |
| 30 | Sales | 10.0.0.128/26 |

### Topology Description
- All PCs connect to **SW1**
- SW1 connects to **R1 using three separate physical interfaces**
- Each router interface serves as the **default gateway** for one VLAN
- Only **access ports** are used (no trunking in this lab)

*(Attach Packet Tracer topology screenshot here)*

---

## 📂 Files

- **Packet Tracer File:**  
  `vlan-access-ports.pkt`

---

## 📐 IP Addressing Plan

### VLAN 10 — Engineering (10.0.0.0/26)
- Network ID: `10.0.0.0`
- Broadcast: `10.0.0.63`
- Gateway (R1): `10.0.0.62`

| Device | IP Address |
|------|------------|
| PC1 | 10.0.0.1 |
| PC2 | 10.0.0.2 |

---

### VLAN 20 — HR (10.0.0.64/26)
- Network ID: `10.0.0.64`
- Broadcast: `10.0.0.127`
- Gateway (R1): `10.0.0.126`

| Device | IP Address |
|------|------------|
| PC3 | 10.0.0.65 |
| PC4 | 10.0.0.66 |

---

### VLAN 30 — Sales (10.0.0.128/26)
- Network ID: `10.0.0.128`
- Broadcast: `10.0.0.191`
- Gateway (R1): `10.0.0.190`

| Device | IP Address |
|------|------------|
| PC5 | 10.0.0.129 |
| PC6 | 10.0.0.130 |

---

## 🔁 How Traffic Moves Between VLANs

1. PC1 sends traffic to a device in another VLAN
2. PC1 encapsulates the packet with:
   - **Destination MAC = Router (default gateway)**
3. The switch forwards the frame to R1
4. R1:
   - Removes the Layer 2 header
   - Routes the packet at Layer 3
   - Re-encapsulates it with the **destination PC’s MAC**
5. The frame is sent back to SW1 and delivered to the target VLAN

> Routers **rewrite MAC addresses** but preserve IP addresses.

---

## ⚙️ Configuration Steps

### Step 1: Configure VLANs on SW1

```bash
enable
configure terminal
vlan 10
 name Engineering
vlan 20
 name HR
vlan 30
 name Sales


🧠 Access Port Definition

An access port:

Belongs to one VLAN only

Connects to end devices such as PCs

Does not carry VLAN tags


###Step 2: Assign Switch Ports to VLANs (Batch Configuration)

interface range f0/1 - 2
 switchport mode access
 switchport access vlan 10

interface range f0/3 - 4
 switchport mode access
 switchport access vlan 20

interface range f0/5 - 6
 switchport mode access
 switchport access vlan 30


Step 3: Configure Router Interfaces (R1)

interface g0/0
 description Engineering VLAN Gateway
 ip address 10.0.0.62 255.255.255.192
 no shutdown

interface g0/1
 description HR VLAN Gateway
 ip address 10.0.0.126 255.255.255.192
 no shutdown

interface g0/2
 description Sales VLAN Gateway
 ip address 10.0.0.190 255.255.255.192
 no shutdown


###Step 4: Configure PCs

Assign IP address

Assign subnet mask

Set default gateway as the router interface IP


✅ Verification

Switch Verification
show vlan brief

Router Verification
show ip interface brief



---

## ✅ This README;

✔ Shows **deep understanding**, not just commands  
✔ Explains **traffic flow clearly**  
✔ Emphasizes **security & performance**  
✔ Clean, recruiter-friendly documentation  
✔ Perfect transition into **trunking & ROAS labs**

---

Next logical labs:
- 🔀 **Router-on-a-Stick**
- 🧠 **VLAN + Trunk Security**
- 🔐 **Port Security**
- ⚙️ **Inter-VLAN Routing on Layer 3 Switch**


