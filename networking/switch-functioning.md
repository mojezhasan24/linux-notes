Switching Defined: Switching is the process of moving data within networks. Any device performing this function follows the same set of rules (0:14).

Layer 2 Operations: Switches are Layer 2 devices; they make forwarding decisions based solely on the Layer 2 header (MAC addresses), ignoring Layer 3 (IP) headers (1:39).

MAC Address Table: Switches maintain a table that maps specific switch ports to the MAC addresses of connected devices. This table starts empty and is populated dynamically as data flows through the switch (2:43).

The Three Fundamental Switch Actions (The Rules of Switching):

    Learning (4:09): The switch inspects the source MAC address of an incoming frame and records which port that device is connected to in its MAC address table.
    Flooding (5:05): If the switch does not know the destination MAC address, it duplicates the frame and sends it out of all ports except the one on which it was received.
    Forwarding (7:09): Once the destination MAC address is in the table, the switch sends the frame directly to the specific port associated with that address.

Traffic Flow: Switches operate differently depending on whether traffic is moving through the switch (using the actions above) or to the switch (where the switch acts like any other host and requires its own IP and MAC address for management) (8:55).

1. Unicast Flooding vs. Broadcasts (0:48 - 3:25)

    Unicast: One-to-one communication. Switches only "flood" these if the destination MAC address is unknown (not in the MAC address table). If the location is known, the switch performs the Forwarding action.
    Broadcast: A frame with a destination MAC of all 'F's (e.g., an ARP request). Switches always flood broadcast frames to all other ports.
    Key distinction: "Flood" is an action a switch takes, whereas "Broadcast" is a type of frame.

2. VLANs (Virtual Local Area Networks) (3:31 - 5:07)

    VLANs allow you to logically divide a single physical switch into isolated groups (e.g., VLAN 20 and VLAN 30).
    The switch maintains separate MAC address tables for each VLAN, ensuring that the fundamental actions (Learning, Flooding, Forwarding) occur independently within each group.

3. Multiple Switches (5:08 - 9:48)

    When multiple switches are connected, each switch maintains its own independent MAC address table.
    They do not share information; they each perform the three fundamental actions as frames traverse the path.
    It is normal for a single switch port to have multiple MAC address mappings (e.g., when a port is connected to another switch).

Key Takeaway: Any device that claims to perform "switching" will follow these same rules. In the next lesson, the series will cover Routers and how they facilitate communication between networks.