A `network` is a collection of interconnected devices that can communicate - sending and receiving data, and also sharing resources with each other. These individual endpoint devices, often called `nodes`, include computers, smartphones, printers, and servers. However, `nodes` alone do not comprise the entire network.

|**Concepts**|**Description**|
|---|---|
|`Nodes`|Individual devices connected to a network.|
|`Links`|Communication pathways that connect nodes (wired or wireless).|
|`Data Sharing`|The primary purpose of a network is to enable data exchange.|
# Importance

|**Function**|**Description**|
|---|---|
|`Resource Sharing`|Multiple devices can share hardware (like printers) and software resources.|
|`Communication`|Instant messaging, emails, and video calls rely on networks.|
|`Data Access`|Access files and databases from any connected device.|
|`Collaboration`|Work together in real-time, even when miles apart.|
# Types

## LAN (Local Area Network)

LANs (Local Area Network) and WLANs (Wireless Local Area Network) will typically assign IP Addresses designated for local use (RFC 1918, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16). In some cases, like some colleges or hotels, you may be assigned a routable (internet) IP Address from joining their LAN, but that is much less common. There's nothing different between a LAN or WLAN, other than WLAN's introduce the ability to transmit data without cables. It is mainly a security designation.

|**Characteristic**|**Description**|
|---|---|
|`Geographical Scope`|Covers a small area.|
|`Ownership`|Typically owned and managed by a single person or organization.|
|`Speed`|High data transfer rates.|
|`Media`|Uses wired (Ethernet cables) or wireless (Wi-Fi) connections.|
![[Pasted image 20260128155746.png]]

## WAN (Wide Area Network)

The WAN (Wide Area Network) is commonly referred to as `The Internet`. When dealing with networking equipment, we'll often have a WAN Address and LAN Address. The WAN one is the address that is generally accessed by the Internet. That being said, it is not exclusive to the Internet; a WAN is just a large number of LANs joined together. `Many large companies or government agencies will have an "Internal WAN" (also called Intranet, Airgap Network, etc.)`. Generally speaking, the primary way we identify if the network is a WAN is to use a WAN Specific routing protocol such as BGP and if the IP Schema in use is not within RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

|**Characteristic**|**Description**|
|---|---|
|`Geographical Scope`|Covers cities, countries, or continents.|
|`Ownership`|Often a collective or distributed ownership (e.g., internet service providers).|
|`Speed`|Slower data transfer rates compared to LANs due to long-distance data travel.|
|`Media`|Utilizes fiber optics, satellite links, and leased telecommunication lines.|
![[Pasted image 20260128155856.png]]

| Network Type                          | Definition                       |
| ------------------------------------- | -------------------------------- |
| Global Area Network (GAN)             | Global network (the Internet)    |
| Metropolitan Area Network (MAN)       | Regional network (multiple LANs) |
| Wireless Personal Area Network (WPAN) | Personal network (Bluetooth)     |

## GAN

A worldwide network such as the `Internet` is known as a `Global Area Network` (`GAN`). However, the Internet is not the only computer network of this kind. Internationally active companies also maintain isolated networks that span several `WAN`s and connect company computers worldwide. `GAN`s use the glass fibers infrastructure of wide-area networks and interconnect them by international undersea cables or satellite transmission.

## MAN

`Metropolitan Area Network` (`MAN`) is a broadband telecommunications network that connects several `LAN`s in geographical proximity.
Internationally operating network operators provide the infrastructure for `MAN`s. Cities wired as `Metropolitan Area Networks` can be integrated supra-regionally in `Wide Area Networks` (`WAN`) and internationally in `Global Area Networks` (`GAN`).

## PAN/WPAN

Modern end devices such as smartphones, tablets, laptops, or desktop computers can be connected ad hoc to form a network to enable data exchange. This can be done by cable in the form of a `Personal Area Network` (`PAN`).

The wireless variant `Wireless Personal Area Network` (`WPAN`) is based on Bluetooth or Wireless USB technologies. A `wireless personal area network` that is established via Bluetooth is called `Piconet`. `PAN`s and `WPAN`s usually extend only a few meters and are therefore not suitable for connecting devices in separate rooms or even buildings.

## VPN

There are three main types `Virtual Private Networks` (`VPN`), but all three have the same goal of making the user feel as if they were plugged into a different network.

### Site-to-Site VPN

Both the client and server are Network Devices, typically either `Routers` or `Firewalls`, and share entire network ranges. This is most commonly used to join company networks together over the Internet, allowing multiple locations to communicate over the Internet as if they were local.

## Remote Access VPN

This involves the client's computer creating a virtual interface that behaves as if it is on a client's network. When analyzing these VPNs, an important piece to consider is the routing table that is created when joining the VPN. If the VPN only creates routes for specific networks (ex: 10.10.10.0/24), this is called a `Split-Tunnel VPN`, meaning the Internet connection is not going out of the VPN.

### SSL VPN

This is essentially a VPN that is done within our web browser and is becoming increasingly common as web browsers are becoming capable of doing anything.
### Comparison

|Aspect|LAN|WAN|
|---|---|---|
|`Size`|Small, localized area|Large, broad area|
|`Ownership`|Single person or organization|Multiple organizations/service providers|
|`Speed`|High|Lower compared to LAN|
|`Maintenance`|Easier and less expensive|Complex and costly|
|`Example`|Home or office network|The Internet|
# OSI Model

The `Open Systems Interconnection (OSI) model` is a conceptual framework that standardizes the functions of a telecommunication or computing system into seven abstract layers.

![[Pasted image 20260711123233.png]]

## Physical Layer (Layer 1)

The `Physical Layer` is the first and lowest layer of the OSI model. It is responsible for transmitting raw bitstreams over a physical medium. This layer deals with the physical connection between devices, including the hardware components like Ethernet cables, hubs, and repeaters.

## Data Link Layer (Layer 2)

The `Data Link Layer` provides node-to-node data transfer - a direct link between two physically connected nodes. It ensures that data frames are transmitted with proper synchronization, error detection, and correction. Devices such as switches and bridges operate at this layer, using `MAC (Media Access Control)` addresses to identify network devices.

## Network Layer (Layer 3)

The `Network Layer` handles packet forwarding, including the routing of packets through different routers to reach the destination network. It is responsible for logical addressing and path determination, ensuring that data reaches the correct destination across multiple networks. Routers operate at this layer.

## Transport Layer (Layer 4)

The `Transport Layer` provides end-to-end communication services for applications. It is responsible for the reliable (or unreliable) delivery of data, segmentation, reassembly of messages, flow control, and error checking. Protocols like `TCP (Transmission Control Protocol)` and `UDP (User Datagram Protocol)` function at this layer.

## Session Layer (Layer 5)

The `Session Layer` manages sessions between applications. It establishes, maintains, and terminates connections, allowing devices to hold ongoing communications known as sessions. This layer is essential for session checkpointing and recovery, ensuring that data transfer can resume seamlessly after interruptions. Protocols and `APIs (Application Programming Interfaces)` operating at this layer coordinate communication between systems and applications.

## Presentation Layer (Layer 6)

The `Presentation Layer` acts as a translator between the application layer and the network format. It handles data representation, ensuring that information sent by the application layer of one system is readable by the application layer of another. This includes data encryption and decryption, data compression, and converting data formats.

## Application Layer (Layer 7)

The `Application Layer` is the topmost layer of the OSI model and provides network services directly to end-user applications. It enables resource sharing, remote file access, and other network services. Common protocols operating at this layer include `HTTP (Hypertext Transfer Protocol)` for web browsing, `FTP (File Transfer Protocol)` for file transfers, `SMTP (Simple Mail Transfer Protocol)` for email transmission, and `DNS (Domain Name System)` for resolving domain names to IP addresses. This layer serves as the interface between the network and the application software.

![[Pasted image 20260128161101.png]]

# TCP/IP Model

The `Transmission Control Protocol/Internet Protocol (TCP/IP) model` is a condensed version of the `OSI` model, tailored for practical implementation on the internet and other networks.

