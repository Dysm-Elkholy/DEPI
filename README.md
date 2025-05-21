# Multi-Branch Office Network Setup

---

### Offices and Departments
- **Main Office**: HR and IT
- **Branch1 Office**: HR and IT
- **Branch2 Office**: HR and IT
- **Two WAN Links**: WAN 1 and WAN 2

---

## Subnetting Details

The network `198.168.1.0/24` is divided using a mix of fixed-length and variable-length subnet masks (VLSM) to efficiently allocate IP addresses. Below are the details:

### VLSM Subnet Allocation for WAN Links
To allocate only two usable IPs per WAN link, the `198.168.1.224/27` subnet was further divided into smaller subnets using a `/30` mask:

| Subnet | Range                  | Usable IPs        | Purpose  |
|--------|------------------------|-------------------|----------|
| 1      | 198.168.1.224/30      | 198.168.1.225-226 | WAN 1    |
| 2      | 198.168.1.228/30      | 198.168.1.229-230 | WAN 2    |
| 3      | 198.168.1.232/30      | 198.168.1.233-234 | Reserved |
| 4      | 198.168.1.236/30      | 198.168.1.237-238 | Reserved |
| 5      | 198.168.1.240/30      | 198.168.1.241-242 | Reserved |
| 6      | 198.168.1.244/30      | 198.168.1.245-246 | Reserved |


### Subnet Allocation for Branches
The remaining subnets were divided using a `/27` mask:

| Subnet | Range                 | Usable IPs                | Location              |
|--------|-----------------------|---------------------------|-----------------------|
| 1      | 198.168.1.0/27       | 198.168.1.1 - 198.168.1.30 | Main HR         |
| 2      | 198.168.1.32/27      | 198.168.1.33 - 198.168.1.62 | Main IT         |
| 3      | 198.168.1.64/27      | 198.168.1.65 - 198.168.1.94 | Branch1 HR           |
| 4      | 198.168.1.96/27      | 198.168.1.97 - 198.168.1.126 | Branch1 IT           |
| 5      | 198.168.1.128/27     | 198.168.1.129 - 198.168.1.158 | Branch2 HR       |
| 6      | 198.168.1.160/27     | 198.168.1.161 - 198.168.1.190 | Branch2 IT       |
| 7      | 198.168.1.192/27     | 198.168.1.193 - 198.168.1.222 | Branch3 HR       |
| 8      | 198.168.1.248/29     | 198.168.1.249 - 198.168.1.254 | Branch3 IT       |
| 9      | 198.168.1.224/27     | 198.168.1.225 - 198.168.1.254 | Reserved for WAN Links |

---

## Subnetting Calculation Details

### Formula
- **Subnet Mask for `/30`**:
  - Block size: `2^(32 - subnet bits) = 2^(32 - 30) = 4`
  - Usable IPs: Block size - 2 (network and broadcast addresses)
  - Usable IPs per subnet: 2
- **Subnet Mask for `/27`**:
  - Block size: `2^(32 - 27) = 32`
  - Usable IPs: Block size - 2
  - Usable IPs per subnet: 30

### Steps for WAN Link Subnetting
1. Start with `198.168.1.224/27`.
2. Subdivide into `/30` subnets:
   - Subnet 1: `198.168.1.224/30` (Usable: `198.168.1.225-226`)
   - Subnet 2: `198.168.1.228/30` (Usable: `198.168.1.229-230`)
   - Subnet 3: `198.168.1.232/30` (Usable: `198.168.1.233-234`)
   - Subnet 4: `198.168.1.236/30` (Usable: `198.168.1.237-238`)
   - Subnet 5: `198.168.1.240/30` (Usable: `198.168.1.241-242`)
   - Subnet 6: `198.168.1.244/30` (Usable: `198.168.1.245-246`)

### Steps for Department Subnetting
1. Allocate `/27` subnets for each department.
2. Assign the subnets in sequence:
   - Main HR: `198.168.1.0/27`
   - Main IT: `198.168.1.32/27`
   - Branch1 HR: `198.168.1.64/27`
   - Branch1 IT: `198.168.1.96/27`
   - Branch2 HR: `198.168.1.128/27`
   - Branch2 IT: `198.168.1.160/27`
   - Branch3 HR: `198.168.1.192/27`
   - Branch3 IT: `198.168.1.248/29`

---

## Router Configuration

### PAT Setup Commands
Below are the PAT configuration commands for setting up NAT on the routers:

```shell
Router> enable
Router# configure terminal
Router(config)# interface gig0/0/0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface gig0/0/1
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface se0/2/0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface se0/2/1
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface se0/1/0
Router(config-if)# ip nat outside
Router(config-if)# exit

Router(config)# access-list 10 permit 198.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 10 interface se0/1/0 overload
Router(config)# exit
```

### Static Routing Example
Static routes must be configured to enable communication between the branches via the WAN links. Example:

```shell
Router(config)# ip route 198.168.1.64 255.255.255.224 <WAN_next_hop_IP>
Router(config)# ip route 198.168.1.128 255.255.255.224 <WAN_next_hop_IP>
```

---

## Verification Steps

### Testing Branch Communication
1. **Ping Between Devices**:
   - From a device in Main HR (Subnet 1), ping a device in Branch1 IT (Subnet 4).
   - Example command:
     ```shell
     ping 198.168.1.100
     ```
2. Ensure devices in all subnets can communicate across the WAN links.

### Testing Internet Access
1. On any PC in the network, configure the default gateway and DNS.
2. Test internet access by pinging a public IP (e.g., `8.8.8.8`).
3. Open a browser and access a website to confirm connectivity.

---

## Deliverables

1. **Network Topology**:
   - Fully functional in Cisco Packet Tracer.
2. **Configuration Files**:
   - Subnetting details for each department.
   - Static routing setup.
   - PAT configuration for internet access.
3. **Testing Verification**:
   - Documentation of successful pings between branches.
   - Proof of internet access from any PC.

---
## Teammates
-Omar Waleed Mohamed

-Youssef Osama Gamal

-Abdelrahman Bahaa

-Omar Magdy

-Seif Sayed

---
**End of Document**
