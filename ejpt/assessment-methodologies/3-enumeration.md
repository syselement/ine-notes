# Enumeration

> #### ⚡ Prerequisites
>
> * Basic familiarity with Linux and networks concepts
> * Footprinting and Scanning
>
> #### 📕 Learning Objectives
>
> * Purpose of service enumeration
> * Enumeration on common and uncommon services and protocols
>
> #### 🔬 Training list - PentesterAcademy Labs
>
> `subscription required`
>
> - [SMB Servers Win Recon](https://attackdefense.pentesteracademy.com/listing?labtype=windows-recon&subtype=windows-recon-smb)
> - [SMB Servers Network Recon](https://attackdefense.pentesteracademy.com/listing?labtype=network-recon&subtype=recon-smb)
> - [FTP Servers Linux Recon](https://attackdefense.pentesteracademy.com/listing?labtype=linux-security-recon&subtype=recon-ftp)
> - [SSH Servers Network Recon](https://attackdefense.pentesteracademy.com/listing?labtype=network-recon&subtype=recon-ssh)
> - [IIS Servers Win Recon](https://attackdefense.pentesteracademy.com/listing?labtype=windows-recon&subtype=windows-recon-iis)
> - [Webservers Network Recon](https://attackdefense.pentesteracademy.com/listing?labtype=network-recon&subtype=recon-webserver)
> - [SQL Databases Linux Recon](https://attackdefense.pentesteracademy.com/listing?labtype=linux-security-recon&subtype=linux-security-recon-sqldbs)
> - [SQL Databases Network Recon](https://attackdefense.pentesteracademy.com/listing?labtype=network-recon&subtype=recon-sqldb)
> - [MSSQL Servers Win Recon](https://attackdefense.pentesteracademy.com/listing?labtype=windows-recon&subtype=windows-recon-mssql)

A **server** is a computer program or device that provides a **service** to another computer program and its user, also known as the **client**. It accepts and responds to request made over a network. The term _server_ can refer to a physical machine, a virtual machine or to software performing server services.

It can run various operating systems like Windows Server, Linux Server, macOS Server.

Servers need to be accessed remotely by multiple clients, so the server must _open_ and accept connections on the listening _port_ of the service.

Open port service bugs and vulnerabilities could expose the entire server to attackers.

Common services are:

* `SMB`
* `FTP`
* `SSH`
* `HTTP`
* `MYSQL`

🗒️ **Enumeration** is a critical phase in a pentest, used to identify information about in-scope assets, discovering potential attack vectors and vulnerabilities.

![Common Ports - by Jeremy Stretch](.gitbook/assets/image-20230211104550784.png)

## SMB

**`SMB`** (**S**erver **M**essage **B**lock) - a network file and resource sharing protocol based on a _client-server_ model.

There are many variants of the SMB protocol like SMBv1, CIFS, SMBv2, SMBv2.1, SMBv3, and so on.

- `e.g.` Windows mapping and sharing drives as letter, uses SMB

Usually SMB can be found on ports `139` or `445` and `nmap` service and scripts enumeration (**`-sV`**, **`-sC`**) can find more info about the O.S. version.

After finding SMB through port scanning, gather more information with `nmap`.

```bash
sudo nmap -sV -sC -O <TARGET_IP>
```

>  🔬 [Windows Recon: SMB Nmap Scripts](https://attackdefense.pentesteracademy.com/challengedetailsnoauth?cid=2222)



## FTP

## SSH

## HTTP

## MYSQL
