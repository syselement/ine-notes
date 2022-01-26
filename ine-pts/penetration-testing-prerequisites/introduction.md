# Introduction

## The Information Security Field

Pentesting career, supported by:

- knowing the information security (**`infosec`**) field
- career opportunities
- talking with colleagues

### InfoSec Culture

Deep roots in the underground hacking scene.

**`Hacker`** = refers to people who prefer to understand how a system works, being curious, intelligent and motivated to pursue knowledge.

Usually a hacker approaches systems with curiosity, so he can:

- Find new ways to use computer systems
- Bypass imposed restrictions
- Understand security pitfalls

*Performing an attack* means to **understand** the technology and the functioning of the target system.

*Being a Hacker* means **improving** skills every day, pushed by **curiosity** and hunger for **knowledge**.

> 📕 There is always something new to learn! *Hacking* is a lifestyle.

*Being an InfoSec professional* means pursuing knowledge by keeping challenging yourself and your colleagues, being honest with yourself and never stop. 

- To have an idea about the ideals of the underground hacking community read [The Conscience of a Hacker](http://phrack.org/issues/7/3.html).

![](.gitbook/assets/TheConscienceOfAHacker_TheMentor_pic.jpg)

### Career Opportunities

Companies and government bodies are using advanced tech to store and process confidential data. Using hacking skills for good has become critical for the safety of nations too.

*Data is transmitted* across private & public networks. It is a **must** to:

- **Protect sensitive information**
- Implement hardware and software defensive systems
- Protect `digital assets` from major **cyber-threats** like:
  - global cyber syndicates
  - hackers for hire
  - hacktivists
  - terrorists
  - state-sponsored hackers

*Train the organization* to make sure:

- secure applications are developed
- proper defensive measures are taken
- proper use of the company's data is in place

*Hire a penetration tester* to ensure that a system is secure from cyber-attacks.

**`Penetration Testers`** (a.k.a. **`pentesters`**) = are professionals hired to simulate a hacking attack against a network, a computer system, a web application or the entire organization. They discover vulnerabilities across the tested systems by mastering the same tools and techniques used by malicious hackers. They often work:

- as freelancers
- in an IT Security services company
- as in-house employees

Pentesters can specialize in specific InfoSec sectors:

- Systems attacks
- Web Applications
- Malware Analysis
- Reverse Engineering
- Mobile Applications
- Network Pentesting
- Social Engineering
- Other...

> 📌 Be passionate, skilled and hungry for knowledge!

------

### Information Security Terms

It is fundamental to speak the InfoSec domain language.

------

**`WHITE HAT HACKER`**

- is a professional pentester or ***ethical hacker*** who performs *authorized* attacks against a system helping the client *solve* their security issues.
- they do NOT perform illegal actions.

------

**`BLACK HAT HACKER`**

- is a hacker who performs *unauthorized* attacks against a system with the purpose of *causing damage* or gaining *profit*.
- a category of black hat hackers is called "*crackers*".

------

**`USER`**

- is a computer system user (an employee of your client of an external user).

------

**`MALICIOUS USER`**

- is a user who *misuses* or *attacks* computer systems and applications.

------

**`ROOT / ADMINISTRATOR`**

- are the users who manage IT networks or single systems.
- they have *maximum privileges* over a system.

------

**`PRIVILEGES`**

- identify the action that a user is *allowed* to do.
- the higher the privileges, the more control.

------

**`SECURITY THROUGH OBSCURITY`**

- is the use of *secrecy of design*, implementation or configuration in order to provide security.
- it *cannot stop* a skilled and motivated attacker.

------

**`ATTACK`**

- is any kind of action aimed at *misusing* or *taking control* over a computer system or application.
  - unauthorized access to an administration area
  - stealing a user's credentials
  - causing denial of service
  - eavesdropping or communications

------

**`PRIVILEGE ESCALATION`**

- **`privesc`** is an attack where a malicious user *gains elevated privileges* over a system.

------

**`DENIAL OF SERVICE`**

- a **`DoS attack`** is used by an attacker to make a system or a service unavailable / unresponsive, causing a service crash or resources saturation.

------

**`REMOTE CODE EXECUTION`**

- during a **`RCE attack`** a malicious user executes some attacker-controlled *code* on a victim *remote* machine.
- RCE vulnerabilities can be *exploited* over the network by a remote attacker.

------

**`SHELL CODE`**

- is a piece of custom code which provides the attacker a **`shell`** on the victim machine, generally used during RCE attacks.

------

## Cryptography Protocols & VPNs

Pentesting career, supported by:

- knowing how info is transmitted over networks, by using the right protocol for the transmission
- protection of the traffic

### Clear-text Protocols

- They should **not** be used for the transmission of critical or private info, since it's easy to intercept.
- Use clear-text protocols only on trusted networks, if really necessary.

### Cryptographic Protocols

- They are used to protect the communication by *encrypting* the transmitted information, in case of eavesdropping.
- Always use a cryptographic protocol for usernames and passwords.

A Clear-text protocol information can be wrapped into a cryptographic protocol, like a *VPN tunnel*.

### Virtual Private Networks (VPN)

A **`VPN`** establish a secure, encrypted and protected connection between a private network and a public one (or the Internet), using a private tunnel for the data.

- The client is directly connected to the private network.

------

## Wireshark

- **`Wireshark`** is a network sniffer tool and packet analyzer, that allows to capture the data transmitted over the network.

### HTTP(s) Traffic Sniffing

0. Connect to the Lab VPN (INE in this case) by using OpenVpn and the **.ovpn** file provided. (in my case INE provided a direct Lab Link / Kali GUI instance opened in another tab).

   - From terminal, check if the machines are reachable:

     `ping demo.ine.local`

     `ping demossl.ine.local`

   - Check open ports with nmap tool:

     `nmap demo.ine.local`

     `nmap demossl.ine.local`

     Check the Kali Machine interface name:

     `ifconfig`

     ![](.gitbook/assets/image-20220126224915778.png)

1. Open Wireshark and start the capture on the Vpn network interface.

   - or use the terminal: `wireshark -i eth1 `

2. Generate traffic from the browser by browsing to the **HTTP** web page (`http://demo.ine.local`) and try a login.

3. The sniffer records the traffic between the browser and the server. Right click on a packet and `Follow TCP Stream` to see the traffic exchange.
   - In case of HTTP protocol, the clear-text traffic can be sniffed easily. The content of the packets is in human readable form.
   
     <img src=".gitbook/assets/image-20220126221608902.png" style="zoom: 80%;" />
   
4. Restart the capture to clean the results. Try the same login into the **HTTPS** web page (`https://demossl.ine.local`) and check the TCP Stream in the captured traffic.
   
   - Check the certificate with the lock icon.
   - In case of HTTPS protocol, the traffic is encrypted, unreadable and protected.
   - HTTPS (HTTP over TLS) protects the content
   
   <img src=".gitbook/assets/image-20220126222042593.png" style="zoom:80%;" />

5. Captured traffic can be filtered in Wireshark with *display filters*.

------

## Binary Arithmetic Basics

