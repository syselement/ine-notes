# 📜eJPT Cheat Sheet

## [Networking](penetration-testing-prerequisites/networking.md)

```bash
# Routing
# Linux
ip route

# Windows
route print

# Mac OS X / Linux
netstat -r
```

```bash
# IP
# Linux
ip a
ip -br -c a

# Windows
ipconfig /all

# Mac OS X / Linux
ifconfig
```

```bash
# ARP
# Linux
ip neighbour

# Windows
arp -a

# Mac OS X / Linux
arp
```

```bash
# PORTS
# Linux
netstat -tunp
netstat -tulpn
ss -tnl

# Windows
netstat -ano

# Mac OS X / Linux
netstat -p tcp -p udp
lsof -n -i4TCP -i4UDP
```



## [Information Gathering](assessment-methodologies/1-info-gathering.md)

```bash
#
```

## [Footprinting & Scanning](assessment-methodologies/2-footprint-scan.md)

```bash
#
```



## [Enumeration](assessment-methodologies/3-enumeration.md)

### SMB

```bash
#
```

### FTP

```bash
#
```

### SSH

```bash
#
```

### HTTP

```bash
#
```

### SQL

```bash
#
```



## [Vulnerability Assessment](assessment-methodologies/4-va.md)

```bash
#
```





## [Host Based Attacks](hostnetwork-penetration-testing/1-system-attack.md)

### Windows Exploitation

```bash
#
```

### Windows Privilege Escalation

```bash
#
```

### Windows Credential Dumping

```bash
#
```

### Linux Exploitation

```bash
#
```

### Linux Privilege Escalation

```bash
#
```

### Linux Credential Dumping

```bash
#
```

## [Network Based Attacks](hostnetwork-penetration-testing/2-network-attack.md)

```bash
#
```

## [Metasploit](hostnetwork-penetration-testing/3-metasploit.md)

```bash
service postgresql start && msfconsole -q
```

### Info Gathering

```bash
#
```

### Enumeration

```bash
#
```

### Vulnerability Scanning

```bash
#
```

### Payloads

```bash
#
```

### Win Exploitation

```bash
#
```

### Linux Exploitation

```bash
#
```

### Win Post-Exploitation

```bash
#
```

### Linux Post-Exploitation

```bash
#
```

### Armitage

```bash
#
```

## [Exploitation](hostnetwork-penetration-testing/4-exploitation.md)

### Vulnerability Scanning

```bash
#
```

### Exploits

```bash
#
```

### Shells

```bash
#
```

### Frameworks

```bash
#
```

### Win Exploitation

```bash
#
```

### Linux Exploitation

```bash
#
```

### Obfuscation

```bash
#
```

## [Post-Exploitation](hostnetwork-penetration-testing/5-post-exploit.md)

### Win Local Enumeration

```bash
#
```

### Linux Local Enumeration

```bash
#
```

### Transferring Files

```bash
#
```

### Shells

```bash
#
```

### Win Privilege Escalation

```bash
#
```

### Linux Privilege Escalation

```bash
#
```

### Win Persistence

```bash
#
```

### Linux Persistence

```bash
#
```

### Dumping & Cracking

```bash
#
```

### Pivoting

```bash
#
```

### Clearing

```bash
#
```

## [Social Engineering](hostnetwork-penetration-testing/6-social-engineer.md)

```bash
#
```



## [Web Application Penetration Testing](webapp-penetration-testing/1-webapp-http.md)

```bash
#
```
