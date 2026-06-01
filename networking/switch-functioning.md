# Switch Functioning

---

## Table of Contents
- [Switching Defined](#switching-defined)
- [Layer 2 Operations](#layer-2-operations)
- [MAC Address Table](#mac-address-table)
- [Three Fundamental Switch Actions](#three-fundamental-switch-actions)
- [Unicast Flooding vs. Broadcasts](#unicast-flooding-vs-broadcasts)
- [VLANs (Virtual Local Area Networks)](#vlans-virtual-local-area-networks)
- [Multiple Switches](#multiple-switches)

---

## Switching Defined

**Definition:** Switching is the process of moving data **within** networks.

**Key Point:** Any device performing this function follows the same set of rules.

**Contrast with Routing:**
- **Switching:** Moving data within a network (Layer 2)
- **Routing:** Moving data between networks (Layer 3)

**See:** [Networking Devices](networking-devices.md) for device comparison

---

## Layer 2 Operations

**Switch Layer:** Data Link Layer (Layer 2)

**How switches make decisions:**
- Based **solely** on the Layer 2 header (MAC addresses)
- **Ignore** Layer 3 (IP) headers completely
- Make forwarding decisions at hardware speed

**Implications:**
- Faster than routers (less processing)
- Cannot route between different networks
- Operates within a single broadcast domain (unless VLANs are used)

---

## MAC Address Table

**Also known as:** CAM table (Content Addressable Memory)

**Purpose:** Maps specific switch ports to the MAC addresses of connected devices.

**Characteristics:**
- Starts **empty** when switch powers on
- Populated **dynamically** as data flows through the switch
- Entries have a timeout period (typically 5 minutes)
- Stored in high-speed memory for fast lookups

**Example:**
```
MAC Address Table:
MAC Address        Port    VLAN
00:1A:2B:3C:4D:5E  Fa0/1   1
00:1A:2B:3C:4D:5F  Fa0/2   1
00:1A:2B:3C:4D:60  Fa0/3   1
00:1A:2B:3C:4D:61  Fa0/24  10
```

---

## Three Fundamental Switch Actions

These are **the rules of switching** — any device that performs switching follows these rules.

### 1. Learning

**When:** Every time a frame enters the switch

**Process:**
1. Inspect the **source MAC address** of incoming frame
2. Record which **port** that device is connected to
3. Add/update entry in MAC address table

**Purpose:** Build a map of which devices are on which ports

**Example:**
```
Frame arrives on Port 1
Source MAC: 00:1A:2B:3C:4D:5E
→ Switch learns: "00:1A:2B:3C:4D:5E is on Port 1"
```

### 2. Flooding

**When:** Switch does **not** know the destination MAC address

**Process:**
1. Duplicate the frame
2. Send it out of **all ports** EXCEPT the port it was received on

**Purpose:** Ensure the frame reaches its destination even when location is unknown

**Scenarios:**
- Unknown unicast (destination MAC not in table)
- Broadcast frames (destination MAC = all F's)
- Multicast frames (without IGMP snooping)

**Example:**
```
Frame received on Port 1
Destination MAC: 00:1A:2B:3C:4D:99 (not in table)
→ Switch floods to: Port 2, 3, 4, 5, ..., 24
```

### 3. Forwarding

**When:** Destination MAC address **is** in the MAC address table

**Process:**
1. Look up destination MAC in table
2. Send frame **only** to the specific port associated with that address

**Purpose:** Efficient delivery without unnecessary traffic

**Example:**
```
Frame received on Port 1
Destination MAC: 00:1A:2B:3C:4D:5E
Table shows: 00:1A:2B:3C:4D:5E → Port 3
→ Switch forwards to: Port 3 only
```

### Decision Flow

```
Frame arrives
    ↓
Learn source MAC (update table)
    ↓
Check destination MAC in table
    ↓
    ├── Found? → Forward to specific port
    └── Not found? → Flood to all ports (except incoming)
```

---

## Traffic Flow

### Through the Switch (Transit Traffic)

- Switch uses the three fundamental actions (Learning, Flooding, Forwarding)
- Makes decisions based on MAC addresses
- Does not examine IP headers

### To the Switch (Management Traffic)

- Switch acts like any other host
- Requires its own **IP address** and **MAC address**
- Used for:
  - Remote management (SSH, web interface)
  - Monitoring (SNMP)
  - Configuration

---

## Unicast Flooding vs. Broadcasts

### Unicast

- **Definition:** One-to-one communication
- **Destination MAC:** Specific device's MAC address
- **Switch behavior:**
  - If MAC in table → **Forward** to specific port
  - If MAC not in table → **Flood** to all ports

### Broadcast

- **Definition:** One-to-all communication
- **Destination MAC:** All F's (`FF:FF:FF:FF:FF:FF`)
- **Switch behavior:** **Always flood** to all other ports

**Example:** ARP requests are broadcasts
```
ARP Request:
"Who has 192.168.1.100?"
Destination MAC: FF:FF:FF:FF:FF:FF
→ Switch floods to all ports
```

### Key Distinction

| Term | What it is | Switch Action |
|------|-----------|---------------|
| **Flood** | An **action** a switch takes | Send to all ports (except incoming) |
| **Broadcast** | A **type** of frame | Always triggers flooding |

**Important:** "Flood" is what the switch does; "Broadcast" is a type of frame that always causes flooding.

---

## VLANs (Virtual Local Area Networks)

### Purpose

Logically divide a single physical switch into **isolated groups**.

**Benefits:**
- **Security:** Separate sensitive traffic (e.g., accounting, HR)
- **Performance:** Reduce broadcast domain size
- **Organization:** Group by function, not physical location
- **Flexibility:** Logical grouping independent of physical wiring

### How VLANs Work

- Switch maintains **separate MAC address tables** for each VLAN
- Fundamental actions (Learning, Flooding, Forwarding) occur **independently** within each VLAN
- Devices in different VLANs **cannot** communicate directly (requires router)

**Example:**
```
Physical Switch with VLANs:
- VLAN 20: Engineering (Ports 1-8)
- VLAN 30: Marketing (Ports 9-16)
- VLAN 40: Management (Ports 17-24)

Result:
- Engineering traffic stays in VLAN 20
- Marketing traffic stays in VLAN 30
- Complete isolation between VLANs
```

### VLAN Communication

- Devices in same VLAN: Communicate directly via switching
- Devices in different VLANs: Require routing (Layer 3 device)

---

## Multiple Switches

### Independent Operation

When multiple switches are connected:
- Each switch maintains its **own independent** MAC address table
- They **do not share** information with each other
- Each switch performs the three fundamental actions independently

### Frame Traversal

```
Host A → Switch 1 → Switch 2 → Host B

Switch 1:
- Learns Host A's MAC on Port 1
- Forwards to Switch 2 (if Host B's MAC known) or floods

Switch 2:
- Learns Host A's MAC on uplink port
- Forwards to Host B (if MAC known) or floods
```

### Multiple MACs per Port

It's **normal** for a single switch port to have multiple MAC address mappings:

**Scenarios:**
- Port connected to another switch
- Port connected to a hub with multiple devices
- Port with virtual machines

**Example:**
```
Port 24 (connected to another switch):
MAC Addresses learned on Port 24:
- 00:1A:2B:3C:4D:5E
- 00:1A:2B:3C:4D:5F
- 00:1A:2B:3C:4D:60
- 00:1A:2B:3C:4D:61
(All devices on the other switch)
```

---

## Key Takeaway

Any device that claims to perform "switching" will follow these same rules:
1. **Learn** source MAC addresses
2. **Flood** when destination is unknown
3. **Forward** when destination is known

These fundamental actions enable efficient, intelligent traffic management within local networks.

---

## Related Topics
- [Networking Devices](networking-devices.md) - Device types and roles
- [Router Functioning](router-functioning.md) - How routers route between networks
- [OSI Model](osi-model.md) - Understanding Layer 2 vs Layer 3
- [Host Functioning](host-functioning.md) - How hosts communicate