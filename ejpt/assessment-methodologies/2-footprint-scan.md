# Footprinting & Scanning

> ### ⚡ Prerequisites
>
> * Basic familiarity with Linux
> * Basic networks concepts
>
> ### 📕 Learning Objectives
>
> * Purpose of network mapping and port scanning in relation to an engagement
> * Perform network host discovery and port scanning
> * Think and act like an adversary

> ❗***Never run these techniques on unauthorized addresses ❗A proper authorization is required to conduct the footprinting and scanning activity.***

## Mapping a Network

### Purpose

Before any type engangement the **purpose** of a pentest must be defined and negociated with the client, in order to mitigate risk and harden the client's system.

- The pentester must determine both the type of access to the client's network to begin the **`discovery`** and the **`scope`** of what will be valuable to the client, while not interfering with its business.

### Process

**Physical Access**

- physical security - access controls, camera, guards
- **`OSINT`** (**O**pen **S**ource **Int**elligence) - DNS records, websites, public IP addresses
- **`Social Engineering`** - *psychological manipulation of people into performing security mistakes or giving awas sensitive information*
- **`sniffing`** - (once connected) sniff network traffic through passive reconnaissance and packet capturing
  - collect IP address and MAC addresses for further enumeration
- [**`ARP`**](../penetration-testing-prerequisites/networking.md#arp) (**A**ddress **R**esolution **P**rotocol) - take advantage of the ARP table and broadcast communications
- **`ICMP`** (**I**nternet **C**ontrol **M**essage **P**rotocol) - `traceroute`, `ping`

## Tools

- [`wireshark`](#wireshark)
- [`arp-scan`](#arp-scan)
- [`ping`](#ping)
- [`fping`](#fping)
- [`nmap`](#nmap)
- [`zenmap`](#zenmap)

### [Wireshark](https://www.wireshark.org/)

Launch `wireshark` and start monitoring the internet network interface (`eth0` in this case).

Run an `arp-scan` on the same interface and check the traffic inside wireshark.

![arp-scan inside Wireshark](.gitbook/assets/image-20230210204911962.png)

![ARP packet](.gitbook/assets/image-20230210205141400.png)

### arp-scan

> **`ip`** - show/manipulate routing, network devices, interfaces and tunnels

```bash
ip -br -c a
# -br = brief
# -c  = color
```

![ip -br -c a](.gitbook/assets/image-20230210205600329.png)

> **`arp-scan`** - send ARP requests to target hosts and display responses

```bash
sudo arp-scan -I eth1 192.168.31.0/24
```

![arp-scan](.gitbook/assets/image-20230210205813009.png)

### ping

> **`ping`** - send ICMP ECHO_REQUEST to network hosts

```bash
ping 192.168.31.2
# Reachable

ping 192.168.31.5
# Unreachable
```

![ping](.gitbook/assets/image-20230210213222404.png)

### fping



### nmap



### zenmap

