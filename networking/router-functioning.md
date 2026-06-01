# Router Functioning

---

## Table of Contents
- [Core Concepts](#core-concepts)
- [The Routing Table](#the-routing-table)
- [ARP Tables](#arp-tables)
- [Packet Forwarding Process](#packet-forwarding-process)
- [Communication Walkthrough](#communication-walkthrough)
- [Router Hierarchies and Route Summarization](#router-hierarchies-and-route-summarization)

---

## Core Concepts

### Routing vs. Switching

| Aspect | Switching | Routing |
|--------|-----------|---------|
| **Scope** | Moves data **within** a network | Moves data **between** different networks |
| **Layer** | Layer 2 (Data Link) | Layer 3 (Network) |
| **Addressing** | MAC addresses | IP addresses |

**See:** [Networking Devices](networking-devices.md) for device comparison

### What is a Router?

**Definition (RFC 2460):** A router is a node that forwards packets **not addressed to itself**.

**Key differences from hosts:**
- **Hosts:** Drop traffic not meant for them
- **Routers:** Designed to direct traffic to its destination

### Network Connectivity

Routers require:
- A **unique IP address** for every network interface
- A **unique MAC address** for every network interface
- One interface per connected network

**Example:**
```
Router with 3 interfaces:
- Interface 1: 192.168.1.1 (Network A)
- Interface 2: 10.0.0.1 (Network B)
- Interface 3: 172.16.0.1 (Network C)
```

---

## The Routing Table

Every router maintains a **routing table**, which serves as a map of known networks and instructions on how to reach them.

### Three Ways Routing Tables are Populated

#### 1. Directly Connected Routes

- **Created automatically** when a router is physically connected to a network
- No manual configuration required
- Most trusted route type

**Example:**
```
Router plugged into 192.168.1.0/24
→ Automatic route: 192.168.1.0/24 via Interface 1
```

#### 2. Static Routes

- **Manually configured** by a network administrator
- Defines paths to networks not directly connected
- Requires manual updates when network changes

**Use cases:**
- Small networks
- Specific traffic control
- Backup routes

**Example:**
```
Static route configuration:
Destination: 10.0.0.0/8
Next hop: 192.168.1.254
```

#### 3. Dynamic Routes

- **Learned automatically** through dynamic routing protocols
- Routers share information about their known networks
- Adapts to network changes automatically

**Common protocols:**
- **OSPF** (Open Shortest Path First)
- **EIGRP** (Enhanced Interior Gateway Routing Protocol)
- **BGP** (Border Gateway Protocol)

**Advantages:**
- Automatic convergence
- Scales well for large networks
- Fault tolerance

---

## ARP Tables

### Difference from Routing Tables

| Feature | Routing Table | ARP Table |
|---------|--------------|-----------|
| **Population** | Pre-populated (connected/static/dynamic) | Starts empty, populated dynamically |
| **Purpose** | Maps networks to next hops | Maps IP addresses to MAC addresses |
| **Scope** | All known networks | Only directly connected networks |
| **Layer** | Layer 3 | Layer 2 |

### How ARP Tables Work

- Map **Layer 3 (IP)** addresses to **Layer 2 (MAC)** addresses
- Only for nodes in **directly connected** networks
- Populated on-demand when needed
- Entries expire after timeout period

---

## Packet Forwarding Process

When a router receives a packet:

### Step-by-Step Process

1. **Receive** packet on incoming interface
2. **Discard** the Layer 2 header (MAC addresses)
3. **Read** the Layer 3 header (destination IP)
4. **Look up** destination IP in routing table
5. **Determine** next hop and outgoing interface
6. **Check ARP table** for next hop's MAC address
   - If not found: Perform ARP request
7. **Construct new** Layer 2 header with:
   - Source MAC: Router's outgoing interface MAC
   - Destination MAC: Next hop's MAC
8. **Forward** packet out of outgoing interface

### Key Points

- **Layer 3 header (IP)** remains constant throughout journey
- **Layer 2 header (MAC)** changes at every hop
- Router makes independent decision at each hop

---

## Communication Walkthrough

### Host A to Host C (Forward Path)

```
Host A (192.168.1.10) → Host C (10.0.0.50)
```

**Process:**
1. Host A identifies destination (10.0.0.50) is on foreign network
2. Host A resolves default gateway's MAC via ARP
3. Host A sends packet to router with:
   - Layer 3: Destination IP = 10.0.0.50
   - Layer 2: Destination MAC = Router's MAC
4. Router receives packet, strips Layer 2 header
5. Router checks routing table for 10.0.0.50
6. Router determines next hop
7. Router performs ARP if needed
8. Router creates new Layer 2 header and forwards
9. Process repeats at each router until packet reaches Host C

### Response (Return Path)

```
Host C (10.0.0.50) → Host A (192.168.1.10)
```

**Why it's faster:**
- ARP entries already populated in routers' tables
- No need for ARP requests (unless entries expired)
- Same routing process, but reduced delay

---

## Router Hierarchies and Route Summarization

### Router Hierarchies

**Why use hierarchical design?**

1. **Scalability:**
   - Easy to add new segments (accounting, help desk, etc.)
   - Minimal impact on existing infrastructure

2. **Consistent Connectivity:**
   - **Linear topology:** Performance varies based on hop count
   - **Hierarchical topology:** Keeps paths short and uniform
   - Better user experience

3. **Manageability:**
   - Clear network boundaries
   - Easier troubleshooting
   - Logical organization

### Route Summarization

**Definition:** Grouping multiple specific routes into a single, less specific entry.

**Example:**
```
Instead of:
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24
192.168.4.0/24

Summarized as:
192.168.0.0/16 (represents all 192.168.x.x networks)
```

**Benefits:**
- Significantly reduces routing table size
- Cleaner, more efficient routing
- Less memory usage
- Faster lookups
- Router only needs path to "summary" prefix, not every sub-network

### Default Route

**Definition:** The "ultimate" route summary

**Notation:** `0.0.0.0/0`

**Purpose:**
- Catch-all route for unmatched destinations
- Tells router: "If no specific match found, send here"
- Effective for reaching external networks/internet

**Example:**
```
Default Route:
Destination: 0.0.0.0/0
Next hop: 203.0.113.1 (ISP router)
```

### IP Address Notation (CIDR)

CIDR (Classless Inter-Domain Routing) notation indicates how many bits must match:

| Notation | Subnet Mask | Hosts | Use Case |
|----------|-------------|-------|----------|
| `/32` | 255.255.255.255 | 1 | Single host |
| `/24` | 255.255.255.0 | 254 | Small network |
| `/16` | 255.255.0.0 | 65,534 | Medium network |
| `/8` | 255.0.0.0 | 16M+ | Large network |
| `/0` | 0.0.0.0 | All | Default route |

**How it works:**
- `/24` means first 24 bits (3 octets) must match
- `/16` means first 16 bits (2 octets) must match
- Router uses longest prefix match (most specific route wins)

---

## Key Takeaway

The internet functions as a large series of routers handing off packets to one another using this identical process, regardless of how many routers are in the path. Each router:
1. Strips the old Layer 2 header
2. Checks the Layer 3 header
3. Determines next hop from routing table
4. Creates new Layer 2 header
5. Forwards the packet

---

## Related Topics
- [OSI Model](osi-model.md) - Understanding network layers
- [Host Functioning](host-functioning.md) - How hosts communicate
- [Networking Devices](networking-devices.md) - Device types and roles
- [Switch Functioning](switch-functioning.md) - How switches work
