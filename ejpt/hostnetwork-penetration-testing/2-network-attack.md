# Network Based Attacks

> #### ⚡ Prerequisites
>
> * Basic Cybersecurity & Network Concepts
>
> #### 📕 Learning Objectives
>
> * Describe service related **Network Attacks**
> * Identify network traffic and perform packet analysis
> * Perform **MITM (Man in the Middle)** attacks
>
> #### 🔬 Training list - PentesterAcademy/INE Labs
>
> `subscription required`
>
> - [Tshark Traffic Analysis](https://attackdefense.com/listing?labtype=traffic-analysis&subtype=tshark-basics)
>   - [Traffic Analysis: TSHARK Unleashed - Course](https://www.pentesteracademy.com/course?id=42)
> - [WIFI Traffic Analysis](https://attackdefense.com/listing?labtype=wifi-security-basics&subtype=wifi-security-basics-traffic-analysis)
>   - [Wi-Fi Security and Pentesting - Course](https://www.pentesteracademy.com/course?id=9)

🗒️ **Network based attacks** are attacks targeted towards specific network traffic and services.

- ARP
- DHCP
- SMB
- FTP
- Telnet
- SSH

🗒️ **MITM** (**M**an **I**n **T**he **M**iddle) is a type of cybersecurity attack that *allows the attacker to eavesdrop/listen on the legitimate communication between two targets*.

![https://www.veracode.com/security/man-middle-attack](.gitbook/assets/veracode-appsec_man-middle-attack.png)

`e.g.`

- [**ARP Poisoning**](https://medium.com/geekculture/understanding-arp-poisoning-mitm-attack-7b12a3b061bd) - intercept communication through broadcasting ARP packets and waiting for answers from other machines.
- **Promiscuous mode** - listen to all the traffic on a network

## Tools

### [Wireshark](https://www.kali.org/tools/wireshark/)

> 🔬 Check some `Wireshark` traffic sniffing in [this lab](../exam-preparation-labs/p.t.-prerequisites-labs/http-s-traffic-sniffing.md)

- `e.g.` Capture a `nmap` scan traffic with  **`Wireshark`**
  - Check the interface before beginning the capture
  - Protocol Hierarchy Statistics
  - Identify all layers looking at captured packets
  - Default time display setting is *relative time in seconds*
  - Follow > TCP Stream to check an entire stream

![Wireshark ARP traffic](.gitbook/assets/image-20230323145133991.png)

![Wireshark Protocol Hierarchy Statistics](.gitbook/assets/image-20230324133345868.png)

### [Tshark](https://tshark.dev/)

> [**tshark**](https://tshark.dev/) - a network protocol analyzer that captures packet data from a live network or from a saved capture file. It is the CLI component of `Wireshark`.
>
> - Native capture file format is `pcapng ` (Wireshark compatible)

![tshark --help](.gitbook/assets/image-20230324110608951.png)

### arpspoof

> [**`arpspoof`**](https://www.kali.org/tools/dsniff/#arpspoof) - a (old) tool to send out unrequested (and possibly forged) `ARP` replies and intercept packets on a switched LAN

![image-20230324130125228](.gitbook/assets/image-20230324130125228.png)



> 🔬 Check some hands-on labs in the [Tshark, ARP, WiFi Lab section](2-network-attack/tshark-arp.md)
>

------

