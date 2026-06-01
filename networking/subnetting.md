# Subnetting

Subnetting divides a larger network into smaller logical subnets. It is used to improve routing efficiency, reduce broadcast domains, and organize IP address allocation.

---

## Table of Contents
- [Basic Concepts](#basic-concepts)
- [Why Subnet?](#why-subnet)
- [Subnet Mask and Prefix Length](#subnet-mask-and-prefix-length)
- [Calculating Subnets](#calculating-subnets)
- [Common Subnetting Patterns](#common-subnetting-patterns)
- [Binary View](#binary-view)
- [Practical Notes](#practical-notes)
- [Useful Commands](#useful-commands)
- [Tips](#tips)

---

## Basic Concepts

### IP Address

- A **32-bit number** in IPv4
- Written in **dotted decimal** form (e.g., `192.168.1.10`)
- Consists of **network portion** and **host portion**

### Network Mask / Subnet Mask

- A **32-bit mask** that separates network and host portions of an IP address
- Written like `255.255.255.0`
- **Network bits:** Represented by `1`s (fixed portion)
- **Host bits:** Represented by `0`s (variable portion)

### CIDR Notation

- **Classless Inter-Domain Routing** - compact representation of network size
- Example: `192.168.1.0/24`
- `/24` indicates **24 bits** of network prefix
- Also called **prefix length**

### Network Address

- The **first address** of a subnet
- All host bits are `0`
- **Cannot** be assigned to a host
- Identifies the subnet itself

### Broadcast Address

- The **last address** of a subnet
- All host bits are `1`
- Used to send data to **all hosts** in the subnet
- **Cannot** be assigned to a host

### Host Addresses

- The **usable addresses** between network and broadcast
- Assigned to devices (computers, printers, routers, etc.)
- Formula: `2^(host bits) - 2` (subtract network and broadcast)

---

## Why Subnet?

**Benefits of subnetting:**

1. **Reduce broadcast domain size**
   - Smaller broadcast domains = less network traffic
   - Improves overall network performance

2. **Improve network performance**
   - Less congestion from unnecessary traffic
   - Faster communication within subnets

3. **Enhance security**
   - Isolate sensitive departments (finance, HR)
   - Control traffic flow between subnets
   - Easier to implement access controls

4. **Efficient address allocation**
   - Avoid wasting IP addresses
   - Match subnet size to actual host requirements
   - Better utilization of address space

5. **Organizational structure**
   - Separate traffic by department, function, or location
   - Logical grouping independent of physical layout
   - Easier network management and troubleshooting

---

## Subnet Mask and Prefix Length

### Common Subnet Masks

| CIDR | Subnet Mask | Binary (Last Octet) | Total Addresses | Usable Hosts | Typical Use |
|------|-------------|---------------------|-----------------|--------------|-------------|
| /24  | 255.255.255.0 | `00000000` | 256 | 254 | Small office/home |
| /25  | 255.255.255.128 | `10000000` | 128 | 126 | Medium department |
| /26  | 255.255.255.192 | `11000000` | 64 | 62 | Small department |
| /27  | 255.255.255.224 | `11100000` | 32 | 30 | Team/group |
| /28  | 255.255.255.240 | `11110000` | 16 | 14 | Small team |
| /29  | 255.255.255.248 | `11111000` | 8 | 6 | Very small group |
| /30  | 255.255.255.252 | `11111100` | 4 | 2 | Point-to-point link |
| /31  | 255.255.255.254 | `11111110` | 2 | 0 | Point-to-point (special) |
| /32  | 255.255.255.255 | `11111111` | 1 | 0 | Single host |

### Key Formulas

- **Total addresses:** `2^(host bits)`
- **Usable hosts:** `2^(host bits) - 2` (subtract network & broadcast)
- **Host bits:** `32 - prefix length`
- **Number of subnets:** `2^(borrowed bits)`

---

## Calculating Subnets

### Step-by-Step Method

1. **Start** with the network address and mask
2. **Convert** the mask to binary or use the prefix length
3. **Determine the block size** for the subnet:
   - Block size = `256 - last octet value of subnet mask` (when subnetting on an octet boundary)
   - Or: Block size = `2^(host bits in last octet)`
4. **List subnets** by adding the block size to the subnet octet
5. **Identify** for each subnet:
   - Network address (first address)
   - First usable host
   - Last usable host
   - Broadcast address (last address)

### Example 1: /26 Subnet

Given: `192.168.1.0/26`

**Analysis:**
- Subnet mask: `255.255.255.192`
- Block size: `256 - 192 = 64`
- Number of subnets: `256 / 64 = 4` subnets
- Usable hosts per subnet: `64 - 2 = 62`

**Subnet Breakdown:**

| Subnet | Network Address | First Usable | Last Usable | Broadcast |
|--------|----------------|--------------|-------------|-----------|
| 1 | `192.168.1.0` | `192.168.1.1` | `192.168.1.62` | `192.168.1.63` |
| 2 | `192.168.1.64` | `192.168.1.65` | `192.168.1.126` | `192.168.1.127` |
| 3 | `192.168.1.128` | `192.168.1.129` | `192.168.1.190` | `192.168.1.191` |
| 4 | `192.168.1.192` | `192.168.1.193` | `192.168.1.254` | `192.168.1.255` |

### Example 2: Finding Subnet for a Host

**Question:** Which subnet does `192.168.1.100/27` belong to?

**Solution:**
1. Block size: `256 - 224 = 32`
2. Subnets: `.0`, `.32`, `.64`, `.96`, `.128`, ...
3. `100` falls between `96` and `128`
4. **Answer:** `192.168.1.96/27`
   - Network: `192.168.1.96`
   - Range: `192.168.1.97` - `192.168.1.126`
   - Broadcast: `192.168.1.127`

---

## Common Subnetting Patterns

### Subnet Division

Each additional prefix bit **doubles** the number of subnets and **halves** the number of hosts:

```
/24 (256 addresses, 254 hosts)
  ↓ Split into 2
/25 (128 addresses, 126 hosts each)
  ↓ Split into 2
/26 (64 addresses, 62 hosts each)
  ↓ Split into 2
/27 (32 addresses, 30 hosts each)
  ↓ Split into 2
/28 (16 addresses, 14 hosts each)
```

### Quick Reference

- `/24` splits into **two** `/25` subnets
- `/25` splits into **two** `/26` subnets
- `/26` splits into **two** `/27` subnets
- Each step down: **2× subnets, ½ hosts**
- Each step up: **½ subnets, 2× hosts**

---

## Binary View

### Understanding Binary Subnet Masks

**Example:** `255.255.255.224` = `/27`

**Binary representation:**
```
255   .   255   .   255   .   224
11111111.11111111.11111111.11100000
```

**Analysis:**
- **Network bits:** 27 (count the `1`s)
- **Host bits:** 5 (count the `0`s)
- **Total addresses:** `2^5 = 32`
- **Usable hosts:** `32 - 2 = 30` (subtract network & broadcast)

### Binary to Decimal Conversion

| Binary | Decimal | CIDR |
|--------|---------|------|
| `00000000` | 0 | /24 |
| `10000000` | 128 | /25 |
| `11000000` | 192 | /26 |
| `11100000` | 224 | /27 |
| `11110000` | 240 | /28 |
| `11111000` | 248 | /29 |
| `11111100` | 252 | /30 |
| `11111110` | 254 | /31 |
| `11111111` | 255 | /32 |

---

## Practical Notes

### Address Assignment Rules

- ❌ **Do NOT use** the network address for hosts
- ❌ **Do NOT use** the broadcast address for hosts
- ✅ **Use** addresses between network and broadcast
- ⚠️ **Exception:** `/31` and `/32` are special cases

### Private IPv4 Ranges (RFC 1918)

These ranges are reserved for internal networks and not routable on the internet:

| Range | CIDR | Addresses | Use Case |
|-------|------|-----------|----------|
| `10.0.0.0` - `10.255.255.255` | `10.0.0.0/8` | 16.7M | Large organizations |
| `172.16.0.0` - `172.31.255.255` | `172.16.0.0/12` | 1M | Medium organizations |
| `192.168.0.0` - `192.168.255.255` | `192.168.0.0/16` | 65K | Home/small office |

### Design Best Practices

1. **Plan for growth**
   - Allocate 20-30% extra addresses for future expansion
   - Avoid subnet sizes that are too tight

2. **Avoid wasting address space**
   - Match subnet size to actual host count
   - Use `/30` or `/31` for point-to-point links
   - Use `/24` or larger for user networks

3. **Use appropriate subnet sizes**
   - Consider current AND future host requirements
   - Document your subnet allocations
   - Keep subnets organized by function/location

---

## Useful Commands

### Linux Subnetting Commands

**Calculate subnet information:**
```bash
ipcalc 192.168.1.10/24
```
Output shows:
- Network address
- Broadcast address
- Host range
- Subnet mask

**View network configuration:**
```bash
ip addr show
```
Displays:
- IP addresses assigned to interfaces
- Subnet masks (in CIDR notation)
- Interface status

**View routing table:**
```bash
ip route show
```
Shows:
- Directly connected networks
- Default gateway
- Static routes

**Test connectivity:**
```bash
ping 192.168.1.1
traceroute 8.8.8.8
```

### Windows Alternatives

```cmd
ipconfig /all
route print
```

---

## Tips

### Quick Subnetting Tricks

1. **Memorize powers of two** for host counts:
   ```
   2^1 = 2      2^5 = 32     2^9 = 512
   2^2 = 4      2^6 = 64     2^10 = 1024
   2^3 = 8      2^7 = 128
   2^4 = 16     2^8 = 256
   ```

2. **Remember the pattern:**
   - A `/24` offers **254 hosts**
   - Each additional prefix bit **decreases hosts by half**
   - Each removed prefix bit **doubles the hosts**

3. **Block size shortcut:**
   - Look at the "interesting octet" (not 255 or 0)
   - Block size = `256 - octet value`
   - Example: `/26` → `255.255.255.192` → `256 - 192 = 64`

### Tools & Resources

- ✅ Use a **cheat sheet** or subnet calculator for exact ranges
- ✅ Practice with **subnetting exercises** online
- ✅ Use `ipcalc` for quick calculations in Linux
- ✅ Draw it out when learning (binary helps visualize)

### Common Mistakes to Avoid

- ❌ Forgetting to subtract 2 for usable hosts
- ❌ Confusing total addresses with usable hosts
- ❌ Misidentifying network vs. broadcast addresses
- ❌ Not checking if host IP is in the correct subnet

---

## Related Topics

- [Hosts, Networks & IP](hosts-network-and-IP.md) - IP addressing basics
- [OSI Model](osi-model.md) - Network layers
- [Router Functioning](router-functioning.md) - How routers use subnets
- [Networking Devices](networking-devices.md) - Device roles in subnetting

