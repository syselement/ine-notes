# The Metasploit Framework (MSF)

> #### ⚡ Prerequisites
>
> * Basic familiarity with Linux & Windows
> * Basic familiarity with TCP & UDP protocols
>
> #### 📕 Learning Objectives
>
> * Understand, install, configure and use **Metasploit Framework**
> * Perform **info-gathering**, **enumeration**, **exploitation**, **post exploitation** with Metasploit
> 
> #### 🔬 Training list - PentesterAcademy/INE Labs
>
> `subscription required`
>
> - [Metasploit Auxiliary modules](https://www.attackdefense.com/listingnoauth?labtype=metasploit&subtype=metasploit-auxiliary)
>- [MITRE ATT&CK Linux Discovery](https://attackdefense.com/listing?labtype=mitre&subtype=mitre-discovery)
> - [Metasploit Meterpreter](https://attackdefense.com/listing?labtype=metasploit&subtype=metasploit-meterpreter)
> - [Linux Vulnerable Servers Exploitation - Metasploit](https://www.attackdefense.com/listingnoauth?labtype=metasploit&subtype=metasploit-linux-exploitation)
> - [Linux Exploitation - Metasploit](https://www.attackdefense.com/listingnoauth?labtype=linux-security-exploitation&subtype=linux-security-exploitation-metasploit)
> - [Linux Post Modules - Metasploit](https://www.attackdefense.com/listingnoauth?labtype=linux-security-post-exploitation&subtype=linux-security-post-exploitation-metasploit)
> - [Win Basic Exploitation - Metasploit](https://www.attackdefense.com/listingnoauth?labtype=windows-exploitation&subtype=windows-exploitation-basics)
> - [Win Pentesting Basic Exploitation](https://attackdefense.com/listing?labtype=windows-exploitation&subtype=windows-exploitation-pentesting)
> - [Win Apps Exploits - Metasploit](https://www.attackdefense.com/listingnoauth?labtype=metasploit&subtype=metasploit-windows-apps-exploits)
> - [Win Post Exploitation - Metasploit](https://attackdefense.com/listing?labtype=windows-post-exploitation&subtype=windows-post-exploitation-metasploit)
> - [Win Maintaining Access](https://attackdefense.com/listing?labtype=windows-maintaining-access&subtype=windows-maintaining-access-basics)

## MSF Introduction

🗒️ The [**Metasploit Framework**](https://www.metasploit.com/) (**MSF**) is an open-source pentesting and exploit development platform, used to write, test and execute exploit code.

- Provides automation of the penetration testing life cycle (specially exploitation and post-exploitation)
- Used to develop and test exploits
- Has a world database and public tested exploits 
- It is modular, new modules can be added and integrated
- [Pre-installed in Kali Linux](https://www.kali.org/tools/metasploit-framework/)
- It is [open-source](https://github.com/rapid7/metasploit-framework)
- Founded by H.D. Moore in 2003 (developed in Perl), Written in Ruby in 2007, acquired by [Rapid7](https://www.rapid7.com/) in 2009, released as Metasploit v6.0 in 2020
- **Metasploit Framework** is the Community Edition
- Metasploit Pro & Express are Commercial versions

### Terminology

| Term              | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| **Interface**     | Methods of interacting with the Metasploit Framework (`msfconsole`, Metasploit cmd) |
| **Module**        | Pieces of code that perform a particular task (an exploit)   |
| **Vulnerability** | Exploitable flaw or weakness in a computer system or network |
| **Exploit**       | Code/Module used to take advantage of a vulnerability        |
| **Payload**       | Piece of code delivered to the target by an exploit (execute arbitrary commands or provide remote access) |
| **Listener**      | Utility that listens for an incoming connection from a target |

> 📌 **Exploit** is launched (takes advantage of the vulnerability) ➡️ **Payload** dropped (executes a reverse shell command) ➡️ Connects back to the **Listener**

### Interfaces

🗒️ **Metasploit Framework Console** (**MSFconsole**) - an all in one interface that provides with access to all the functionality of the MSF.

![msfconsole](.gitbook/assets/image-20230409121754103.png)

🗒️ **Metasploit Framework Command Line Interface** (**MSFcli**) - a command line utility used to facilitate the creation of automation scripts that utilize Metasploit modules.

- Discontinued in 2015, MSFconsole can be used with the same functionality of *redirecting output from other tools into `msfcli` and vice versa.*

🗒️ **Metasploit Community Edition GUI** - a web based GUI front-end of the MSF.

🗒️ [**Armitage**](https://www.kali.org/tools/armitage/) - a free Java based GUI front-end cyber attack management tool for the MSF.

- Visualizes targets and simplifies network discovery
- Recommends exploits
- Exposes the advanced capabilities of the MSF

### [Architecture](https://www.offsec.com/metasploit-unleashed/metasploit-architecture/)

![Metasploit Framework Architecture - oreilly.com](.gitbook/assets/16c68136-bdbb-4846-be83-4e93822ee0de.png)

🗒️ A **module** is the piece of code that can be utilized and executed by the MSF.

The MSF **libraries** (Rex, Core, Base) allow to extend and initiate functionality, facilitating the execution of modules without having to write additional code.

### [Modules](https://www.offsec.com/metasploit-unleashed/modules-and-locations/)

| MSF Module    | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| **Exploit**   | Used to take advantage of a vulnerability, usually paired with a payload |
| **Payload**   | Code delivered and remotely executed on the target *after successful exploitation* - **e.g.** a reverse shell that initiates a connection |
| **Encoder**   | Used to encode payloads in order to avoid Anti Virus detection - **e.g.** [shikata_ga_nai](https://www.mandiant.com/resources/blog/shikata-ga-nai-encoder-still-going-strong) encoding scheme |
| **NOPS**      | Keep the payload sizes consistent across exploit attempts and ensure the *stability of a payload* on the target system |
| **Auxiliary** | Is not paired with a payload, used to perform additional functionality - **e.g.** port scanners, fuzzers, sniffers, etc |

### [Payload Types](https://www.offsec.com/metasploit-unleashed/payloads/)

**Payloads** are created at runtime from various components. Depending on the target system and infrastructure, there are two types of payloads that can be used:

- **Non-Staged Payload** - sent to the target system as is, along with the exploit
- **Staged Payload** - sent to the target in two parts:
  - the **stager** (first part) establish a stable communication channel between the attacker and target. It contains a payload, the stage, that initiates a reverse connection back to the attacker
  - the **stage** (second part) is downloaded by the stager and executed
    - executes arbitrary commands on the target
    - provides a reverse shell or Meterpreter session

🗒️ The [**Meterpreter**](https://www.offsec.com/metasploit-unleashed/about-meterpreter/) is an advanced multi-functional payload executed by in memory DLL injection stagers on the target system.

- Communicates over the stager socket
- Provides an interactive command interpreter on the target system

### [File System](https://www.offsec.com/metasploit-unleashed/filesystem-and-libraries/)

```bash
ls /usr/share/metasploit-framework
```

![ls /usr/share/metasploit-framework](.gitbook/assets/image-20230409193231022.png)

- MSF filesystem is intuitive and organized by directories.
- Modules are stored under:
  - `/usr/share/metasploit-framework/modules/`
  - `~/.msf4/modules` - user specified modules

![](.gitbook/assets/image-20230409193537174.png)

![](.gitbook/assets/image-20230409194316207.png)

![](.gitbook/assets/image-20230409194335838.png)

****

### Pentesting with MSF



## Metasploit Fundamentals

- Installing



## Information Gathering & Enumeration with MSF





## Vulnerability Scanning With MSF





## Client-Side Attacks with MSF





## Exploitation with MSF





## Post Exploitation with MSF





## Armitage - MSF GUI

### Kali Linux Installation

```bash
sudo apt install armitage -y
```

```bash
sudo msfdb init
```

```bash
sudo nano /etc/postgresql/15/main/pg_hba.conf
# On line 87 switch “scram-sha-256” to “trust”
```

```bash
sudo systemctl enable postgresql
sudo systemctl restart postgresql
```

```bash
sudo armitage
```

