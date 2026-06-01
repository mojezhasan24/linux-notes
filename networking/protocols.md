# Networking Protocols

---

## Table of Contents
- [What is a Protocol?](#what-is-a-protocol)
- [Common Networking Protocols](#common-networking-protocols)
- [Internet Connectivity Requirements](#internet-connectivity-requirements)
- [DHCP (Dynamic Host Configuration Protocol)](#dhcp-dynamic-host-configuration-protocol)

---

## What is a Protocol?

**Definition:** A protocol is a set of rules and messages that form an internet standard.

**Purpose:**
- Enables devices from **different vendors** (HP, Apple, Dell, Cisco, etc.) to communicate
- Establishes a **common language** for network communication
- Ensures **interoperability** across diverse hardware and software

**Standards:**
- Defined by organizations like **IETF** (Internet Engineering Task Force)
- Documented in **RFCs** (Request for Comments)
- Publicly available for implementation by anyone

**Example:**
- **ARP (Address Resolution Protocol)** uses rules defined in **RFC 826**
- Purpose: Resolves IP addresses to MAC addresses
- See [Host Functioning](host-functioning.md) for detailed ARP process

---

## Common Networking Protocols

### FTP (File Transfer Protocol)

**Purpose:** Sending and receiving files between systems

**Characteristics:**
- Uses **TCP** for reliable delivery
- Default ports: **20** (data) and **21** (control)
- Supports authentication (username/password)
- Can transfer text and binary files

**Use Cases:**
- Website file uploads
- Backup systems
- Large file transfers

**Security Note:** FTP transmits credentials in plaintext — use **SFTP** or **FTPS** for encrypted transfers

---

### SMTP (Simple Mail Transfer Protocol)

**Purpose:** Used by email servers to exchange and relay mail

**Characteristics:**
- Uses **TCP** for reliable delivery
- Default port: **25** (unencrypted), **587** (TLS), **465** (SSL)
- Text-based protocol
- Client-server model

**How it works:**
1. Client connects to SMTP server
2. Sends `HELO/EHLO` greeting
3. Specifies sender (`MAIL FROM`)
4. Specifies recipient (`RCPT TO`)
5. Sends message data (`DATA`)

**Related Protocols:**
- **POP3** (Port 110): Retrieve email from server
- **IMAP** (Port 143): Access email on server (more features than POP3)

---

### HTTP (Hypertext Transfer Protocol)

**Purpose:** Exchange web pages and web content

**Characteristics:**
- Uses **TCP** for reliable delivery
- Default port: **80**
- **Stateless** protocol (each request is independent)
- Based on **request-response** model

**How it works:**
1. **Client** (browser) sends HTTP request
2. **Server** processes request and sends response
3. Connection closes (unless using keep-alive)

**Common Methods:**
- `GET`: Retrieve data
- `POST`: Send data to server
- `PUT`: Update existing resource
- `DELETE`: Remove resource
- `HEAD`: Get headers only

**Format:** Data is typically in **HTML** (Hypertext Markup Language)

---

### HTTPS (HTTP Secure)

**Purpose:** HTTP conversation secured with encryption

**Characteristics:**
- Uses **TCP** for reliable delivery
- Default port: **443**
- **HTTP** wrapped in **SSL/TLS** encryption tunnel
- Provides **confidentiality**, **integrity**, and **authentication**

**Security Features:**
- **Encryption:** Data cannot be read by third parties
- **Authentication:** Server identity verified via certificates
- **Integrity:** Data cannot be tampered with without detection

**Modern Standard:**
- HTTPS is now the default for most websites
- Browsers mark HTTP sites as "Not Secure"
- Uses TLS 1.2 or 1.3 (SSL is deprecated)

---

### DNS (Domain Name System)

**Purpose:** Automatically translates human-readable domain names into machine-readable IP addresses

**Characteristics:**
- Uses **UDP** (port 53) for queries, **TCP** (port 53) for zone transfers
- Hierarchical, distributed database
- Essential for internet functionality

**How it works:**
```
User types: www.example.com
         ↓
DNS resolver queries DNS servers
         ↓
Returns IP: 93.184.216.34
         ↓
Browser connects to IP address
```

**Why it's essential:**
- Humans remember names better than numbers
- IP addresses can change without affecting users
- Enables load balancing and redundancy

**Common DNS Records:**
- **A:** IPv4 address (e.g., `192.168.1.1`)
- **AAAA:** IPv6 address
- **MX:** Mail server
- **CNAME:** Alias for another domain
- **NS:** Name server

---

## Internet Connectivity Requirements

To reach the Internet, every host (device) must be configured with **four specific items**:

### 1. IP Address

**Definition:** The host's unique identity on the network

**Example:** `192.168.1.10`

**Purpose:**
- Identifies the device on the network
- Enables communication with other devices
- Must be unique within its subnet

**See:** [Hosts, Networks & IP](hosts-network-and-IP.md) for detailed explanation

---

### 2. Subnet Mask

**Definition:** Defines the size of the network

**Example:** `255.255.255.0` (or `/24`)

**Purpose:**
- Separates network portion from host portion
- Determines which IPs are on the local network
- Helps decide if traffic goes to gateway or directly to destination

**See:** [Subnetting](subnetting.md) for comprehensive coverage

---

### 3. Default Gateway

**Definition:** The IP address of the router used to reach foreign networks

**Example:** `192.168.1.1`

**Purpose:**
- Exit point for traffic destined outside local network
- Required for internet access
- Acts as a "door" to other networks

**How it works:**
- If destination IP is on foreign network → send to gateway
- Gateway (router) forwards packet toward destination

**See:** [Host Functioning](host-functioning.md) for gateway communication details

---

### 4. DNS Server IP

**Definition:** IP address of the DNS server used to resolve domain names to IP addresses

**Example:** `8.8.8.8` (Google DNS), `1.1.1.1` (Cloudflare DNS)

**Purpose:**
- Enables use of human-readable names (google.com)
- Without DNS, you'd need to memorize IP addresses
- Required for web browsing and most internet services

**How it works:**
- User types: `google.com`
- Host queries DNS server: "What's the IP for google.com?"
- DNS server responds: `142.250.80.46`
- Host connects to that IP address

---

### Configuration Example

```
Host Configuration:
- IP Address: 192.168.1.100
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1
- DNS Server: 8.8.8.8
```

---

## DHCP (Dynamic Host Configuration Protocol)

### Purpose

**DHCP** automates the network configuration process for hosts.

### How it Works

When a device connects to a network:

1. **DHCP Discover** (Client)
   - Broadcast message: "Is there a DHCP server?"
   - Sent to `255.255.255.255` (broadcast address)

2. **DHCP Offer** (Server)
   - Server responds with available configuration:
     - IP address
     - Subnet mask
     - Default gateway
     - DNS server IP
     - Lease time (how long you can use the IP)

3. **DHCP Request** (Client)
   - Client accepts the offer
   - Requests the specific configuration

4. **DHCP Acknowledgment** (Server)
   - Server confirms the allocation
   - Configuration is now active

### Characteristics

- **Uses UDP:** Port 67 (server), Port 68 (client)
- **Lease-based:** IP addresses are temporary (typically 24 hours)
- **Automatic:** No manual configuration required
- **Scalable:** Manages hundreds or thousands of devices

### Benefits

✅ **Automatic configuration** — no manual IP entry
✅ **Prevents conflicts** — server tracks allocated addresses
✅ **Centralized management** — one place to configure network settings
✅ **Efficient address usage** — reuses IPs when devices disconnect
✅ **Easy to scale** — add devices without configuration

### DHCP vs. Static Configuration

| Aspect | DHCP (Dynamic) | Static |
|--------|---------------|--------|
| **Setup** | Automatic | Manual |
| **Time** | Instant | Time-consuming |
| **Conflicts** | Server prevents | Administrator must avoid |
| **Flexibility** | Addresses reused | Fixed allocation |
| **Use case** | Most devices | Servers, routers, printers |

**Best Practice:**
- Use **DHCP** for: workstations, laptops, phones, tablets
- Use **static** for: servers, routers, network printers, critical infrastructure

---

## Summary Table

| Protocol | Port | Transport | Purpose |
|----------|------|-----------|---------|
| FTP | 20, 21 | TCP | File transfer |
| SMTP | 25, 587, 465 | TCP | Email transmission |
| HTTP | 80 | TCP | Web browsing |
| HTTPS | 443 | TCP | Secure web browsing |
| DNS | 53 | UDP/TCP | Domain name resolution |
| DHCP | 67, 68 | UDP | Automatic configuration |

---

## Related Topics

- [Hosts, Networks & IP](hosts-network-and-IP.md) - Basic networking concepts
- [Subnetting](subnetting.md) - IP address organization
- [Host Functioning](host-functioning.md) - How hosts communicate
- [OSI Model](osi-model.md) - Understanding network layers
