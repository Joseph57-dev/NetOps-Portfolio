# 🧪 Lab 3 — VLSM Subnetting and Static Routing

## 📅 Date
2026-01-22

---

## 🎯 Objective

To design an efficient IP addressing scheme using **Variable Length Subnet Masking (VLSM)** from a single `/24` network and configure **static routing** to enable full connectivity between multiple LANs.

---

## 🧠 Concepts Covered

- VLSM subnet design  
- IPv4 address planning  
- Network ID, usable range, and broadcast calculation  
- Router and end-device interface configuration  
- Static routing between routers  
- End-to-end connectivity verification  

---

## 🗺 Network Topology

### Devices Used
- 2 × Routers  
  - **R1**: LAN 1 & LAN 2 gateway  
  - **R2**: LAN 3 & LAN 4 gateway  
- 1 × Switch per LAN  
- 1 × PC per LAN  

### Topology Description
- R1 serves LAN 1 and LAN 2  
- R2 serves LAN 3 and LAN 4  
- R1 and R2 are connected via a point-to-point link  
- All PCs must communicate using static routing  

*(Attached topology diagram screenshot)*

---

## 📂 Files

- **Packet Tracer File:**  
  `lab3 vlsm.pkt`

- **Config Backups:**  
  `R1-config.png`  
  `R2-config.png`

---

## 📐 Addressing Requirements

| Network | Hosts Required |
|------|---------------|
| LAN 1 | 45 |
| LAN 2 | 64 |
| LAN 3 | 14 |
| LAN 4 | 9 |
| R1 ↔ R2 | 2 |

---

## 🧮 VLSM Calculations  
**Base Network:** `192.168.5.0/24`

### Design Method (Simple Logic)
1. Start with the **largest LAN**
2. Allocate the **smallest possible subnet**
3. Move sequentially through the address space
4. Avoid wasting IP addresses

---

### 🔹 LAN 2 — 64 Hosts

- Required addresses: 64 + 2 = 66  
- Nearest power of 2: 128  
- Subnet: **/25** (`255.255.255.128`)

| Item | Address |
|----|--------|
| Network ID | 192.168.5.0 |
| First Usable | 192.168.5.1 |
| Last Usable | 192.168.5.126 |
| Broadcast | 192.168.5.127 |

---

### 🔹 LAN 1 — 45 Hosts

- Required addresses: 45 + 2 = 47  
- Nearest power of 2: 64  
- Subnet: **/26** (`255.255.255.192`)

| Item | Address |
|----|--------|
| Network ID | 192.168.5.128 |
| First Usable | 192.168.5.129 |
| Last Usable | 192.168.5.190 |
| Broadcast | 192.168.5.191 |

---

### 🔹 LAN 3 — 14 Hosts

- Required addresses: 14 + 2 = 16  
- Subnet: **/28** (`255.255.255.240`)

| Item | Address |
|----|--------|
| Network ID | 192.168.5.192 |
| First Usable | 192.168.5.193 |
| Last Usable | 192.168.5.206 |
| Broadcast | 192.168.5.207 |

---

### 🔹 LAN 4 — 9 Hosts

- Required addresses: 9 + 2 = 11  
- Nearest power of 2: 16  
- Subnet: **/28** (`255.255.255.240`)

| Item | Address |
|----|--------|
| Network ID | 192.168.5.208 |
| First Usable | 192.168.5.209 |
| Last Usable | 192.168.5.222 |
| Broadcast | 192.168.5.223 |

---

### 🔹 Router-to-Router Link

- Hosts needed: 2  
- Subnet: **/30** (`255.255.255.252`)

| Item | Address |
|----|--------|
| Network ID | 192.168.5.224 |
| R1 IP | 192.168.5.225 |
| R2 IP | 192.168.5.226 |
| Broadcast | 192.168.5.227 |

---

## 🧾 Address Assignment Summary

| Network | Subnet |
|------|-------|
| LAN 2 | 192.168.5.0/25 |
| LAN 1 | 192.168.5.128/26 |
| LAN 3 | 192.168.5.192/28 |
| LAN 4 | 192.168.5.208/28 |
| R1–R2 | 192.168.5.224/30 |

---

## ⚙️ Configuration Steps

### Router Interface Configuration
- Assign IP addresses according to the VLSM table
- Enable all interfaces
- Add interface descriptions
- Set hostnames

### Static Routing

#### On R1
```bash
ip route 192.168.5.192 255.255.255.240 192.168.5.226
ip route 192.168.5.208 255.255.255.240 192.168.5.226
