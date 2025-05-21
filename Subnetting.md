# Subnetting for Multi-Branch Office Network Setup

---

## Network Address
- **Base Network**: 198.168.1.0/24
- **Total Hosts**: 256 (including network and broadcast addresses)

---

## Requirements
1. Subnets for three branches:
   - Main Office: HR and IT departments
   - Branch1 Office: HR and IT departments
   - Branch2 Office: HR and IT departments
2. Three WAN links connecting the branches.

---

## Step-by-Step Subnetting Process

### Step 1: Calculate Subnet Mask
We need subnets for departments, WAN links, and future requirements. Use VLSM to optimize IP usage.

### Step 2: Allocate Subnets for Current and Future Needs

#### Subnet 1: Main Office
- **Departments**: HR and IT
- **Hosts Required**: ~30 hosts per department
- **Subnet Mask**: /27 (32 IPs per subnet, 30 usable IPs)
- **Allocation**:
  - HR: 198.168.1.0/27 (Usable: 198.168.1.1 - 198.168.1.30)
  - IT: 198.168.1.32/27 (Usable: 198.168.1.33 - 198.168.1.62)

#### Subnet 2: Branch1 Office
- **Departments**: HR and IT
- **Hosts Required**: ~30 hosts per department
- **Subnet Mask**: /27 (32 IPs per subnet, 30 usable IPs)
- **Allocation**:
  - HR: 198.168.1.64/27 (Usable: 198.168.1.65 - 198.168.1.94)
  - IT: 198.168.1.96/27 (Usable: 198.168.1.97 - 198.168.1.126)

#### Subnet 3: Branch2 Office
- **Departments**: HR and IT
- **Hosts Required**: ~30 hosts per department
- **Subnet Mask**: /27 (32 IPs per subnet, 30 usable IPs)
- **Allocation**:
  - HR: 198.168.1.128/27 (Usable: 198.168.1.129 - 198.168.1.158)
  - IT: 198.168.1.160/27 (Usable: 198.168.1.161 - 198.168.1.190)

#### Subnet 4: Branch3 Office
-**Departments**: HR and IT
-**Hosts Required**:
- HR: ~30 hosts
- IT: Small group (~6 hosts)
-**Subnet Masks**:
- HR: /27 (32 IPs per subnet, 30 usable)
- IT: /29 (8 IPs per subnet, 6 usable)
-**Allocation**:
- HR: 198.168.1.192/27
  - **Usable IP Range**: 198.168.1.193 – 198.168.1.222
  - **Broadcast Address**: 198.168.1.223
- IT: 198.168.1.248/29
  - **Usable IP Range**: 198.168.1.249 – 198.168.1.254
  - **Broadcast Address**: 198.168.1.255

#### Subnet 5: WAN Links
- **Hosts Required**: 3 hosts per WAN link
- **Subnet Mask**: /30 (4 IPs per subnet, 2 usable IPs)
- **Allocation**:
  - WAN 1: 198.168.1.224/30 (Usable: 198.168.1.225 - 198.168.1.226)
  - WAN 2: 198.168.1.228/30 (Usable: 198.168.1.229 - 198.168.1.230)
  - WAN 3: 198.168.1.232/30 (Usable: 198.168.1.233 - 198.168.1.234)
#### Reserved Subnets for Future Needs
- **Future WAN Links**:
  - 198.168.1.232/30 (Usable: 198.168.1.233 - 198.168.1.234)
  - 198.168.1.236/30 (Usable: 198.168.1.237 - 198.168.1.238)
  - 198.168.1.240/30 (Usable: 198.168.1.241 - 198.168.1.242)
  - 198.168.1.244/30 (Usable: 198.168.1.245 - 198.168.1.246)
  - 198.168.1.248/30 (Usable: 198.168.1.249 - 198.168.1.250)
  - 198.168.1.252/30 (Usable: 198.168.1.253 - 198.168.1.254)

---

## Final Subnet Allocation Table

| Subnet         | CIDR           | Range                  | Usable IPs              | Purpose                     |
|----------------|----------------|------------------------|-------------------------|-----------------------------|
| Subnet 1       | 198.168.1.0/27 | 198.168.1.0 - 198.168.1.31   | 198.168.1.1 - 198.168.1.30   | Main HR            |
| Subnet 2       | 198.168.1.32/27| 198.168.1.32 - 198.168.1.63  | 198.168.1.33 - 198.168.1.62  | Main IT            |
| Subnet 3       | 198.168.1.64/27| 198.168.1.64 - 198.168.1.95  | 198.168.1.65 - 198.168.1.94  | Branch1 HR         |
| Subnet 4       | 198.168.1.96/27| 198.168.1.96 - 198.168.1.127 | 198.168.1.97 - 198.168.1.126 | Branch1 IT         |
| Subnet 5       | 198.168.1.128/27| 198.168.1.128 - 198.168.1.159| 198.168.1.129 - 198.168.1.158| Branch2 HR        |
| Subnet 6       | 198.168.1.160/27| 198.168.1.160 - 198.168.1.191| 198.168.1.161 - 198.168.1.190| Branch2 IT        |
| Subnet 7       | 198.168.1.192/27| 198.168.1.192 - 198.168.1.223| 198.168.1.192 - 198.168.1.223| Branch3 HR        |
| Subnet 8       | 198.168.1.248/29| 198.168.1.248 - 198.168.1.255| 198.168.1.249 - 198.168.1.254|  Branch3 IT       |
| Subnet 9       | 198.168.1.224/30| 198.168.1.224 - 198.168.1.227| 198.168.1.225 - 198.168.1.226| WAN 1                      |
| Subnet 10      | 198.168.1.228/30| 198.168.1.228 - 198.168.1.231| 198.168.1.229 - 198.168.1.230| WAN 2                      |
| Subnet 11      | 198.168.1.232/30| 198.168.1.232 - 198.168.1.235| 198.168.1.233 - 198.168.1.234| WAN 3                      |
| Reserved WAN 1 | 198.168.1.236/30| 198.168.1.236 - 198.168.1.239| 198.168.1.237 - 198.168.1.238| Reserved for future WAN    |
| Reserved WAN 2 | 198.168.1.240/30| 198.168.1.240 - 198.168.1.243| 198.168.1.241 - 198.168.1.242| Reserved for future WAN    |
| Reserved WAN 3 | 198.168.1.244/30| 198.168.1.244 - 198.168.1.247| 198.168.1.245 - 198.168.1.246| Reserved for future WAN    |


---

This configuration ensures efficient IP address allocation while keeping scalability for future needs.

