# 1. Signal Management

   ## Repeater : A device used to regenerate signals that decay over long distances. It ensures connectivity between hosts far apart.

# 2. Early Scaling Devices

   ## Hub : A multi-port repeater. It duplicates incoming data and sends it out to all remaining ports.
        Problem: All connected devices receive every packet, leading to inefficient communication.
   ## Bridge : Sits between hub-connected segments. It only has two ports and learns which hosts are on which side, allowing it to contain traffic to necessary segments.

# 3. Modern Network Components

   ## Switch : A combination of a hub and a bridge. It facilitates communication within a network by sending data only to the specific ports where the destination host is connected.
   ## Router : Used to facilitate communication between networks. It uses a routing table to determine the best path for data, serving as a boundary for security and traffic control.

# 4. Hierarchical Concepts

   ## Gateway : The specific IP address of a router interface that serves as a host's exit point from its local network.
   ## Routing vs. Switching :
        Switching: Moving data within a network.
        Routing: Moving data between different networks.

- These devices collectively enable the global hierarchy of the Internet, which is essentially a collection of interconnected routers