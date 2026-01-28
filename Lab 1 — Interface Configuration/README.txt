# 🧪 Lab 1 — Interface Configuration

## 📅 Date
2026-01-21

---

## 🎯 Objective

To configure network interfaces on Cisco devices, assign IP addresses, enable interfaces, and verify end-to-end connectivity using standard IOS verification commands.

This lab demonstrates a **foundational networking skill** required for routing, switching, security, and automation tasks.

---

## 🧠 Concepts Covered

- Interface configuration on routers and switches  
- IP addressing and subnet assignment  
- Interface states (administratively down vs up)  
- Basic Layer 3 connectivity testing  
- Verification using IOS show commands  

---

## 🗺 Topology

### Devices Used
- 1 × Router (R1)  
- 1–2 × Switches (SW1, SW2)  
- 2–4 × End devices (PCs)  

### Logical Overview
- Router interfaces act as default gateways  
- Switches provide Layer 2 connectivity  
- PCs communicate across subnets via the router  



---

## 📂 Files

- **Packet Tracer File:**  
  `Interface_configuration.pkt`

interface configuration topology.png
PC1_config.png 
R1_config.png 
SW1_config.png


> 📌 All lab files are stored within this lab folder to maintain a clean and consistent repository structure.

---

## ⚙️ Step-by-Step Configuration

### Step 1: Configure Hostnames

On each device:
```bash
enable
configure terminal
hostname R1

### Step 2: Configure Router Interface G0/0

   'Assign IP addresses and enable interfaces:'
```bash
interface g0/0
 description Connection to LAN 1
 ip address 172.16.255.254 255.255.0.0
 no shutdown
 exit


### Step 3: Verify Interface Status

show ip interface brief


### Step 4: Configure the 2 switches (SW1 & SW2)
(as directed in SW1_config.png)


### Step 5: Configure End Devices (PCs)
--On each PC:

  'Assign IP address within the subnet'

 'Set the default gateway to the router interface IP'


IP Address: 172.16.0.1

Subnet Mask: 255.255.0.0

Default Gateway: 172.16.255.254



✅ Verification

--eg From each PC1:

ping 172.16.0.2

📘 Lessons Learned 

Interface configuration is the foundation of all routing and switching tasks.

Verification commands are critical for confirming network health.

Small configuration errors can break connectivity.


















