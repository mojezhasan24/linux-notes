# Hosts, Networks, and IP Addresses

---

## Table of Contents
- [Hosts](#1-hosts)
- [IP Addresses](#2-ip-addresses)
- [Networks](#3-networks)

---

## 1. Hosts

**Definition:** Any device that sends or receives traffic over a network.

**Examples:**
- Computers (desktops, laptops)
- Mobile devices (phones, tablets)
- Servers
- IoT devices (smart thermostats, cameras, etc.)

**Roles:**
- **Client:** Initiates requests for data or services
- **Server:** Responds to requests and provides data or services
- **Note:** These roles are relative to the specific communication — a device can be both client and server simultaneously

**Key Concept:**
A server is simply a computer with specific software installed to handle incoming requests. There's no fundamental hardware difference between a client and server.

---

## 2. IP Addresses

**Definition:** The unique identity of a host on a network, required for all internet communication.

**Structure (IPv4):**
- Composed of **32 bits**
- Divided into four **octets** (8 bits each)
- Represented in dotted-decimal notation (e.g., `192.168.1.1`)
- Each octet ranges from 0-255

**Example:** `192.168.1.100`
- Octet 1: 192
- Octet 2: 168
- Octet 3: 1
- Octet 4: 100

**Hierarchy:**
- IP addresses are assigned hierarchically (by location, organization, or department)
- **Subnetting:** Process of dividing a network into smaller subnetworks
  - Allows for efficient IP address allocation
  - Enables easier identification of where a host is located
  - Improves network management and security

---

## 3. Networks

**Definition:** A logical grouping of hosts that require similar connectivity and can communicate directly with each other.

**Function:**
- Automates the sharing of data between devices
- Enables resource sharing (files, printers, internet access)
- Provides centralized management and security

**Structure:**
- Networks can contain other networks (**subnetworks** or **subnets**)
- The internet is a vast collection of interconnected networks
- Each network has a unique network address

**Types:**
- **LAN (Local Area Network):** Small geographic area (home, office, building)
- **WAN (Wide Area Network):** Large geographic area (city, country, global)
- **MAN (Metropolitan Area Network):** City-wide coverage

---

## Related Topics
- [OSI Model](osi-model.md) - Understanding network layers
- [Networking Devices](networking-devices.md) - How devices connect networks
- [Host Functioning](host-functioning.md) - How hosts communicate