## Link Layer

This layer is responsible for handling the physical aspects of network hardware and media. It includes technologies such as Ethernet for wired connections and Wi-Fi for wireless connections. The Link Layer corresponds to the Physical and Data Link Layers of the OSI model, covering everything from the physical connection to data framing.

## Internet Layer

The `Internet Layer` manages the logical addressing of devices and the routing of packets across networks. Protocols like IP (Internet Protocol) and ICMP (Internet Control Message Protocol) operate at this layer, ensuring that data reaches its intended destination by determining logical paths for packet transmission. This layer corresponds to the Network Layer in the OSI model.

## Transport Layer

At the `Transport Layer`, the TCP/IP model provides end-to-end communication services that are essential for the functioning of the internet. This includes the use of TCP (Transmission Control Protocol) for reliable communication and UDP (User Datagram Protocol) for faster, connectionless services. This layer ensures that data packets are delivered in a sequential and error-free manner, corresponding to the Transport Layer of the OSI model.

## Application Layer

The `Application Layer` of the TCP/IP model contains protocols that offer specific data communication services to applications. Protocols such as HTTP (Hypertext Transfer Protocol), FTP (File Transfer Protocol), and SMTP (Simple Mail Transfer Protocol) enable functionalities like web browsing, file transfers, and email services. This layer corresponds to the top three layers of the OSI model (Session, Presentation, and Application), providing interfaces and protocols necessary for data exchange between systems.

![[Pasted image 20260128161252.png]]

The most Important tasks of `TCP/IP` are:

