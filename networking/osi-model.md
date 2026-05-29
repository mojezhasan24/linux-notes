The OSI (Open Systems Interconnection) model is a universal, 7-layer conceptual framework created by the ISO. It standardizes how different computer systems and hardware interact and communicate over a network, breaking down data transmission into distinct, abstract stages to simplify design and troubleshooting.
![alt text](image.png)

Layer 1: The Physical Layer (2:00 - 3:42)

    Goal: Transporting bits (ones and zeros) between devices.
    Examples: Physical media like cables, wireless technology (Wi-Fi), and devices like repeaters and hubs are considered Layer 1 technologies as they extend or carry signals.

Layer 2: The Data Link Layer (3:43 - 7:27)

    Goal: "Hop-to-hop" delivery; moving data between specific network interfaces.
    Addressing: Uses MAC addresses, which are 48-bit unique identifiers for network interface cards (NICs).
    Devices: Switches and NICs are considered Layer 2 technologies because they assist in the delivery of data across a single hop.

Layer 3: The Network Layer (7:38 - 12:08)

    Goal: "End-to-end" delivery; ensuring data reaches the target destination from the source.
    Addressing: Uses IP addresses (32-bit addresses) to identify the specific end-host.
    Devices: Routers function at Layer 3 to facilitate this end-to-end communication.

Key Concept: How Layers Work Together (8:48 - 11:24)

    The video explains that data travels by wrapping the payload in Layer 3 information (IP addresses) for the full journey, while constantly adding and removing Layer 2 headers (MAC addresses) at each router hop to facilitate the physical transit.
    The Address Resolution Protocol (ARP) is mentioned as the essential protocol that links Layer 3 IP addresses to Layer 2 MAC addresses (11:39 - 12:07).

Layer 4: The Transport Layer (0:58 - 3:30)

    Primary Goal: Service-to-service delivery. While Layer 3 (Network) handles end-to-end delivery (host-to-host) and Layer 2 (Data Link) handles hop-to-hop delivery, Layer 4 ensures data reaches the specific program (e.g., web browser, game, chat app) running on the host.
    Mechanism: Uses ports to distinguish between multiple data streams running simultaneously on a computer.
    Protocols:
        TCP: Prioritizes reliability.
        UDP: Prioritizes efficiency.
    Port Addressing:
        There are 65,535 possible ports for both TCP and UDP.
        Server-side: Servers listen on predefined, "well-known" port numbers (e.g., HTTPS on 443, HTTP on 80, IRC on 6667).
        Client-side: The client dynamically selects a random source port for each connection. This allows the client to handle multiple tabs or active programs without data streams overlapping.

Layers 5, 6, and 7: Session, Presentation, and Application (7:18 - 8:27)

    Current State: The formal distinctions between these layers have become somewhat vague in modern practice.
    Universal Application Layer: Because applications are free to implement these layers as they see fit, they are often grouped together as a single Application layer, mirroring the simplified structure of the TCP/IP model.
