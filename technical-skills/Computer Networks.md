# OSI Model
1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

## Physical Layer
The physical layer provides the electrical, mechanical, and procedural interface to the transmission medium. It specifies low-level parameters such as the shapes and properties of electrical connectors, the frequencies to transmit on, line coding, voltage levels, and other similar characteristics.

For example, the IEEE 802.3 (Ethernet) standards specify the medium (copper or fiber), the connectors (RJ45 for twisted pair; LC/SC for fiber), data rates (10 Mbps, 100 Mbps, 1 Gbps, etc.), signaling and line coding, impedance and maximum cable length, and voltage levels.

At its core, the physical layer carries a stream of bits (more precisely: symbols encoded as electrical/optical/radio signals) and does not interpret the meaning of the information those bits represent.

### Ethernet Hub
All data coming into an Ethernet hub travels to all stations connected to the hub.

### Hosts and Nodes
A host is a device connected to a network that is in some way involved in applications. For example, computers, smartphones, and tablets are nodes. Routers or switches that just forward packets don’t count as hosts; they’re just nodes, unless they offer applications (such as configuration via the network).

### Interfaces
Examples: Ethernet interface, USB interface, WiFi interface, etc.

### Links
Example: Ethernet cable connecting two interfaces.

## Data Link Layer
The sending host packs data into frames. Data link layer devices, such as network switches, receive these frames, interpret the headers (such as MAC addresses), and forward the frames to the appropriate port.

The sending host uses the Address Resolution Protocol (ARP) to discover the MAC address associated with an IP address. When the sending host wants to send data to another device on the local network (including the default gateway), it checks its ARP cache. If the MAC address is not known, it sends an ARP request, and the device with the matching IP address replies with its MAC address.

Switches typically do not modify the frames. They read the headers—specifically the destination MAC address—and forward the frames to the correct physical interface (port) based on their MAC address table. The frame contents remain unchanged during this process.

### Address Resolution Protocol (ARP)
ARP is used to map a known IP address to its corresponding MAC address within the local network. When a device wants to communicate with another device on the same local segment, it uses ARP to discover the MAC address associated with the target IP address.

The sending host does not deliver the ARP request directly to the target IP address. Instead, it broadcasts the ARP request to all devices on the local network segment. The ARP request asks: "Who has IP address X.X.X.X? Tell me your MAC address." Every device on the local network receives this broadcast. Only the device with the matching IP address responds with its MAC address.

### Local Network
A local network (or local segment, broadcast domain) is a group of devices that can communicate directly at the data link layer (Layer 2), typically using MAC addresses and broadcasts like ARP. Devices in the same local network can send frames to each other without passing through a router.

- Who delivers ARP requests in the local network?
  Switches (or hubs) forward ARP broadcast frames to all devices on the same segment.

  ARP requests do not require a server to take effect. ARP is a protocol that works automatically between devices on the same local network segment. The sending host broadcasts the ARP request, and any device with the matching IP address replies directly. Switches or hubs simply forward the broadcast; no server is involved.

## Network Layer
When a frame arrives at a network layer device (like a router), the device decapsulates the frame to access and examine the network layer packet (such as an IP packet) inside. The router then processes the packet and, if forwarding is needed, encapsulates it in a new frame for the next hop.

It should be noted that it is the interface of a host (such as an Ethernet or WiFi interface) that gets assigned an IP address, not the host itself.

### IP Version 4 (IPv4)
32 bits (4 octets) long, commonly written in dotted decimal format; e.g., 130.216.72.11 = 10000010 11011000 01001000 00001011.

### IP Version 6 (IPv6)
128 bits (16 octets) long, commonly written in hexadecimal format with `:` as a hextet separator; e.g., a23d:09fb:0000:0000:0000:48ca:0000:0000. Zeros can be omitted for abbreviation as long as it's not ambiguous: a23d:9fb::48ca:0000:0000

### Netid and Hostid
The 32 bits of the IPv4 address consist of two parts: **netid** and **hostid**. The lengths of the netid and hostid can vary.

For example, you can specify that the first 20 bits are the netid: **10000010 11011000 0100**1000 00001011. The netid is an identifier for the network that the IP address belongs to.
- All hosts in the network with that netid have IP addresses with the same netid.
- When routers communicate about how to reach certain networks, they exchange netids only.

The remaining 12 bits are the hostid: 10000010 11011000 0100**1000 00001011**. The hostid is the address of the host within the network identified by the netid.
- Different hosts in the network with the same netid all have different hostids.
- A gateway router has at least two physical interfaces, with each interface located in a different network and assigned a hostid in its corresponding network.
- When you connect two previously isolated networks, all overlapping IP addresses must be resolved. This is because each device on interconnected networks must have a unique IP address within the combined address space.

