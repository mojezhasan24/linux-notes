# Host Functioning and Communication

---

## Table of Contents
- [Core Concepts](#core-concepts)
- [The ARP Process](#the-arp-process-address-resolution-protocol)
- [Communication Logic](#communication-logic)
- [Foreign Network Communication](#foreign-network-communication)
- [Summary: Steps for Sending Data](#summary-steps-for-sending-data)

---

## Core Concepts

### Host Perspective

Hosts are **unaware** of whether they are:
- Directly connected to another host, or
- Connected via switches/hubs/routers

The communication process remains the same from the host's point of view.

### IP Addresses & Subnet Masks

- **IP Address:** Identifies the host on the network
- **Subnet Mask:** Determines which network the host belongs to
- Used together to determine if a target IP is on the **same local network** or a **foreign network**

### Data Encapsulation

When a host sends data:
1. Creates a **Layer 3 header** (IP addresses) for end-to-end delivery
2. Needs a **Layer 2 header** (MAC addresses) to interact with the physical wire
3. Headers are added at each layer before transmission

**See:** [OSI Model](osi-model.md) for detailed layer explanation

---

## The ARP Process (Address Resolution Protocol)

### Purpose

When a host lacks the MAC address of a target device, it uses ARP to resolve the IP address to a MAC address.

### Step-by-Step Process

#### 1. ARP Request

- Sent as a **broadcast** (destination MAC: all `F`s - `FF:FF:FF:FF:FF:FF`)
- Asks: *"Who has this IP address?"*
- All devices on the local network receive the request

#### 2. ARP Response

- Sent via **unicast** directly to the requesting host
- Only the target host (whose IP matches) responds
- Contains the target's MAC address

#### 3. ARP Cache

- Devices store IP-to-MAC mappings locally
- **Purpose:** Avoid repeated ARP requests for the same device
- Entries have a timeout period (typically 2-20 minutes)
- Future communications become faster

### Visual Flow

```
Host A: "Who has 192.168.1.100?" [BROADCAST]
         ↓
All hosts receive the request
         ↓
Host B (192.168.1.100): "I have it! My MAC is 00:1A:2B:3C:4D:5E" [UNICAST]
         ↓
Host A stores mapping in ARP cache
```

---

## Communication Logic

### Local vs. Foreign Network

When a host prepares to send data:

1. **Check destination IP** against subnet mask
2. **Determine** if target is on:
   - **Local network:** Same subnet → direct communication
   - **Foreign network:** Different subnet → must go through default gateway

### The Role of the Default Gateway

- **Definition:** The router's IP address on the local network
- **Purpose:** Exit point for traffic destined to foreign networks
- **Required configuration** alongside IP address and subnet mask

**Example:**
```
Host Configuration:
- IP Address: 192.168.1.10
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1
```

---

## Foreign Network Communication

### Process Overview

When communicating with a host on a different network:

1. **Identify** the destination IP address
2. **Determine** destination is on a foreign network (using subnet mask)
3. **Use ARP** to find the MAC address of the default gateway
4. **Populate** the ARP cache with gateway's MAC
5. **Create headers:**
   - **Layer 3 header:** Target host's IP (end-to-end)
   - **Layer 2 header:** Router's MAC (hop-to-hop)
6. **Send** the packet to the router

### Reusability

- Once the host has the router's MAC address in ARP cache:
  - It can **reuse** that entry for all foreign network communications
  - ARP resolution for the gateway happens only once (until cache expires)
  - Subsequent communications are faster

### Header Lifecycle

When the router receives the data:
1. **Strips** the Layer 2 header (MAC addresses)
2. **Reads** the Layer 3 header (IP addresses)
3. **Determines** next hop using routing table
4. **Creates new** Layer 2 header for next hop
5. **Forwards** the packet

**Key Point:** Layer 3 header (IP) remains constant throughout the journey, but Layer 2 header (MAC) changes at each hop.

---

## Post-Communication

Once the MAC address is known:
1. Host adds Layer 2 header to data for hop-to-hop delivery
2. Receiving host **discards headers layer-by-layer** to process application data
3. Future exchanges are faster because both hosts' ARP caches are populated

---

## Summary: Steps for Sending Data to a Foreign Network

1. **Identify** the destination IP address
2. **Determine** if destination is on local or foreign network (using subnet mask)
3. **If foreign:** Use ARP to find MAC address of default gateway
4. **Populate** the ARP cache
5. **Create** Layer 2 (hop-to-hop) and Layer 3 (end-to-end) headers
6. **Send** the packet to the router
7. **Router handles** the rest of the journey

---

## Related Topics
- [OSI Model](osi-model.md) - Understanding network layers
- [Hosts, Networks, and IP](hosts-network-and-IP.md) - Basic networking concepts
- [Router Functioning](router-functioning.md) - How routers handle packets
- [Switch Functioning](switch-functioning.md) - How switches forward frames
