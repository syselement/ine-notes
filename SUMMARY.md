# Table of contents

* [INE Training Notes](README.md)

## Courses

* [INE - PTSv2](ejpt/README.md)
  * [📒Penetration Testing Prerequisites](ejpt/penetration-testing-prerequisites/README.md)
    * [Introduction](ejpt/penetration-testing-prerequisites/introduction.md)
    * [Networking](ejpt/penetration-testing-prerequisites/networking.md)
    * [Web Applications](ejpt/penetration-testing-prerequisites/web-applications.md)
  * [📒1. Assessment Methodologies & Auditing](ejpt/assessment-methodologies/README.md)
    * [Information Gathering](ejpt/assessment-methodologies/1-info-gathering.md)
    * [Footprinting & Scanning](ejpt/assessment-methodologies/2-footprint-scan.md)
    * [Enumeration](ejpt/assessment-methodologies/3-enumeration.md)
      * [🔬SMB Enum](ejpt/assessment-methodologies/3-enumeration/smb-enum.md)
      * [🔬FTP Enum](ejpt/assessment-methodologies/3-enumeration/ftp-enum.md)
      * [🔬SSH Enum](ejpt/assessment-methodologies/3-enumeration/ssh-enum.md)
      * [🔬HTTP Enum](ejpt/assessment-methodologies/3-enumeration/http-enum.md)
      * [🔬MYSQL Enum](ejpt/assessment-methodologies/3-enumeration/mysql-enum.md)
      * [🔬SMTP Enum](ejpt/assessment-methodologies/3-enumeration/smtp-enum.md)
    * [Vulnerability Assessment](ejpt/assessment-methodologies/4-va.md)
    * [Auditing Fundamentals](ejpt/assessment-methodologies/5-audit.md)
  * [📒2. Host & Network Penetration Testing](ejpt/hostnetwork-penetration-testing/README.md)
    * [System/Host Based Attacks](ejpt/hostnetwork-penetration-testing/1-system-attack.md)
      * [🪟 Windows Attacks](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks.md)
        * [🔬IIS - WebDAV](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/iis-webdav.md)
        * [🔬SMB - PsExec](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/smb-psexec.md)
        * [🔬RDP](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/rdp.md)
        * [🔬WinRM](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/winrm.md)
        * [🔬Win Kernel Privesc](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/winkernel.md)
        * [🔬UAC Bypass](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/uacme.md)
        * [🔬Access Token](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/accesstoken.md)
        * [🔬Alternate Data Stream](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/alternateds.md)
        * [🔬Credentials Dumping](ejpt/hostnetwork-penetration-testing/1-system-attack/windows-attacks/creds-dump.md)
      * [🐧 Linux Attacks](ejpt/hostnetwork-penetration-testing/1-system-attack/linux-attacks.md)
        * [🔬Bash](ejpt/hostnetwork-penetration-testing/1-system-attack/linux-attacks/bash-shell.md)
        * [🔬FTP](ejpt/hostnetwork-penetration-testing/1-system-attack/linux-attacks/ftp-unix.md)
        * [🔬SSH](ejpt/hostnetwork-penetration-testing/1-system-attack/linux-attacks/ssh-unix.md)
        * [🔬SAMBA](ejpt/hostnetwork-penetration-testing/1-system-attack/linux-attacks/samba-unix.md)
        * [🔬Cron Jobs](ejpt/hostnetwork-penetration-testing/1-system-attack/linux-attacks/cron.md)
        * [🔬SUID](ejpt/hostnetwork-penetration-testing/1-system-attack/linux-attacks/suid.md)
        * [🔬Hashes Dumping](ejpt/hostnetwork-penetration-testing/1-system-attack/linux-attacks/creds-dump-unix.md)
    * [Network Based Attacks](ejpt/hostnetwork-penetration-testing/2-network-attack.md)
      * [🔬Tshark, ARP, WiFi](ejpt/hostnetwork-penetration-testing/2-network-attack/tshark-arp.md)
    * [The Metasploit Framework (MSF)](ejpt/hostnetwork-penetration-testing/3-metasploit.md)
      * [🔬HFS - MSF Exploit](ejpt/hostnetwork-penetration-testing/3-metasploit/hfs-msf-exp.md)
      * [🔬Tomcat - MSF Exploit](ejpt/hostnetwork-penetration-testing/3-metasploit/tomcat-msf-exp.md)
      * [🔬FTP - MSF Exploit](ejpt/hostnetwork-penetration-testing/3-metasploit/ftpd-msf-exp.md)
      * [🔬Samba - MSF Exploit](ejpt/hostnetwork-penetration-testing/3-metasploit/samba-msf-exp.md)
      * [🔬SSH - MSF Exploit](ejpt/hostnetwork-penetration-testing/3-metasploit/ssh-msf-exp.md)
      * [🔬SMTP - MSF Exploit](ejpt/hostnetwork-penetration-testing/3-metasploit/smtp-msf-exp.md)
      * [🔬Meterpreter - MSF](ejpt/hostnetwork-penetration-testing/3-metasploit/meterpreter-msf.md)
      * [🔬Win Post Exploitation - MSF](ejpt/hostnetwork-penetration-testing/3-metasploit/win-post-msf.md)
      * [🔬Linux Post Exploitation - MSF](ejpt/hostnetwork-penetration-testing/3-metasploit/linux-post-msf.md)
    * [Exploitation](ejpt/hostnetwork-penetration-testing/4-exploitation.md)
    * [Post-Exploitation](ejpt/hostnetwork-penetration-testing/5-post-exploit.md)
    * [Social Engineering](ejpt/hostnetwork-penetration-testing/6-social-engineer.md)
  * [📒3. Web Application Penetration Testing](ejpt/webapp-penetration-testing/README.md)
    * [Introduction to the Web and HTTP Protocol](ejpt/webapp-penetration-testing/1-webapp-http.md)
  * [🔬Exam Preparation - Labs](ejpt/exam-preparation-labs/README.md)
    * [PTSv1 Prerequisites Labs](ejpt/exam-preparation-labs/p.t.-prerequisites-labs/README.md)
      * [🔬HTTP(S) Traffic Sniffing](ejpt/exam-preparation-labs/p.t.-prerequisites-labs/http-s-traffic-sniffing.md)
      * [🔬Find the Secret Server](ejpt/exam-preparation-labs/p.t.-prerequisites-labs/find-the-secret-server.md)
      * [🔬Data Exfiltration](ejpt/exam-preparation-labs/p.t.-prerequisites-labs/data-exfiltration.md)
      * [🔬Burp Suite Basics - Directory Enumeration](ejpt/exam-preparation-labs/p.t.-prerequisites-labs/burp-suite-basics.md)
  * [🌐References](ejpt/references.md)
  
  <!---
  
  * [📜eJPT Cheat Sheet](ejpt/ejpt-cheatsheet.md)
  
  --->


***

- [🏠 syselement's Blog Home](https://blog.syselement.com/home)