### Netmask and Classless Inter-Domain Routing (CIDR) Notation
Both methods are used to determine the length of the netid, which defines the size of the network.

#### Netmask
A netmask is a 32-bit number used in IPv4 networking to specify which portion of an IP address is the network part (netid) and which part is the host part (hostid). It is usually written in dotted decimal notation, like 255.255.255.0.

For example:
- IP address: 192.168.1.10
- Netmask: 255.255.255.0

The netmask tells the network which bits of the IP address refer to the network and which bits refer to the host. In this example, the first 24 bits (255.255.255) are the network, and the last 8 bits (0) are for hosts.

#### CIDR
CIDR notation is a way to specify IP addresses and their associated network prefix length. It looks like this: 192.168.1.10/24.
- The /24 means the first 24 bits are the network part (netid), and the remaining bits are for hosts.
- CIDR allows more flexible division of IP address space than traditional class-based addressing.
- For example, 10.0.0.0/8 is a large network, while 192.168.1.0/28 is a small network.

CIDR notation replaces the need for a separate netmask by including the prefix length directly after the IP address.

### IPv4 Broadcast Address
A broadcast address is used to send a message to all devices on a network or subnet at once. In IPv4, sending data to the broadcast address ensures that every device in the specified network segment receives the message, rather than just a single device. This is useful for protocols like ARP or for network announcements.

- Global broadcast address: 255.255.255.255  
  The address 255.255.255.255 is the IPv4 limited broadcast address, which is only valid within a local network segment. Routers do not forward packets sent to 255.255.255.255 beyond the local network. This prevents broadcast messages from being sent to every device on the internet. Only devices on the same local subnet will receive the broadcast; routers drop such packets by design.
- Subnet broadcast addresses: all bits of hostid = 1  
  The subnet broadcast address allows you to broadcast messages only to devices within a specific subnet, as determined by the netid and netmask (or CIDR prefix). The broadcast address for a subnet is formed by setting all bits of the hostid portion to 1. Only devices within that subnet will receive the broadcast message, not devices outside the subnet.

### How Does a Host Interface Get Its IP Address?
A host interface gets assigned an IP address by an administrator, who obtains a range of available IP addresses from an ISP. The ISP gets its assignable IP addresses from a Regional Internet Registry (RIR), and the RIR gets IP addresses from ICANN/IANA.

The administrator can assign a static IP address manually to the host's interface (called explicit assignment), or use dynamic assignment via Dynamic Host Configuration Protocol (DHCP).

#### DHCP
To request an IP address, the client's interface broadcasts a DHCPDISCOVER message, which is encapsulated in a UDP datagram with source port 68 and destination port 67. The UDP datagram also contains the client's MAC address and a parameter list specifying what is requested in addition to an IP address, such as a subnet mask, gateway address, DNS server, domain name, etc. The UDP datagram is encapsulated in an IP packet with source address 0.0.0.0 and destination address 255.255.255.255. The IP packet is then encapsulated in an Ethernet frame (or an 802.11 frame for WiFi) with the client's MAC address as the source and FF:FF:FF:FF:FF:FF as the destination (broadcast).

Upon receiving the DHCPDISCOVER request, the DHCP server responds with a DHCPOFFER message. This response is typically sent as a broadcast (destination MAC FF:FF:FF:FF:FF:FF) but includes the client's MAC address so the client can identify its offer. The DHCPOFFER contains the proposed IP address and other configuration parameters.

Finally, the client broadcasts a DHCPREQUEST message to accept the address offered. This message informs all DHCP servers which offer it is accepting, and the chosen server responds with a DHCPACK message to confirm the assignment.

Note the DHCPDISCOVER message does not specify whether the client wants a public or private IP address. The message simply requests an IP address and related configuration. It is up to the DHCP server to decide which address to offer, based on its configuration and available address pools.
### Private IP Addresses
The following IP address ranges are private:
- 10.0.0.0/8
- 100.64.0.0/10 (for carrier-grade NATs)
- 172.16.0.0/12
- 192.168.0.0/16

Private IP addresses don't require assignment by an ISP and are not forwarded by routers on the internet. They are frequently used on the local network side of NATs (Network Address Translators).

### Subnet Partitioning
Subnet partitioning is the process of dividing a larger network (IP address block) into smaller, more manageable subnetworks called subnets. Each subnet has its own network identifier (netid) and a range of host addresses (hostid).

Subnet partitioning is typically done by adjusting the subnet mask (or CIDR prefix length) to allocate fewer host bits and more network bits. This allows organizations to organize their network into logical segments, improve security, and optimize traffic flow.

For example, partitioning 192.168.1.0/24 into four subnets would result in:
- 192.168.1.0/26
- 192.168.1.64/26
- 192.168.1.128/26
- 192.168.1.192/26

Each subnet can be used for a different department, floor, or purpose within the organization.