| **Task**               | **Protocol** | **Description**                                                                                                                                                                                                                                                                                                                        |
| ---------------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Logical Addressing`   | `IP`         | Due to many hosts in different networks, there is a need to structure the network topology and logical addressing. Within TCP/IP, IP takes over the logical addressing of networks and nodes. Data packets only reach the network where they are supposed to be. The methods to do so are `network classes`, `subnetting`, and `CIDR`. |
| `Routing`              | `IP`         | For each data packet, the next node is determined in each node on the way from the sender to the receiver. This way, a data packet is routed to its receiver, even if its location is unknown to the sender.                                                                                                                           |
| `Error & Control Flow` | `TCP`        | The sender and receiver are frequently in touch with each other via a virtual connection. Therefore control messages are sent continuously to check if the connection is still established.                                                                                                                                            |
| `Application Support`  | `TCP`        | TCP and UDP ports form a software abstraction to distinguish specific applications and their communication links.                                                                                                                                                                                                                      |
| `Name Resolution`      | `DNS`        | DNS provides name resolution through Fully Qualified Domain Names (FQDN) in IP addresses, enabling us to reach the desired host with the specified name on the internet.                                                                                                                                                               |

# Comparison

![[Pasted image 20260128161307.png]]

`TCP/IP` is a communication protocol that allows hosts to connect to the Internet. It refers to the `Transmission Control Protocol` used in and by applications on the Internet. In contrast to `OSI`, it allows a lightening of the rules that must be followed, provided that general guidelines are followed.

`OSI`, on the other hand, is a communication gateway between the network and end-users. The OSI model is usually referred to as the reference model because it is newer and more widely used. It is also known for its strict protocol and limitations.

# Packet Transfers

In a layered system, devices in a layer exchange data in a different format called a `protocol data unit` (`PDU`). For example, when we want to browse a website on the computer, the remote server software first passes the requested data to the application layer. It is processed layer by layer, each layer performing its assigned functions. The data is then transferred through the network's physical layer until the destination server or another device receives it. The data is routed through the layers again, with each layer performing its assigned operations until the receiving software uses the data.

![[Pasted image 20260711123539.png]]

During the transmission, each layer adds a `header` to the `PDU` from the upper layer, which controls and identifies the packet. This process is called `encapsulation`. The header and the data together form the PDU for the next layer. The process continues to the `Physical Layer` or `Network Layer`, where the data is transmitted to the receiver. The receiver reverses the process and unpacks the data on each layer with the header information. After that, the application finally uses the data. This process continues until all data has been sent and received.

![[Pasted image 20260711123627.png]]

# Network Layer

The `network layer` (`Layer 3`) of `OSI` controls the exchange of data packets, as these cannot be directly routed to the receiver and therefore have to be provided with routing nodes. The data packets are then transferred from node to node until they reach their target. To implement this, the `network layer` identifies the individual network nodes, sets up and clears connection channels, and takes care of routing and data flow control. When sending the packets, addresses are evaluated, and the data is routed through the network from node to node. There is usually no processing of the data in the layers above the `L3` in the nodes. Based on the addresses, the routing and the construction of routing tables are done.

In short, it is responsible for the following functions:

- `Logical Addressing`
- `Routing`
# Protocols

`Protocols` are standardized rules that determine the formatting and processing of data to facilitate communication between devices in a network.

## Common Protocols

|**Protocol**|**Description**|
|---|---|
|`HTTP (Hypertext Transfer Protocol)`|Primarily used for transferring web pages. It operates at the Application Layer, allowing browsers and servers to communicate in the delivery of web content.|
|`FTP (File Transfer Protocol)`|Facilitates the transfer of files between systems, also functioning at the Application Layer. It provides a way for users to upload or download files to and from servers.|
|`SMTP (Simple Mail Transfer Protocol)`|Handles the transmission of email. Operating at the Application Layer, it is responsible for sending messages from one server to another, ensuring they reach their intended recipients.|
|`TCP (Transmission Control Protocol)`|Ensures reliable data transmission through error checking and recovery, operating at the Transport Layer. It establishes a connection between sender and receiver to guarantee the delivery of data in the correct order.|
|`UDP (User Datagram Protocol)`|Allows for fast, connectionless communication, which operates without error recovery. This makes it ideal for applications that require speed over reliability, such as streaming services. UDP operates at the Transport Layer.|
|`IP (Internet Protocol)`|Crucial for routing packets across network boundaries, functioning at the Internet Layer. It handles the addressing and routing of packets to ensure they travel from the source to the destination across diverse networks.|
# Transmission

`Transmission` in networking refers to the process of sending data signals over a medium from one device to another.

## Transmission Types

Transmission in networking can be categorized into two main types: `analog` and `digital`. `Analog` transmission uses continuous signals to represent information. `Digital` transmission employs discrete signals (bits) to encode data.

## Transmission Modes

Transmission modes define how data is sent between two devices. `Simplex` mode allows one-way communication only, such as from a keyboard to a computer, where signals travel in a single direction. `Half-duplex` mode permits two-way communication but not simultaneously; examples include walkie-talkies where users must take turns speaking. `Full-duplex` mode, used in telephone calls, supports two-way communication simultaneously, allowing both parties to speak and listen at the same time.

## Transmission Medias

The physical means by which data is transmitted in a network is known as transmission media, which can be wired or wireless. Wired media includes `twisted pair` cables, commonly used in Ethernet networks and local area network (LAN) connections; `coaxial` cables, used for cable TV and early Ethernet; and `fiber optic` cables, which transmit data as light pulses and are essential for high-speed internet backbones. Wireless media, on the other hand, encompasses `radio waves` for Wi-Fi and cellular networks, `microwaves` for satellite communications, and `infrared` technology used for short-range communications like remote controls.

# Components

|**Component**|**Description**|
|---|---|
|`End Devices`|Computers, Smartphones, Tablets, IoT / Smart Devices|
|`Intermediary Devices`|Switches, Routers, Modems, Access Points|
|`Network Media and Software Components`|Cables, Protocols, Management and Firewalls Software|
|`Servers`|Web Servers, File Servers, Mail Servers, Database Servers|
## End Devices

An `end device`, also known as a `host`, is any device that ultimately ends up sending or receiving data within a network.

## Intermediary Devices

An `intermediary device` has the unique role of facilitating the flow of data between `end devices`, either within a local area network, or between different networks.
These devices include routers, switches, modems, and access points, all of which play crucial roles in ensuring efficient and secure data transmission. Intermediary devices are responsible for `packet forwarding`, directing data packets to their destinations by reading network address information and determining the most efficient paths. They often incorporate security features like `firewalls` to protect certain networks from unauthorized access and potential threats. Operating at different layers of the OSI model—for example, routers at the `Network Layer (Layer 3)` and switches at the `Data Link Layer (Layer 2)`—use routing tables and protocols to make informed decisions about data forwarding .

### Network Interface Card (NIC)

A `Network Interface Card (NIC)` is a hardware component installed in a computer, or other device, that enables connection to a network. Each NIC has a unique Media Access Control (MAC) address, which is essential for devices to identify each other, and facilitate communication at the data link layer.

### Routers

A `router` is an intermediary device that plays a hugely important role: the forwarding of data packets between networks, and ultimately directing internet traffic. Operating at the network layer (Layer 3) of the OSI model, routers read the network address information in data packets to determine their destinations. They use routing tables and routing protocols such as `Open Shortest Path First (OSPF)` or `Border Gateway Protocol (BGP)` to find the most efficient path for data to travel across interconnected networks, including the internet.
They fulfill this role by `examining incoming data packets` and forwarding them toward their destinations, based on IP addresses. By `connecting multiple networks`, routers enable devices on different networks to communicate. They also manage network traffic by selecting optimal paths for data transmission, which helps prevent congestion—a process known as `traffic management`. Additionally, routers enhance `security` by incorporating features like firewalls and access control lists, protecting the network from unauthorized access and potential threats.

### Switches

The `switch` is another integral component, with its primary job being to connect multiple devices within the same network, typically a Local Area Network (LAN).

### Hubs

A `hub` is a basic (and now antiquated) networking device. It connects multiple devices in a network segment and broadcasts incoming data to all connected ports, regardless of the destination. Operating at the physical layer (Layer 1) of the OSI model, hubs are simpler than switches and do not manage traffic intelligently.

# Network Media and Software Components

## Cabling and Connectors

`Cabling and connectors` are the physical materials used to link devices within a network, forming the pathways through which data is transmitted. This includes the various types of cables mentioned previously, but also connectors like the RJ-45 plug, which is used to interface cables with network devices such as computers, switches, and routers.

## Network Protocols

`Network protocols` are the set of rules and conventions that control how data is formatted, transmitted, received, and interpreted across a network.

- `Data Segmentation`
- `Addressing`
- `Routing`
- `Error Checking`
- `Synchronization`
Common protocols:
- `TCP/IP`: ubiquitous across all internet communications
    
- `HTTP/HTTPS`: The standard for Web traffic
    
- `FTP`: File transfers
    
- `SMTP`: Email transmissions

# Network Management Software

`Network management software` consists of tools and applications used to monitor, control, and maintain network components and operations. These software solutions provide functionalities for:

- `performance monitoring`
- `configuration management`
- `fault analysis`
- `security management`
## Software Firewalls

A `software firewall` is a security application installed on individual computers or devices that monitors and controls incoming and outgoing network traffic based on predetermined security rules. Unlike hardware firewalls that protect entire networks, software firewalls (also called Host-based firewalls) provide protection at the device level, guarding against threats that may bypass the network perimeter defenses.

# Servers

A `server` is a powerful computer designed to provide services to other computers, known as clients, over a network. Servers are the backbone behind websites, email, files, and applications.
servers play a crucial role by hosting services that clients access (i.e., web pages and email services), facilitating `service provision`. They enable `resource sharing` by allowing multiple users to access resources like files and printers. Servers also handle `data management` by storing and managing data centrally, which simplifies backup processes and enhances security management. Additionally, they manage `authentication` by controlling user access and permissions, across multiple components in the network. Servers often run specialized operating systems optimized for handling multiple, simultaneous requests in what is known as the `Client-Server Model`, where the server waits for requests from clients and responds accordingly.

# MAC Address

A `Media Access Control (MAC) address` is a unique identifier assigned to the network interface card (NIC) of a device, allowing it to be recognized on a local network. Operating at the `Data Link Layer (Layer 2)` of the OSI model. Each MAC address is 48 bits long and is typically represented in hexadecimal format, appearing as six pairs of hexadecimal digits separated by colons or hyphens (e.g., `00:1A:2B:3C:4D:5E`). The uniqueness of a MAC address comes from its structure: the first 24 bits represent the `Organizationally Unique Identifier (OUI)` assigned to the manufacturer, while the remaining 24 bits are specific to the individual device.

There are several different standards for the MAC address:

- Ethernet (IEEE 802.3)
- Bluetooth (IEEE 802.15)
- WLAN (IEEE 802.11)


![[Pasted image 20260128183213.png]]

# IP Address

An `Internet Protocol (IP) address` is a numerical label assigned to each device connected to a network that utilizes the Internet Protocol for communication. Functioning at the `Network Layer (Layer 3)` of the OSI model.
There are two versions of IP addresses: `IPv4` and `IPv6`. IPv4 addresses consist of a 32-bit address space, typically formatted as four decimal numbers separated by dots, such as `192.168.1.1`. IPv6 addresses, which were developed to address the depletion of IPv4 addresses, have a 128-bit address space and are formatted in eight groups of four hexadecimal digits, an example being `2001:0db8:85a3:0000:0000:8a2e:0370:7334`.

- `IPv4` / `IPv6` - describes the unique postal address and district of the receiver's building.
- `MAC` - describes the exact floor and apartment of the receiver.

## IPv4 Structure

The most common method of assigning IP addresses is `IPv4`, which consists of a `32`-bit binary number combined into `4 bytes` consisting of `8`-bit groups (`octets`) ranging from `0-255`. These are converted into more easily readable decimal numbers, separated by dots and represented as dotted-decimal notation.

Thus an IPv4 address can look like this:

| **Notation** | **Presentation**                        |
| ------------ | --------------------------------------- |
| Binary       | 0111 1111.0000 0000.0000 0000.0000 0001 |
| Decimal      | 127.0.0.1                               |

Each network interface (network cards, network printers, or routers) is assigned a unique IP address.

The `IPv4` format allows 4,294,967,296 unique addresses. The IP address is divided into a `host part` and a `network part`. The `router` assigns the `host part` of the IP address at home or by an administrator. The respective `network administrator` assigns the `network part`. On the Internet, this is `IANA`, which allocates and manages the unique IPs.

In the past, further classification took place here. The IP network blocks were divided into `classes A - E`. The different classes differed in the host and network shares' respective lengths.

| **`Class`** | **Network Address** | **First Address** | **Last Address** | **Subnetmask** | **CIDR**  | **Subnets** | **IPs**        |
| ----------- | ------------------- | ----------------- | ---------------- | -------------- | --------- | ----------- | -------------- |
| `A`         | 1.0.0.0             | 1.0.0.1           | 127.255.255.255  | 255.0.0.0      | /8        | 127         | 16,777,214 + 2 |
| `B`         | 128.0.0.0           | 128.0.0.1         | 191.255.255.255  | 255.255.0.0    | /16       | 16,384      | 65,534 + 2     |
| `C`         | 192.0.0.0           | 192.0.0.1         | 223.255.255.255  | 255.255.255.0  | /24       | 2,097,152   | 254 + 2        |
| `D`         | 224.0.0.0           | 224.0.0.1         | 239.255.255.255  | Multicast      | Multicast | Multicast   | Multicast      |
| `E`         | 240.0.0.0           | 240.0.0.1         | 255.255.255.255  | reserved       | reserved  | reserved    | reserve        |
|             |                     |                   |                  |                |           |             |                |

### Subnet Mask

A further separation of these classes into small networks is done with the help of `subnetting`. This separation is done using the `netmasks`, which is as long as an IPv4 address. As with classes, it describes which bit positions within the IP address act as `network part` or `host part`.

| **Class** | **Network Address** | **First Address** | **Last Address** | **`Subnetmask`** | **CIDR**  | **Subnets** | **IPs**        |
| --------- | ------------------- | ----------------- | ---------------- | ---------------- | --------- | ----------- | -------------- |
| **A**     | 1.0.0.0             | 1.0.0.1           | 127.255.255.255  | `255.0.0.0`      | /8        | 127         | 16,777,214 + 2 |
| **B**     | 128.0.0.0           | 128.0.0.1         | 191.255.255.255  | `255.255.0.0`    | /16       | 16,384      | 65,534 + 2     |
| **C**     | 192.0.0.0           | 192.0.0.1         | 223.255.255.255  | `255.255.255.0`  | /24       | 2,097,152   | 254 + 2        |
| **D**     | 224.0.0.0           | 224.0.0.1         | 239.255.255.255  | `Multicast`      | Multicast | Multicast   | Multicast      |
| **E**     | 240.0.0.0           | 240.0.0.1         | 255.255.255.255  | `reserved`       | reserved  | reserved    | reserved       |

## Network and Gateway Address

The `two` additional `IPs` added in the `IPs column` are reserved for the so-called `network address` and the `broadcast address`. Another important role plays the `default gateway`, which is the name for the IPv4 address of the `router` that couples networks and systems with different protocols and manages addresses and transmission methods. It is common for the `default gateway` to be assigned the first or last assignable IPv4 address in a subnet. This is not a technical requirement, but has become a de-facto standard in network environments of all sizes.

## Broadcast Address

The `broadcast` IP address's task is to connect all devices in a network with each other. `Broadcast` in a network is a message that is transmitted to all participants of a network and does not require any response. In this way, a host sends a data packet to all other participants of the network simultaneously and, in doing so, communicates its `IP address`, which the receivers can use to contact it. This is the `last IPv4` address that is used for the `broadcast`.

## Binary System in IPv4

An IPv4 address is divided into 4 octets. Each `octet` consists of `8 bits`. Each position of a bit in an octet has a specific decimal value.

IPv4 Address: `192.168.10.39`

### 1st Octet - Value: 192

```
Values:         128  64  32  16  8  4  2  1
Binary:           1   1   0   0  0  0  0  0
```

| **Octet** | **Values**                       | **Sum** |
| --------- | -------------------------------- | ------- |
| 1st       | 128 + 64 + 0 + 0 + 0 + 0 + 0 + 0 | = `192` |
| 2nd       | 128 + 0 + 32 + 0 + 8 + 0 + 0 + 0 | = `168` |
| 3rd       | 0 + 0 + 0 + 0 + 8 + 0 + 2 + 0    | = `10`  |
| 4th       | 0 + 0 + 32 + 0 + 0 + 4 + 2 + 1   | = `39`  |

## CIDR

`Classless Inter-Domain Routing` (`CIDR`) is a method of representation and replaces the fixed assignment between IPv4 address and network classes (A, B, C, D, E). The division is based on the subnet mask or the so-called `CIDR suffix`, which allows the bitwise division of the IPv4 address space and thus into `subnets` of any size. The `CIDR suffix` indicates how many bits from the beginning of the IPv4 address belong to the network. It is a notation that represents the `subnet mask` by specifying the number of `1`-bits in the subnet mask.

- IPv4 Address: `192.168.10.39`
- Subnet mask: `255.255.255.0`
- CIDR: `192.168.10.39/24`

The CIDR suffix is, therefore, the sum of all ones in the subnet mask.

```
Octet:             1st         2nd         3rd         4th
Binary:         1111 1111 . 1111 1111 . 1111 1111 . 0000 0000 (/24)
Decimal:           255    .    255    .    255    .     0
```

# Subnetting

The division of an address range of IPv4 addresses into several smaller address ranges is called `subnetting`.
A subnet is a logical segment of a network that uses IP addresses with the same network address. We can think of a subnet as a labeled entrance on a large building corridor.

With the help of subnetting, we can create a specific subnet by ourselves or find out the following outline of the respective network:

- `Network address`
- `Broadcast address`
- `First host`
- `Last host`
- `Number of hosts`

- IPv4 Address: `192.168.12.160`
- Subnet Mask: `255.255.255.192`
- CIDR: `192.168.12.160/26`

IP address is divided into the `network part` and the `host part`.

### Network Part

| **Details of** | **1st Octet** | **2nd Octet** | **3rd Octet** | **4th Octet** | **Decimal**         |
| -------------- | ------------- | ------------- | ------------- | ------------- | ------------------- |
| IPv4           | `1100 0000`   | `1010 1000`   | `0000 1100`   | `10`10 0000   | 192.168.12.160`/26` |
| Subnet mask    | `1111 1111`   | `1111 1111`   | `1111 1111`   | `11`00 0000   | `255.255.255.192`   |
| Bits           | /8            | /16           | /24           | /32           |                     |
In subnetting, we use the subnet mask as a template for the IPv4 address. From the `1`-bits in the subnet mask, we know which bits in the IPv4 address `cannot` be changed. These are `fixed` and therefore determine the "main network" in which the subnet is located.

### Host Part

| **Details of** | **1st Octet** | **2nd Octet** | **3rd Octet** | **4th Octet** | **Decimal**       |
| -------------- | ------------- | ------------- | ------------- | ------------- | ----------------- |
| IPv4           | 1100 0000     | 1010 1000     | 0000 1100     | 10`10 0000`   | 192.168.12.160/26 |
| Subnet mask    | 1111 1111     | 1111 1111     | 1111 1111     | 11`00 0000`   | 255.255.255.192   |
| Bits           | /8            | /16           | /24           | /32           |                   |
The bits in the `host part` can be changed to the `first` and `last` address. The first address is the `network address`, and the last address is the `broadcast address` for the respective subnet.

The `network address` is vital for the delivery of a data packet. If the `network address` is the same for the source and destination address, the data packet is delivered within the same subnet. If the network addresses are different, the data packet must be routed to another subnet via the `default gateway`.

The `subnet mask` determines where this separation occurs.

### Seperation of Network and Host Parts

| **Details of** | **1st Octet** | **2nd Octet** | **3rd Octet** | **4th Octet** | **Decimal**       |
| -------------- | ------------- | ------------- | ------------- | ------------- | ----------------- |
| IPv4           | 1100 0000     | 1010 1000     | 0000 1100     | 10`\|`10 0000 | 192.168.12.160/26 |
| Subnet mask    | `1111 1111`   | `1111 1111`   | `1111 1111`   | `11\|`00 0000 | 255.255.255.192   |
| Bits           | /8            | /16           | /24           | /32           |                   |
### Network Address

So if we now set all bits to `0` in the `host part` of the IPv4 address, we get the respective subnet's `network address`.

| **Details of** | **1st Octet** | **2nd Octet** | **3rd Octet** | **4th Octet** | **Decimal**         |
| -------------- | ------------- | ------------- | ------------- | ------------- | ------------------- |
| IPv4           | 1100 0000     | 1010 1000     | 0000 1100     | 10`\|00 0000` | `192.168.12.128`/26 |
| Subnet mask    | `1111 1111`   | `1111 1111`   | `1111 1111`   | `11\|`00 0000 | 255.255.255.192     |
| Bits           | /8            | /16           | /24           | /32           |                     |
### Broadcast Address

If we set all bits in the `host part` of the IPv4 address to `1`, we get the `broadcast address`.

| **Details of** | **1st Octet** | **2nd Octet** | **3rd Octet** | **4th Octet** | **Decimal**         |
| -------------- | ------------- | ------------- | ------------- | ------------- | ------------------- |
| IPv4           | 1100 0000     | 1010 1000     | 0000 1100     | 10`\|11 1111` | `192.168.12.191`/26 |
| Subnet mask    | `1111 1111`   | `1111 1111`   | `1111 1111`   | `11\|`00 0000 | 255.255.255.192     |
| Bits           | /8            | /16           | /24           | /32           |                     |
Since we now know that the IPv4 addresses `192.168.12.128` and `192.168.12.191` are assigned, all other IPv4 addresses are accordingly between `192.168.12.129-190`. Now we know that this subnet offers us a total of `64 - 2` (network address & broadcast address) or `62` IPv4 addresses that we can assign to our hosts.

| **Hosts**         | **IPv4**         |
| ----------------- | ---------------- |
| Network Address   | `192.168.12.128` |
| First Host        | `192.168.12.129` |
| Other Hosts       | `...`            |
| Last Host         | `192.168.12.190` |
| Broadcast Address | `192.168.12.191` |
## Subnetting Into Smaller Networks

| **Exponent** | **Value** |
| ------------ | --------- |
| 2`^0`        | = 1       |
| 2`^1`        | = 2       |
| 2`^2`        | = 4       |
| 2`^3`        | = 8       |
| 2`^4`        | = 16      |
| 2`^5`        | = 32      |
| 2`^6`        | = 64      |
| 2`^7`        | = 128     |
| 2`^8`        | = 256     |

# Ports

A `port` is a number assigned to specific processes or services on a network to help computers sort and direct network traffic correctly. It functions at the `Transport Layer (Layer 4)` of the OSI model and works with protocols such as TCP and UDP. Ports facilitate the simultaneous operation of multiple network services on a single IP address by differentiating traffic intended for different applications.

## Well Known Ports

`Well-known ports`, numbered from 0 to 1023, are reserved for common and universally recognized services and protocols, as standardized and managed by the [Internet Assigned Numbers Authority (IANA)](https://www.iana.org/).

## Registered Ports (1024-49151)

`Registered ports`, which range from 1024 to 49151, are not as strictly regulated as `well-known ports` but are still registered and assigned to specific services by the Internet Assigned Numbers Authority (IANA).

## Dynamic/Private Ports (49152-65535)

Dynamic or private ports, also known as ephemeral ports, range from 49152 to 65535 and are typically used by client applications to send and receive data from servers, such as when a web browser connects to a server on the internet. These ports are called `dynamic` because they are not fixed; rather, they can be randomly selected by the client's operating system as needed for each session.

# DHCP

`DHCP` is a network management protocol used to automate the process of configuring devices on IP networks. It allows devices to automatically receive an IP address and other network configuration parameters, such as subnet mask, default gateway, and DNS servers, without manual intervention.

The DHCP process involves a series of interactions between the client (the device requesting an IP address) and the DHCP server (the service running on a network device that assigns IP addresses). This process is often referred to as `DORA`, an acronym for `Discover`, `Offer`, `Request`, and `Acknowledge`.

|**Role**|**Description**|
|---|---|
|`DHCP Server`|A network device (like a router or dedicated server) that manages IP address allocation. It maintains a pool of available IP addresses and configuration parameters.|
|`DHCP Client`|Any device that connects to the network and requests network configuration parameters from the DHCP server.|

|**Step**|**Description**|
|---|---|
|`1. Discover`|When a device connects to the network, it broadcasts a **DHCP Discover** message to find available DHCP servers.|
|`2. Offer`|DHCP servers on the network receive the discover message and respond with a **DHCP Offer** message, proposing an IP address lease to the client.|
|`3. Request`|The client receives the offer and replies with a **DHCP Request** message, indicating that it accepts the offered IP address.|
|`4. Acknowledge`|The DHCP server sends a **DHCP Acknowledge** message, confirming that the client has been assigned the IP address. The client can now use the IP address to communicate on the network.|
![[Pasted image 20260128183735.png]]

# NAT (Network Address Translation)

`Network Address Translation (NAT)` is a process carried out by a router or a similar device that modifies the source or destination IP address in the headers of IP packets as they pass through. This modification is used to translate the private IP addresses of devices within a local network to a single public IP address that is assigned to the router.

## Private IP and Public IP

`Public IP` addresses are globally unique identifiers assigned by Internet Service Providers (ISPs). Devices equipped with these IP addresses can be accessed from anywhere on the Internet, allowing them to communicate across the global network.

`Private IP` addresses are designated for use within local networks such as homes, schools, and offices. These addresses are not routable on the global internet, meaning packets sent to these addresses are not forwarded by internet backbone routers. Defined by RFC 1918, common IPv4 private address ranges include 10.0.0.0 to 10.255.255.255, 172.16.0.0 to 172.31.255.255, and 192.168.0.0 to 192.168.255.255.

![[Pasted image 20260128184403.png]]

## Types

|**Type**|**Description**|
|---|---|
|`Static NAT`|Involves a one-to-one mapping, where each private IP address corresponds directly to a public IP address.|
|`Dynamic NAT`|Assigns a public IP from a pool of available addresses to a private IP as needed, based on network demand.|
|`Port Address Translation (PAT)`|Also known as NAT Overload, is the most common form of NAT in home networks. Multiple private IP addresses share a single public IP address, differentiating connections by using unique port numbers. This method is widely used in home and small office networks, allowing multiple devices to share a single public IP address for internet access.|
## Pros and Cons

|**Benefits**|
|---|
|Conserves the limited IPv4 address space.|
|Provides a basic layer of security by not exposing internal network structure directly.|
|Flexible for internal IP addressing schemes.|

|**Trade-Offs**|
|---|
|Complex services like hosting a public server behind NAT can require additional configuration (e.g., port forwarding).|
|NAT can break certain protocols that rely on end-to-end connectivity without special handling.|
|Adds complexity to troubleshooting connectivity issues.|

# Domain Name System (DNS)

The Domain Name System (DNS) is like the phonebook of the internet. It helps us find the right number (an IP address) for a given name (a domain such as `www.google.com`). Without DNS, we would need to memorize long, often complex IP addresses for every website we visit. DNS makes our lives easier by allowing us to use human-friendly names to access online resources.

## DNS and IP address

| **Address**   | **Description**                                                            |
| ------------- | -------------------------------------------------------------------------- |
| `Domain Name` | A readable address like `www.example.com` that people can easily remember. |
| `IP Address`  | A numerical label (e.g., `93.184.216.34`)                                  |

## Hierarchy

|**Layer**|**Description**|
|---|---|
|`Root Servers`|The top of the DNS hierarchy.|
|`Top-Level Domains (TLDs)`|Such as `.com`, `.org`, `.net`, or country codes like `.uk`, `.de`.|
|`Second-Level Domains`|For example, `example` in `example.com`.|
|`Subdomains or Hostname`|For instance, `www` in `www.example.com`, or `accounts` in `accounts.google.com`.|

![[Pasted image 20260128185916.png]]

## DNS Resolution Protocol (Domain Translation)

When we enter a domain name in our browser, the computer needs to find the corresponding IP address. This process is known as `DNS resolution` or `domain translation`.

|**Step**|**Description**|
|---|---|
|`Step 1`|We type `www.example.com` into our browser.|
|`Step 2`|Our computer checks its local DNS cache (a small storage area) to see if it already knows the IP address.|
|`Step 3`|If not found locally, it queries a `recursive DNS server`. This is often provided by our Internet Service Provider or a third-party DNS service like Google DNS.|
|`Step 4`|The recursive DNS server contacts a `root server`, which points it to the appropriate `TLD name server` (such as the `.com` domains, for instance).|
|`Step 5`|The TLD name server directs the query to the `authoritative name server` for `example.com`.|
|`Step 6`|The authoritative name server responds with the IP address for `www.example.com`.|
|`Step 7`|The recursive server returns this IP address to your computer, which can then connect to the website’s server directly.|
![[Pasted image 20260128190032.png]]

# Internet Architecture

## Peer-to-Peer (P2P Architecture)

In a `Peer-to-Peer (P2P`) network, each node, whether it's a computer or any other device, acts as both a client and a server. This setup allows nodes to communicate directly with each other, sharing resources such as files, processing power, or bandwidth, without the need for a central server. P2P networks can be fully decentralized, with no central server involved, or partially centralized, where a central server may coordinate some tasks but does not host data.
A popular example of Peer-to-Peer (P2P) architecture is torrenting, as seen with applications like BitTorrent. In this system, anyone who has the file, referred to as a `seeder`, can upload it, allowing others to download it from multiple sources simultaneously.
![[Pasted image 20260128191800.png]]

|**Advantage**|**Description**|
|---|---|
|`Scalability`|Adding more nodes can increase total resources (storage, CPU, etc.).|
|`Resilience`|If one node goes offline, others can continue functioning.|
|`Cost distribution`|Resource burden, like bandwidth and storage, is distributed among peers, making it more cost-efficient.|

|**Disadvantage**|**Description**|
|---|---|
|`Management complexity`|Harder to control and manage updates/security policies across all nodes|
|`Potential reliability issues`|If too many peers leave, resources could be unavailable.|
|`Security challenges`|Each node is exposed to potential vulnerabilities.|

## Client-Server Architecture

The `Client-Server` model is one of the most widely used architectures on the Internet. In this setup, clients, which are user devices, send requests, such as a web browser asking for a webpage, and servers respond to these requests, like a web server hosting the webpage. This model typically involves centralized servers where data and applications reside, with multiple clients connecting to these servers to access services and resources.
![[Pasted image 20260128191837.png]]

### Single-Tier Architecture
In a `single-tier` architecture, the client, server, and database all reside on the same machine. This setup is straightforward but is rarely used for large-scale applications due to significant limitations in scalability and security.

### Two-Tier Architecture
The `two-tier` architecture splits the application environment into a client and a server. The client handles the presentation layer, and the server manages the data layer. This model is typically seen in desktop applications where the user interface is on the user's machine, and the database is on a server. Communication usually occurs directly between the client and the server, which can be a database server with query-processing capabilities.

### Three-Tier Architecture
A `three-tier` architecture introduces an additional layer between the client and the database server, known as the application server. In this model, the client manages the presentation layer, the application server handles all the business logic and processing, and the third tier is a database server. This separation provides added flexibility and scalability because each layer can be developed and maintained independently.

### N-Tier Architecture
In more complex systems, an `N-tier` architecture is used, where `N` refers to any number of separate tiers used beyond three. This setup involves multiple levels of application servers, each responsible for different aspects of business logic, processing, or data management. N-tier architectures are highly scalable and allow for distributed deployment, making them ideal for web applications and services that demand robust, flexible solutions.

|**Advantage**|**Description**|
|---|---|
|`Centralized control`|Easier to manage and update.|
|`Security`|Central security policies can be applied.|
|`Performance`|Dedicated servers can be optimized for their tasks.|

|**Disadvantage**|**Description**|
|---|---|
|`Single point of failure`|If the central server goes down, clients lose access.|
|`High Cost and Maintenance`|Setting up and sustaining a client-server architecture is expensive, requiring constant operation and expert management , making it costly to maintain.|
|`Network Congestion`|High traffic on the network can lead to congestion, slowing down or even disrupting connections when too many clients access the server simultaneously.|

## Hybrid Architecture

A `Hybrid` model blends elements of both `Client-Server` and `Peer-to-Peer (P2P)` architectures. In this setup, central servers are used to facilitate coordination and authentication tasks, while the actual data transfer occurs directly between peers. This combination leverages the strengths of both architectures to enhance efficiency and performance. The following example gives a high-level explanation of how a hybrid architecture works.
![[Pasted image 20260128192125.png]]

|**Advantage**|**Description**|
|---|---|
|`Efficiency`|Relieves workload from servers by letting peers share data.|
|`Control`|Central server can still manage user authentication, directory services, or indexing.|

|**Disadvantage**|**Description**|
|---|---|
|`Complex Implementation`|Requires more sophisticated design to handle both centralized and distributed components.|
|`Potential Single Point of Failure`|If the central coordinating server fails, peer discovery might stop.|

## Cloud Architecture

`Cloud Architecture` refers to computing infrastructure that is hosted and managed by third-party providers, such as AWS, Azure, and Google Cloud. This architecture operates on a virtualized scale following a client-server model. It provides on-demand access to resources such as servers, storage, and applications, all accessible over the Internet. In this model, users interact with these services without controlling the underlying hardware.
![[Pasted image 20260128192201.png]]

|**Characteristic**|**Description**|
|---|---|
|`1. On-demand self-service`|Automatically set up and manage the services without human help.|
|`2. Broad network access`|Access services from any internet-connected device.|
|`3. Resource pooling`|Share and allocate service resources dynamically among multiple users.|
|`4. Rapid elasticity`|Quickly scale services up or down based on demand.|
|`5. Measured service`|Only pay for the resources you use, tracked with precision.|

|**Advantage**|**Description**|
|---|---|
|`Scalability`|Easily add or remove computing resources as needed.|
|`Reduced cost & maintenance`|Hardware managed by the cloud provider.|
|`Flexibility`|Access services from anywhere with Internet connectivity.|

|**Disadvantage**|**Description**|
|---|---|
|`Vendor lock-in`|Migrating from one cloud provider to another can be complex.|
|`Security/Compliance`|Relying on a third party for data hosting can introduce concerns about data privacy.|
|`Connectivity`|Requires stable Internet access.|
## Software-Defined Architecture (SDN)
`Software-Defined Networking (SDN)` is a modern networking approach that separates the control plane, which makes decisions about where traffic is sent, from the data plane, which actually forwards the traffic. Traditionally, network devices like routers and switches housed both of these planes. However, in SDN, the control plane is centralized within a software-based controller. This configuration allows network devices to simply execute instructions they receive from the controller. SDN provides a programmable network management environment, enabling administrators to dynamically adjust network policies and routing as required. This separation makes the network more flexible and improves how it's managed.
![[Pasted image 20260128192310.png]]

|**Advantage**|**Description**|
|---|---|
|`Centralized control`|Simplifies network management.|
|`Programmability & Automation`|Network configurations can be changed quickly through software instead of manually configuring each device.|
|`Scalability & Efficiency`|Can optimize traffic flows dynamically, leading to better resource utilization.|

|**Disadvantage**|**Description**|
|---|---|
|`Controller Vulnerability`|If the central controller goes down, the network might be adversely affected.|
|`Complex Implementation`|Requires new skill sets and specialized software/hardware.|

## Key Comparisons

|`Architecture`|`Centralized`|`Scalability`|`Ease of Management`|`Typical Use Cases`|
|---|---|---|---|---|
|`P2P`|Decentralized (or partial)|High (as peers grow)|Complex (no central control)|File-sharing, blockchain|
|`Client-Server`|Centralized|Moderate|Easier (server-based)|Websites, email services|
|`Hybrid`|Partially central|Higher than C-S|More complex management|Messaging apps, video conferencing|
|`Cloud`|Centralized in provider’s infra|High|Easier (outsourced)|Cloud storage, SaaS, PaaS|
|`SDN`|Centralized control plane|High (policy-driven)|Moderate (needs specialized tools)|Datacenters, large enterprises|

# Wireless Networks

## Wireless Router
|**Function**|**Description**|
|---|---|
|`Routing`|Directing data to the correct destination (within your network or on the internet).|
|`Wireless Access Point`|Providing Wi-Fi coverage.|
### Components
|**Component**|**Description**|
|---|---|
|`WAN (Wide Area Network) Port`|Connects to your internet source (e.g., a cable modem).|
|`LAN (Local Area Network) Ports`|For wired connections to local devices (e.g., desktop computer, printer).|
|`Antennae`|Transmit and receive wireless signals. (Some routers have internal antennae.)|
|`Processor & Memory`|Handle routing and network management tasks.|
## Mobile Hotspots
A `mobile hotspot` allows a smartphone (or other hotspot devices) to share its cellular data connection via Wi-Fi. Other devices (laptops, tablets, etc.) then connect to this hotspot just like they would to a regular Wi-Fi network.

## Cell Tower
A `cell tower` (or `cell site`) is a structure where antennas and electronic communications equipment are placed to create a cellular network cell. This `cell` in a cellular network refers to the specific area of coverage provided by a single cell tower, which is designed to seamlessly connect with adjacent cells created by other towers. Each tower covers a certain geographic area, allowing mobile phones (and other cellular-enabled devices) to send and receive signals.

## Frequencies Involved

|**Frequency Bands**|
|---|
|`1.` **2.4 GHz (Gigahertz)** – Used by older Wi-Fi standards (802.11b/g/n). Better at penetrating walls, but can be more prone to interference (e.g., microwaves, Bluetooth).|
|`2.` **5 GHz** – Used by newer Wi-Fi standards (802.11a/n/ac/ax). Faster speeds, but shorter range.|
|`3.` **Cellular Bands** – For 4G (LTE) and 5G. These range from lower frequencies (700 MHz) to mid-range (2.6 GHz) and even higher frequencies for some 5G services (up to 28 GHz and beyond).|

# Network Security

Goal is to maintain `CIA Triad`

|**Principle**|**Description**|
|---|---|
|`Confidentiality`|Only authorized users can view the data.|
|`Integrity`|The data remains accurate and unaltered.|
|`Availability`|Network resources are accessible when needed.|
## Firewalls
A `Firewall` is a network security device, either hardware, software, or a combination of both, that monitors incoming and outgoing network traffic. Firewalls enforce a set of rules (known as `firewall policies` or `access control lists`) to determine whether to `allow` or `block` specific traffic.

### Packet Filtering Firewalls
|**Description**|
|---|
|Operates at Layer 3 (Network) and Layer 4 (Transport) of the OSI model.|
|Examines source/destination IP, source/destination port, and protocol type.|
|`Example`: A simple router ACL that only allows HTTP (port 80) and HTTPS (port 443) while blocking other ports.|
### Stateful Inspection Firewall
|**Description**|
|---|
|Tracks the state of network connections.|
|More intelligent than packet filters because they understand the entire conversation.|
|`Example`: Only allows inbound data that matches an already established outbound request.|
### Application Layer Firewall (Proxy Firewall)
|**Description**|
|---|
|Operates up to Layer 7 (Application) of the OSI model.|
|Can inspect the actual content of traffic (e.g., HTTP requests) and block malicious requests.|
|`Example`: A web proxy that filters out malicious HTTP requests containing suspicious patterns.|
### Next Gen Firewall (NGFW)
|**Description**|
|---|
|Combines stateful inspection with advanced features like deep packet inspection, intrusion detection/prevention, and application control.|
|`Example`: A modern firewall that can block known malicious IP addresses, inspect encrypted traffic for threats, and enforce application-specific policies.|
![[Pasted image 20260128223559.png]]

## Intrusion Detection and Intrusion Prevention System (IDS/IPS)

An `Intrusion Detection System (IDS)` observes traffic or system events to identify malicious behavior or policy violations, generating alerts but not blocking the suspicious traffic.

An `Intrusion Prevention System (IPS)` operates similarly to an IDS but takes an additional step by preventing or rejecting malicious traffic in real time.

The key difference lies in their actions: an IDS detects and alerts, while an IPS detects and prevents.

|**Techniques**|**Description**|
|---|---|
|`Signature-based detection`|Matches traffic against a database of known exploits.|
|`Anomaly-based detection`|Detects anything unusual compared to normal activity.|
### Network-Based IDS/IPS (NIDS/NIPS)
|**Description**|
|---|
|Hardware device or software solution placed at strategic points in the network to inspect all passing traffic.|
|`Example`: A sensor connected to the core switch that monitors traffic within a data center.|
### Host-Based IDS/IPS(HIDS/HIPS)
|**Description**|
|---|
|Runs on individual hosts or devices, monitoring inbound/outbound traffic and system logs for suspicious behavior on that specific machine.|
|`Example`: An antivirus or endpoint security agent installed on a server.|
![[Pasted image 20260128223828.png]]

## Best Practices
|**Practice**|**Description**|
|---|---|
|`Define Clear Policies`|Consistent firewall rules based on the principle of `least privilege` (only allow what is necessary).|
|`Regular Updates`|Keep firewall, IDS/IPS signatures, and operating systems up to date to defend against the latest threats.|
|`Monitor and Log Events`|Regularly review firewall logs, IDS/IPS alerts, and system logs to identify suspicious patterns early.|
|`Layered Security`|Use `defense in depth` (a strategy that leverages multiple security measures to slow down an attack) with multiple layers: Firewalls, IDS/IPS, antivirus, and endpoint protection to cover different attack vectors.|
|`Periodic Penetration Testing`|Test the effectiveness of the security policies and devices by simulating real attacks.|

# Data Flow

## 1. Accessing the Internet
|**Steps**|
|---|
|The laptop first identifies the correct wireless network/SSID|
|If the network uses WPA2/WPA3, the user must provide the correct password or credentials to authenticate.|
|Finally, the connection is established, and the DHCP protocol takes over the IP configuration.|
## 2. Checking Local Network Config (DHCP)
|**Steps**|**Description**|
|---|---|
|`IP Address Assignment`|If the laptop does not already have an IP, it requests one from the home router's `DHCP` server. This IP address is only valid within the local network.|
|`DHCP Acknowledgement`|The DHCP server assigns a private IP address (for example, _192.168.1.10_) to the laptop, along with other configuration details such as subnet mask, default gateway, and DNS server.|
## 3. DNS Resolution
|**Steps**|**Description**|
|---|---|
|`DNS Query`|The laptop sends a DNS query to the DNS server, which is typically an external DNS server provided by the ISP or a third-party service like Google DNS.|
|`DNS Response`|The DNS server looks up the domain `www.example.com` and returns its IP address (e.g., 93.184.216.34).|
## 4. Data Encapsulation and Local Network Transmission
|**Steps**|**Description**|
|---|---|
|`Application Layer`|The browser creates an HTTP (or HTTPS) request for the webpage.|
|`Transport Layer`|The request is wrapped in a TCP segment (or UDP, but for web traffic it's typically TCP). This segment includes source and destination ports (HTTP default port 80, HTTPS default port 443).|
|`Internet Layer`|The TCP segment is placed into an IP packet. The source IP is the laptop's private IP (e.g., 192.168.1.10), and the destination IP is the remote server’s IP (93.184.216.34).|
|`Link Layer`|The IP packet is finally placed into an Ethernet frame (if we're on Ethernet) or Wi-Fi frame. Here, the MAC (Media Access Control) addresses are included (source MAC is the laptop's network interface, and destination MAC is the router's interface).|
### 5. Network Address Translation (NAT)
Once the router receives the frame, it processes the IP packet. At this point, the router replaces the private IP (192.168.1.10) with its public IP address (e.g., 203.0.113.45) in the packet header. This process is known as `Network Address Translation (NAT)`.

### 6. Server Receives the Request and Respond
Upon reaching the destination network, the server's firewall, if there is one, checks if the incoming traffic on port 80 (HTTP) or 443 (HTTPS) is allowed. If it passes firewall rules, it goes to the server hosting `www.example.com`. Next, the web server software (e.g., Apache, Nginx, IIS) receives and processes the request, prepares the webpage (HTML, CSS, images, etc.), and sends it back as a response.

### 7. Decapsulation and Display
Finally, our laptop receives the response and strips away the Ethernet/Wi-Fi frame, the IP header, and the TCP header, until the application layer data is extracted. The laptop's browser reads the HTML/CSS/JavaScript, and ultimately displays the webpage.

![[Pasted image 20260128224140.png]]

# Topologies

A `network topology` is a typical arrangement and `physical` or `logical` connection of devices in a network. Computers are `hosts`, such as `clients` and `servers`, that actively use the network.
The `transmission medium layout` used to connect devices is the physical topology of the network. For conductive or glass fiber media, this refers to the cabling plan, the positions of the `nodes`, and the connections between the nodes and the cabling. In contrast, the `logical topology` is how the signals act on the network media or how the data will be transmitted across the network from one device to the devices' physical connection.

## Connections

|`Wired connections`|`Wireless connections`|
|---|---|
|Coaxial cabling|Wi-Fi|
|Glass fiber cabling|Cellular|
|Twisted-pair cabling|Satellite|
|and others|and others|
## Nodes

|              |          |           |          |
| ------------ | -------- | --------- | -------- |
| Repeaters    | Hubs     | Bridges   | Switches |
| Router/Modem | Gateways | Firewalls |          |
Network nodes are the `transmission medium's connection points` to transmitters and receivers of electrical, optical, or radio signals in the medium. A node may be connected to a computer, but certain types may have only one microcontroller on a node or may have no programmable device at all.
## Classifications

|   |   |
|---|---|
|Point-to-Point|Bus|
|Star|Ring|
|Mesh|Tree|
|Hybrid|Daisy Chain|
## Point-to-Point

The simplest network topology with a dedicated connection between two hosts is the `point-to-point` topology. In this topology, a direct and straightforward physical link exists only between `two hosts`.
`Point-to-point` topologies are the basic model of traditional telephony and must not be confused with `P2P` (`Peer-to-Peer` architecture).
![[Pasted image 20260130122800.png]]

## Bus

All hosts are connected via a transmission medium in the bus topology. Every host has access to the transmission medium and the signals that are transmitted over it. There is no central network component that controls the processes on it. The transmission medium for this can be, for example, a `coaxial cable`.
Since the medium is shared with all the others, only `one host can send`, and all the others can only receive and evaluate the data and see whether it is intended for itself.
![[Pasted image 20260130122842.png]]

## Star

The star topology is a network component that maintains a connection to all hosts. Each host is connected to the `central network component` via a separate link. This is usually a router, a hub, or a switch. These handle the `forwarding function` for the data packets. To do this, the data packets are received and forwarded to the destination. The data traffic on the central network component can be very high since all data and connections go through it.
![[Pasted image 20260130122907.png]]

## Ring

The `physical` ring topology is such that each host or node is connected to the ring with two cables:

- One for the `incoming` signals and
- the another for the `outgoing` ones.

This means that one cable arrives at each host and one cable leaves. The ring topology typically does not require an active network component. The control and access to the transmission medium are regulated by a protocol to which all stations adhere.

A `logical` ring topology is based on a physical star topology, where a distributor at the node simulates the ring by forwarding from one port to the next.
![[Pasted image 20260130122954.png]]

## Mesh

Many nodes decide about the connections on a `physical` level and the routing on a `logical` level in meshed networks. Therefore, meshed structures have no fixed topology. There are two basic structures from the basic concept: the `fully meshed` and the `partially meshed` structure.
Each host is connected to every other host in the network in a `fully meshed structure`. This means that the hosts are meshed with each other. This technique is primarily used in `WAN` or `MAN` to ensure high reliability and bandwidth.

In the `partially meshed structure`, the endpoints are connected by only one connection. In this type of network topology, specific nodes are connected to exactly one other node, and some other nodes are connected to two or more other nodes with a point-to-point connection.
![[Pasted image 20260130125801.png]]

## Tree

The tree topology is an extended star topology that more extensive local networks have in this structure.
There are both logical tree structures according to the `spanning tree` and physical ones. Modular modern networks, based on structured cabling with a hub hierarchy, also have a tree structure. Tree topologies are also used for `broadband networks` and `city networks` (`MAN`).
![[Pasted image 20260130125835.png]]

## Hybrid

Hybrid networks combine two or more topologies so that the resulting network does not present any standard topologies.
A hybrid topology is always created when `two different` basic network topologies are interconnected.
![[Pasted image 20260130125906.png]]

## Daisy Chain

In the daisy chain topology, multiple hosts are connected by placing a cable from one node to another.
Since this creates a chain of connections, it is also known as a daisy-chain configuration in which multiple hardware components are connected in a series. This type of networking is often found in automation technology (`CAN`).
![[Pasted image 20260130125935.png]]

# IPX (Internetwork Packet Exchange)

`IPX/SPX or Internetwork Packet Exchange/Sequenced Packet Exchange was developed by Novell to be a replacement to the TCP/IP Protocol Suite.`

## Working

IPX is the network layer and SPX is the transport layer of the IPX/SPX network protocol. IPX and IP protocol have similar functions and this defines how data is sent and received between devices. The transport layer protocol or SPX protocol is used to establish and maintain a connection between devices. Together, they can be used to transfer data and create a network connection between systems. IPX does not require a consistent connection to be maintained while packets are being sent from one system to another, this is what is called being connectionless. It can resume the transfer from the point where it was interrupted due to bad connection or power loss.

## Applications

IPX provides peer-to-peer support connectivity. Like IP, IPX also contains end-user data and is connectionless, just like network addresses. 

## Advantages

- IPX/SPX was primarily designed for local area networks (LANs) and is very efficient when used for this only.
- IPX has a larger address space: 48 bits instead of 32 bits in IPv4.
- IPX addresses incorporate the local MAC address:compared to "address assignment" like with IPv4.
- No BootP or DHCP in IPX. (DHCP was invented from BootP was so that IPv4 could allow "plug-and-go" network addressing like what IPX did. It was later added in IPv6.)

## Disadvantages

- Nowadays IPX is falling out of trend. TCP/IP is mostly used because of its superior performance over wide area networks and the Internet and its a more mature protocol created with the same purpose in mind. The real advantage of TCP/IP is interoperability and vendor-independent open standards.
- With IPX applications and the use of the internet, the costs are higher if you are implementing VPNs.
- Encapsulating and encrypting of IPX frames in an IP packet requires expensive hardware than performing a straight IPSec VPN.

# Proxies

A proxy is when a device or service sits in the middle of a connection and acts as a mediator. The `mediator` is the critical piece of information because it means the device in the middle must be able to inspect the contents of the traffic. Without the ability to be a `mediator`, the device is technically a `gateway`, not a proxy.

Proxies will almost always operate at `Layer 7 of the OSI Model`. 
Types of proxy services:
1. `Dedicated Proxy` / `Forward Proxy`
2. `Reverse Proxy`
3. `Transparent Proxy`

## Dedicated Proxy

A `Forward Proxy` is when a client makes a request to a computer, and that computer carries out the request.
This can be an incredibly powerful line of defense against malware, as not only does it need to bypass the web filter (easy), but it would also need to be `proxy aware` or use a non-traditional C2.

Web Browsers like Internet Explorer, Edge, or Chrome all obey the "System Proxy" settings by default. If the malware utilizes WinSock (Native Windows API), it will likely be proxy aware without any additional code. Firefox does not use `WinSock` and instead uses `libcurl`, which enables it to use the same code on any operating system. This means that the malware would need to look for Firefox and pull the proxy settings, which malware is highly unlikely to do.

![[Pasted image 20260710163438.png]]

## Reverse Proxy

A `reverse proxy`, is the reverse of a `Forward Proxy`. Instead of being designed to filter outgoing requests, it filters incoming ones. The most common goal with a `Reverse Proxy`, is to listen on an address and forward it to a closed-off network.

Many organizations use CloudFlare as they have a robust network that can withstand most DDOS Attacks. By using Cloudflare, organizations have a way to filter the amount (and type) of traffic that gets sent to their webservers.

Penetration Testers will configure reverse proxies on infected endpoints. The infected endpoint will listen on a port and send any client that connects to the port back to the attacker through the infected endpoint. This is useful to bypass firewalls or evade logging. Organizations may have `IDS` (`Intrusion Detection Systems`), watching external web requests. If the attacker gains access to the organization over SSH, a reverse proxy can send web requests through the SSH Tunnel and evade the IDS.

Another common Reverse Proxy is `ModSecurity`, a `Web Application Firewall` (`WAF`). Web Application Firewalls inspect web requests for malicious content and block the request if it is malicious.

![[Pasted image 20260710163728.png]]

## (Non- ) Transparent Proxy

All these proxy services act either `transparently` or `non-transparently`.

With a `transparent proxy`, the client doesn't know about its existence. The transparent proxy intercepts the client's communication requests to the Internet and acts as a substitute instance. To the outside, the transparent proxy, like the non-transparent proxy, acts as a communication partner.

If it is a `non-transparent proxy`, we must be informed about its existence. For this purpose, we and the software we want to use are given a special proxy configuration that ensures that traffic to the Internet is first addressed to the proxy. If this configuration does not exist, we cannot communicate via the proxy. However, since the proxy usually provides the only communication path to other networks, communication to the Internet is generally cut off without a corresponding proxy configuration.













