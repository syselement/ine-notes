# Introduction

## The Information Security Field

> ⚡ Penetration Testing Usage: 
>
> - Knowing the information security (**`infosec`**) field
> - Career opportunities
> - Talking with colleagues

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

> 📌Be passionate, skilled and hungry for knowledge!

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

> ⚡ P.T. Usage: 
>
> - Knowing how info is transmitted over networks, by using the right protocol for the transmission
> - Traffic protection

### Clear-text Protocols

- They should **not** be used for the transmission of critical or private info, since it's easy to intercept.
- Use clear-text protocols only on trusted networks, if really necessary.

### Cryptographic Protocols

- They are used to protect the communication by *encrypting* the transmitted information, in case of eavesdropping.
- Always use a **cryptographic** protocol for usernames and passwords.

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

> ⚡ P.T. Usage: 
>
> - Computer/Boolean logic, data is represented in binary format
> - Network addressing

### Binary and Decimal Bases

**`Binary`** notation uses only two symbols to represent numbers, **0** (**zero**) and **1** (**one**).

**`Decimal`** notation uses ten symbols (0 to 9).

- *Counting* in binary: start counting from 0, when reach 1, you increment the digit to the left of it.
  - 0, 1, 10, 11, 100, 101, 110, 111, 1000 ... and so on.

- *Converting* from **bin**ary to **dec**imal: by using the position of the digits.
  $$ {From binary 11011 to decimal}
  11011(_2) = 1*2^0+1*2^1+0*2^2+1*2^3+1*2^4=27_{(10)}
  $$

- *Converting* from **dec**imal to **bin**ary:
  
  - by dividing the decimal number by 2 (base 2), write the remainder, continue like this until reaching 0 as dividend.
  
  $$ {From decimal 26 to binary}
  26/2=13
  \\remainder\ is\ 0
  \\
  13/2=6
  \\remainder\ is\ 1
  \\
  6/2=3
  \\remainder\ is\ 0
  \\
  3/2=1
  \\remainder\ is\ 1
  \\
  1/2=0
  \\remainder\ is\ 1
  \\
  $$
  $$
  26_{(10)}=11010(_2)
  $$
  
  
  
  - Or use the powers of two in a "base 2 table":
  
  $$
  {Powers\ of\ 2}
  \\
  2^0=1
  \\
  2^1=2
  \\
  2^2=4
  \\
  2^3=8
  \\
  2^4=16
  \\
  2^5=32
  \\
  2^6=64
  \\
  2^7=128
  \\
  2^8=256
  \\
  2^9=512
  \\
  2^{10}=1024
  \\
  2^{11}=2048
  \\
  2^{12}=4096
  \\
  2^{13}=8192
  \\
  2^{14}=16384
  \\
  2^{15}=32768
  $$
  
  $$
  {Example\ }
  \\
  2^4+2^3+2^1=16+8+2=26
  \\
  2^4\ YES\ \dashrightarrow 1
  \\
  2^3\ YES\ \dashrightarrow 1
  \\
  2^2\ NO\ \dashrightarrow 0
  \\
  2^1\ YES\ \dashrightarrow 1
  \\
  2^0\ NO\ \dashrightarrow 0
  \\
  26_{(10)}=11010(_2)
  $$
  
  

### Bitwise operations

- **`NOT`** flips the bits - zero to one, and one to zero.
  $$
  NOT(11011)=00100
  $$
  
- **`AND`** logical AND (**&**) between the bits of the operands.
  $$
  {If\ both\ bits\ are\ =1, result\ is\ 1.}
  \\
  0\ \&\ 0\ = 0
  \\
  0\ \&\ 1\ = 0
  \\
  1\ \&\ 0\ = 0
  \\
  1\ \&\ 1\ = 1
  $$
  
  $$
  1010\ \&\ 1101\ = 1000
  $$
  
- **`OR`** logical OR (**|**) between the bits of the operands.
  $$
  {If\ at\ least\ one\ bit\ is\ =1, result\ is\ 1.}
  \\
  0\ |\ 0\ = 0
  \\
  0\ |\ 1\ = 1
  \\
  1\ |\ 0\ = 1
  \\
  1\ |\ 1\ = 1
  $$
  
  $$
  1010\ |\ 1100\ = 1110
  $$
  
- **`XOR`** logical Exclusive OR (**^** , **⊕**) between the bits of the operands.
  $$
  {If\ just\ one\ bit\ is\ =1, result\ is\ 1.}
  \\
  0 \oplus 0\ = 0
  \\
  0 \oplus  1\ = 1
  \\
  1 \oplus  0\ = 1
  \\
  1 \oplus  1\ = 0
  $$
  
  $$
  1010 \oplus 1101\ = 0111 = 111
  $$

Windows calculator can help with calculations in "Programmer" mode.

<img src=".gitbook/assets/image-20220131231201595.png" style="zoom: 67%;" />

### Hexadecimal arithmetic

**`Hexadecimal`** system is used too in computer science. It uses 16 symbols (with letters for double-digit numbers):

- `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F`

To distinguish a hexadecimal number from a decimal number, "**0x**" at the beginning or "**h**" at the end is added.

- *Converting* from **hex**adecimal to **dec**imal: by using the position of the digits.
  $$
  0x4a1 = 0x(4\ 10\ 1)
  \\
  0x4a1 = 1*16^0 + 10*16^1 + 4*16^2 = 1 + 160 + 1024 = 1185_{(10)}
  $$

- *Converting* from **dec**imal to **hex**adecimal:

  - by subsequently dividing the decimal number by 16 (base 16), write the remainder, continue like this until reaching 0.
  $$ {From decimal 1185 to hexadecimal}
  1185/16=74,0625
  \\remainder\ is\ 0,0625
  \\
  74/16=4,625
  \\remainder\ is\ 0,625
  \\
  4/16=0,25
  \\remainder\ is\ 0,25
  \\
  0/16=can't\ divide\ 0.
  $$
  
  $$
  0.25*16=4
  \\
  0.625*16=10\ (a)
  \\
  0.0625*16=1
  $$
  
  $$
  1185_{(10)}=4a1_{(16)}=0x4a1
  $$

> 📌Online converters can help to speed up the calculations, for example here you can find some conversion tools [Binary Hex Converters](https://www.binaryhexconverter.com/).

------

