# Networking Devices

---

## Table of Contents
- [Signal Management](#1-signal-management)
- [Early Scaling Devices](#2-early-scaling-devices)
- [Modern Network Components](#3-modern-network-components)
- [Hierarchical Concepts](#4-hierarchical-concepts)

---

## 1. Signal Management

### Repeater

**Purpose:** Regenerate signals that decay over long distances.

**How it works:**
- Receives weak/degraded signals
- Amplifies and retransmits them at original strength
- Ensures connectivity between hosts that are far apart

**Layer:** Physical Layer (Layer 1)

**Use case:** Extending network cable runs beyond standard distance limitations

---

## 2. Early Scaling Devices

### Hub

**Definition:** A multi-port repeater.

**How it works:**
- Duplicates incoming data
- Sends it out to **all** remaining ports

**Problem:**
- All connected devices receive every packet
- Creates unnecessary network traffic
- Inefficient communication
- Security concern (all devices see all traffic)

**Layer:** Physical Layer (Layer 1)

### Bridge

**Definition:** A device that sits between hub-connected network segments.

**Characteristics:**
- Has only **two ports**
- Learns which hosts are on which side
- Contains traffic to necessary segments only

**How it works:**
- Monitors traffic patterns
- Builds a table of which devices are on each segment
- Only forwards traffic when destination is on the other segment

**Layer:** Data Link Layer (Layer 2)

---

## 3. Modern Network Components

### Switch

**Definition:** A combination of a hub and a bridge.

**How it works:**
- Facilitates communication **within** a network
- Sends data **only** to the specific port where the destination host is connected
- Uses MAC addresses to make forwarding decisions

**Advantages over hubs:**
- Reduces unnecessary traffic
- Improves network performance
- Better security (devices only see traffic meant for them)

**Layer:** Data Link Layer (Layer 2)

**See also:** [Switch Functioning](switch-functioning.md) for detailed explanation

### Router

**Definition:** A device that facilitates communication **between** networks.

**How it works:**
- Uses a **routing table** to determine the best path for data
- Makes decisions based on IP addresses
- Serves as a boundary for security and traffic control
- Can connect different types of networks (Ethernet, Wi-Fi, etc.)

**Key functions:**
- Packet forwarding
- Path determination
- Network address translation (NAT)
- Firewall capabilities

**Layer:** Network Layer (Layer 3)

**See also:** [Router Functioning](router-functioning.md) for detailed explanation

---

## 4. Hierarchical Concepts

### Gateway

**Definition:** The specific IP address of a router interface that serves as a host's exit point from its local network.

**Also known as:** Default gateway

**Purpose:**
- Provides the path to external networks
- Required configuration for internet access
- Acts as the "door" to other networks

### Routing vs. Switching

| Aspect | Switching | Routing |
|--------|-----------|---------|
| **Scope** | Moving data **within** a network | Moving data **between** different networks |
| **Addressing** | Uses MAC addresses | Uses IP addresses |
| **Layer** | Layer 2 (Data Link) | Layer 3 (Network) |
| **Speed** | Faster (hardware-based) | Slower (more processing) |
| **Devices** | Switches | Routers |

---

## Summary

These devices collectively enable the global hierarchy of the Internet, which is essentially a collection of interconnected routers working together to deliver data from source to destination.

**Network Evolution:**
```
Repeater → Hub → Bridge → Switch → Router
(Simple signal extension → Intelligent traffic management)
```

---

## Related Topics
- [Switch Functioning](switch-functioning.md) - How switches learn and forward
- [Router Functioning](router-functioning.md) - How routers route between networks
- [OSI Model](osi-model.md) - Understanding device layers