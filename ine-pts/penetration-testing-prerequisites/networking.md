# Networking

## Protocols

Computers have to use a wide variety of networking **protocols**, to ensure that different types of hardware and software can communicate between each other.



### Packets

**`Packets`** carry the information exchanged in networking.

- Streams of bits, electric signals running on media such as a **wire** in a LAN network or **the air** in a wireless connection.
- Interpreted as bits (0,1).

A packet's structure consists of a **`header`** and a **`payload`**.

- The header have a protocol specific structure, ensuring that the payload is correctly interpreted.
- The payload is the data, the content, the body of a packet.

***IP** (Internet Protocol) header* (IPv4) is 160 bits (20 Bytes) long.

- First 4 bits - version of IP protocol.
- Source Address - 32 bits (4 Bytes), in position 96.
- Destination Address - 32bits (4 Bytes) after Source Address.

![](.gitbook/assets/image-20220203230835178.png)



### Protocol Layers

A protocol provides various features like:

- making an *application* work (emails, VoIP calls, FTP, browser)
- connecting a server and a client, *transporting* data between them
- identifying and internetworking computers/hosts on a *network*
- transmitting data over *physical* media

These features are structured as a protocol stack, in a series of layers.

- TCP/IP is a real world implementation of a networking stack.

| TCP/IP Protocol Stack |
| :-------------------: |
|      Application      |
|       Transport       |
|  Network / Internet   |
|  Physical Interface   |

   > 📕 Each layer has its own protocol and serves the one above it.



### ISO/OSI

The Open System Interconnection (**`OSI`**) is a logical and conceptual model published by the International Organization for Standardization (**`ISO`**) in 1984, used as reference.

| ISO/OSI Protocol Stack |
| :--------------------: |
|      Application       |
|      Presentation      |
|        Session         |
|       Transport        |
|        Network         |
|       Data Link        |
|        Physical        |

- TCP/IP vs ISO/OSI model:

![](.gitbook/assets/image-20220204000600475.png)



### Encapsulation

During the process of **`encapsulation`**, a lower-layer protocol places the entire upper protocol packet (header + payload) in its payload portion and adds its own header to the packet.

- Every packet sent by a host is encapsulated.
- The same operation is done in reverse order by the receiving host.

------

## Internet Protocols (IP)

...
