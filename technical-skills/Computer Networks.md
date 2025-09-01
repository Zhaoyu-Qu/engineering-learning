# OSI Model
1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

## Physical layer
The physical layer provides the electrical, mechanical, and procedural interface to the transmission medium. It specifies low-level parameters such as the shapes and properties of electrical connectors, the frequencies to transmit on, line coding, voltage levels, and other similar characteristics.

For example, the IEEE 802.3 (Ethernet) standards specify the medium (copper or fiber), the connectors (RJ45 for twisted pair; LC/SC for fiber), data rates (10 Mbps, 100 Mbps, 1 Gbps, etc.), signaling and line coding, impedance and maximum cable length, and voltage levels.

At its core the physical layer carries a stream of bits (more precisely: symbols encoded as electrical/optical/radio signals) and does not interpret the meaning of the information those bits represent.

### Ethernet hub
All data coming into an Ethernet hub travels to all stations connected to the hub.

### Hosts and Nodes
A host is a device connected to a network that is in some way involved in
applications. For example, computers, smartphones and tablets are nodes. Routers or switches that just forward packets don’t count as hosts, they’re just nodes, unless they offer applications (such as configuration via the network)

#### Ethernet hub
All data coming into an Ethernet hub travels to all stations connected to the hub.

### Interfaces
Examples: Ethernet interface, USB interface, WiFi interface, etc.

### Links
Example: Ethernet cabel connecting two interfaces.

## Data Link Layer
The sending host packs data into frames. Data link layer devices, such as network switches, receive these frames, interpret the headers (such as MAC addresses), and forward the frames to the appropriate port.

Switches typically do not modify the frames. They read the headers—specifically the destination MAC address—and forward the frames to the correct physical interface (port) based on their MAC address table. The frame contents remain unchanged during this process.