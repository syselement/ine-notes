# Information Gathering

> ## ⚡ Prerequisites
>
> - Basic familiarity with Linux
> - Basic familiarity with web technologies
>
> ## 📕 Learning Objectives
>
> - Differences between **active** and **passive** information gathering
> - Perform passive and active information gathering with various tools and resources
>

**Information gathering** (***Reconnaissance***) is the initial stage of any penetration test and one of the most important phase.

- It involves finding out as much information as possible about a targeted individual, website, company or system.
- The more information a pentester has on a **target**, the more successful and easier the latter stages of a pentest will be. It depends on the **scope** of the penetration test too.
- `E.g.1` - Pentest on a Website: web technology, vulnerabilities, IP address of the hosting server.
- `E.g.2` - Pentest on a public facing assets and some internal systems, there can be more attack vectors:
  - gain access to the internal network through the public facing web server (one **access vector**)
  - during the info-gathering phase, learn more about the company employees (names, email addresses, credentials), getting this important information (useful for exploitation or initial access) by using phishing attacks, malicious attachments via email (another **access vector**)


## Passive Information Gathering Introduction

[**Passive information gathering**](#passive-information-gathering) involves *obtaining as much data as possible without actively interacting with the target*. 

- The pentester uses what's available on the Internet.
- `E.g.` - Website: utilizing publicly accessible information and resources of that particular website, through the browser, public IP address of the webserver hosting that website, etc.

### What passive information?

- IP addresses, DNS, domain names and domain ownership
- Email addresses, social media profiles
- Web technologies, subdomains

## Active Information Gathering Introduction

[**Active information gathering**](#active-information-gathering) involves *obtaining as much information as possible by actively engaging with the target*.

- ❗***An authorization is required to conduct active information gathering.***
- The target will be aware of the attacker's engagement.
- `E.g.` - Website: perform a port scan of the webserver IP address (found with passive info gathering) using **`nmap`** tool to identify the open ports and running services. Identify exploitable vulnerabilities on those services and consequently access the web server.

### What active information?

- Open ports, internal network/organization infrastructure
- Enumeration target info

### Code of conduct

> 📌 From the [The Pentester's Code of Conduct - by Sherri Davidoff](https://www.lmgsecurity.com/the-pentesters-code-of-conduct-rules-that-keep-everyone-safe/)
>
> - **Know your scope.**
> - **Do not exceed your scope.**
> - **Take responsibility.**
> - **Only hack when under signed contract.**
> - **Verify your targets** well in advance of the start of an engagement, and have the list in writing.
> - **Do a thorough and complete job.**
> - **Take careful notes.**
> - **Upload your evidence to a central repository as** soon as you can.
> - **Know your client.**
> - **Communicate** with your teammates, your client, and your project managers.
> - **Know your limitations and do not exceed them.**
> - **Treat all others with respect.**
> - **Own your mistakes.**
> - **Include your best suggestions for a solution when reporting a problem.**
> - **Google first, then ask questions.**
> - **Share your knowledge.**
> - **Above all, exercise common sense.**
>
> 🚩🔬 [zonetransfer.me](https://digi.ninja/projects/zonetransferme.php) domain will be utilized for training purposes and examples.

------

## Passive Information Gathering

### Website Reconnaissance & Footprinting

**Footprinting** is like reconnaissance, with more important information about a particular target.

| What to look for in a Website? |
| :----------------------------- |
| IP addresses of the web server |
| Hidden directories             |
| Names, Email addresses         |
| Phone numbers                  |
| Physical Addresses             |
| Web technologies               |

**`E.g.`** - Passive Reconnaissance on [hackersploit.org](https://hackersploit.org/):

- **`host`** command

```bash
host hackersploit.org
    hackersploit.org has address 188.114.97.7
    hackersploit.org has address 188.114.96.7
    hackersploit.org has IPv6 address 2a06:98c1:3121::7
    hackersploit.org has IPv6 address 2a06:98c1:3120::7
    hackersploit.org mail is handled by 0 _dc-mx.2c2a3526b376.hackersploit.org.
```

- 2 IP addresses found - the website is behind Cloudflare proxy.
  - Check the [DNSLytics Report](https://dnslytics.com/domain/hackersploit.org) too.
- Social Links at the bottom of the main page:

![](.gitbook/assets/image-20221115212013574.png)

- **`robots.txt`** file - [https://hackersploit.org/robots.txt](https://hackersploit.org/robots.txt)
  - Avoid having the site indexed by search engines by using the "Disallow" feature, which lets the site owner designate which file or folder not to index.
  - **`/wp-content`** indicates that the website is running Wordpress

![hackersploit.org/robots.txt](.gitbook/assets/image-20221115212251374.png)

- **`sitemap.xml`** file - [https://hackersploit.org/sitemap.xml](https://hackersploit.org/sitemap.xml)
  - Used to provide search engines with an organized way of indexing the website.
  - List of site pages, categories, author, etc

![hackersploit.org/sitemap.xml](.gitbook/assets/image-20221115215549993.png)

- Broswer add-ons for **Web Technology footprinting**:
  - [Wappalyzer](https://www.wappalyzer.com/) - find out the technology stack of the website

![Wappalyzer Example](.gitbook/assets/image-20221115215931204.png)

- **`whatweb`** command

![whatweb hackersploit.org](.gitbook/assets/image-20221115220428770.png)

- Download the entire website, for analyzing the source code for example:
  - [HTTrack](https://www.httrack.com/)

```bash
sudo apt install httrack
# Open from start menu "WebHTTrack Website Copier", opening up the web instance
```

![HTTrack Website copier](.gitbook/assets/image-20221115220852909.png)

### Whois Enumeration

**[Whois](https://g.co/kgs/YizQVp)** lookups are used to identify information regarding a particular domain.

- Date of registration, Owner, Registrar, Owner Email address, etc
- **`WHOIS`** *is a query and response protocol that is widely used for querying databases that store the registered users or assignees of an Internet resource, such as a domain name, an IP address block or an autonomous system, but is also used for a wider range of other information.* - [Whois - Wikipedia](https://en.wikipedia.org/wiki/WHOIS)

- **`whois`** command

![whois hackersploit.org](.gitbook/assets/image-20221115222123640.png)

![whois 172.64.32.93](.gitbook/assets/image-20221115223033163.png)

- [**who.is**](https://who.is/whois/hackersploit.org) site
- [**domaintools.com**](https://whois.domaintools.com/hackersploit.org) site

![domaintools.com example](.gitbook/assets/image-20221115222704909.png)

### Website Footprinting with Netcraft

**[Netcraft](https://www.netcraft.com/internet-data-mining/)** *provides internet security services for a large number of use cases, including cybercrime detection and disruption, application testing and PCI scanning.*

- It collates previous information identified with other tools and outputs an easy to read format.

**`E.g.`** - [Netcraft - Hackersploit.org](https://sitereport.netcraft.com/?url=https://hackersploit.org) - check the information needed for the pentest:

- Background
- Network: domain IP address, Nameserver, Domain registrar, IP delegation
- SSL/TLS Certificate: Issuer, Validity, Transparency, vulnerabilities
- Hosting History
- Web Trackers
- Site Technology: Server-Side, Client-Side, Frameworks, etc

![sitereport.netcraft.com example](.gitbook/assets/image-20221115224036510.png)

### DNS Reconnaissance

**DNS Recon** is used to *identify DNS records associated to a domain*, like A record, IP address, mail server IP.

- [**`dnsrecon`**](https://github.com/darkoperator/dnsrecon) tool - a Python script that provides the ability to perform NS/DNS Records Enumeration, records lookup, subdomain brute force, etc.

![dnsrecon --help](.gitbook/assets/image-20221126162221715.png)

```bash
dnsrecon -d hackersploit.org
# It responds with the NameServer addresses (NS)
# A record - IPv4 address of the website
# AAAA record - IPv6 addresses
# MX record - mail server address
# TXT record - domain/site verification or other values (SPF ...)
```

![dnsrecon -d hackersploit.org](.gitbook/assets/image-20221126162431648.png)

![dnsrecon -d zonetransfer.me](.gitbook/assets/image-20221126162754153.png)

- [**dnsdumpster.com**](https://dnsdumpster.com/) site
  - discover hosts related to a domain
  - map the domain in a graph `.png` image or `.xlsx` file.

![dnsdumpster.com example](.gitbook/assets/image-20221126170656955.png)

![dnsdumpster.com export](.gitbook/assets/image-20221126171001737.png)

![zonetransfer.me Domain map - from dnsdumpster.com](.gitbook/assets/image-20230118233326703.png)

### WAF

**W**eb **A**pplication **F**irewall (**`WAF`**) detection with [**`wafw00f`**](https://github.com/EnableSecurity/wafw00f). It does the following:

- *Sends a normal HTTP request and analyses the response; this identifies a number of WAF solutions.**
- **If that is not successful, it sends a number of (potentially malicious) HTTP requests and uses simple logic to deduce which WAF it is.*
- If that is also not successful, it analyses the responses previously returned and uses another simple algorithm to guess if a WAF or security solution is actively responding to our attacks.*

![wafw00f -h](.gitbook/assets/image-20221126175203304.png)

```bash
wafw00f -l
# List all WAFs that it is able to detect
```

![wafw00f -l](.gitbook/assets/image-20221126175346664.png)

![wafw00f hackersploit.org](.gitbook/assets/image-20221126175427482.png)

![wafw00f hackertube.net](.gitbook/assets/image-20221126175530960.png)

```bash
# -a option
wafw00f hackertube.net -a
wafw00f zonetransfer.me -a
```

![wafw00f hackertube.net -a](.gitbook/assets/image-20221126175831387.png)

![wafw00f zonetransfer.me -a](.gitbook/assets/image-20221126175930959.png)

- This would be definitely tested within the *active information gathering* phase with a port scan on the webserver IP address.

### Subdomain Enumeration with Sublist3r

To identify the subdomains of a specific domain in a passive way, **publicly available resources and databases** can be utilized.

- [**`sublist3r`**](https://github.com/aboul3la/Sublist3r) tool - a Python tool that enumerate subdomains of websites using OSINT (**O**pen-**S**ource **Int**eligence).
  - this example is NOT active enumeration - is is passive (using public available resources)
  - it enumerates subdomains using search engines (Google, Yaoo, Bing ...) and other tools (Netcraft, Virustotal, DNSdumpster, ReverseDNS, ThreatCrowd).

![sublist3r -h](.gitbook/assets/image-20221126181132532.png)

```bash
sudo apt install sublist3r

sublist3r -d hackersploit.com
sublist3r -d hackersploit.com -e google,yahoo
sublist3r -d hackersploit.com -o hs_sub_enum.txt
# Find hackersploit.com subdomains and save the results to a text file
```

![sublist3r -d ine.com](.gitbook/assets/image-20221126190544117.png)

### Google Dorks

**Google Dorking**/Hacking can be utilized to identify public information pertinent to a target.

- Search filters for specific subdomains, files, etc using [google.com](https://www.google.com/).
- First try to directly search for the specific domain and look for useful information.

!["ine.com" on Google Search](.gitbook/assets/image-20221129205552223.png)

- **`site:`**
  - limit all results to the particular domain/site
  - shows subdomains for that particular domain

!["site:ine.com" on Google Search](.gitbook/assets/image-20230117171129629.png)

!["site:ine.com employees" standard Google Search](.gitbook/assets/image-20230117173818457.png)

- **`inurl:`**
  - look for specific results within the website title/URL
  - **`e.g.`** - `inurl:admin` , etc.

!["site:ine.com inurl:forum" on Google Search](.gitbook/assets/image-20230117171241409.png)

- **`site:*.site.com`**
  - show **subdomains** (indexed by Google) for a particular domain
  - usually they are exposed subdomains
    - sometimes unintended exposed subdomains

!["site:*.ine.com" on Google Search](.gitbook/assets/image-20230117172425752.png)

- **`intitle:`**
  - limit the results to subdomains with a specific word in the site title

!["site:*.ine.com intitle:forum" on Google Search](.gitbook/assets/image-20230117172953025.png)

- **`filetype:`**
  - limit the results to a file type in the URL
  - make the search query a bit more specific

!["site:*.ine.com filetype:pdf" on Google Search](.gitbook/assets/image-20230117173421370.png)

- **`intitle:index of`**
  - look for sites with **directory listing** enabled, searching for ***index of***
  - common web servers vulnerability/misconfiguration (against security)
  - directory listing allows users to see the content of the directory

!["intitle:index of" on Google Search](.gitbook/assets/image-20230117174445549.png)

- **`cache:`**
  - shows the cached website

!["cache:ine.com" on Google Search](.gitbook/assets/image-20230117174639735.png)

- Other Google dorking examples:
  - **`inurl:auth_user_file.txt`**
  - **`inurl:passwd.txt`**
  - **`inurl:wp-config.bak`**
- [**Google Hacking Database - exploit-db.com**](https://www.exploit-db.com/google-hacking-database)
    - use it to search for Dorks by Category to find potentially unsecured files

![Google Hacking Database - exploit-db.com](.gitbook/assets/image-20230117183216690.png)

- [**Wayback Machine**](https://archive.org/web/)
  - a digital archive by the Internet Archive
  - captures/snapshots web pages over time
  - check earlier version of websites
  - on older versions of the websites there can be useful **sensitive information** leaked

![ine.com on Wayback Machine](.gitbook/assets/image-20230117180911030.png)

![01.09.2012 - ine.com on Wayback Machine](.gitbook/assets/image-20230117181058309.png)

### Email Harvesting

- [**`theHarvester`**](https://github.com/laramies/theHarvester) tool - an open-source Python tool that performs OSINT gathering to help determine a domain's external threat landscape.
  - used to enumerate the emails (names, IPs, URLs, subdomains) belonging to a domain target, using **publicly available resources and databases**.
  - check the GitHub repository for more information on the [Passive](https://github.com/laramies/theHarvester#passive) and [Active](https://github.com/laramies/theHarvester#active) information gathering and [Installation](https://github.com/laramies/theHarvester/wiki/Installation).
- In this case the tool is used for Email Harvesting.

![theHarvester -h](.gitbook/assets/image-20230117185341402.png)

```bash
# Pre-installed on Kali Linux.
theHarvester -d hackersploit.org
theHarvester -d hackersploit.org -b dnsdumpster,duckduckgo,crtsh
# It finds some subdomains
```

![theHarvester -d hackersploit.org](.gitbook/assets/image-20230117185637805.png)

![theHarvester -d hackersploit.org -b dnsdumpster,duckduckgo,crtsh](.gitbook/assets/image-20230117185933279.png)

```bash
theHarvester -d zonetransfer.me -b all
```

![theHarvester -d zonetransfer.me -b all](.gitbook/assets/image-20230117190350307.png)

- *Emails could be used to send phishing email with malicious attachments during an attack.*

### Leaked Password Databases

Email or account passwords can be potentially found and used for a **password spray attack** = use the discovered passwords and test them for authentication on many other services (*not part of Passive info gathering*).

- Leaked online password databases can be utilized, usually coming from a site data breach containing the users credentials.
- [**haveibeenpwned.com**](https://haveibeenpwned.com/) site by [Troy Hunt](https://www.troyhunt.com/)
  - safe, reliable, no signup
  - insert the found target email in the site to check for data breaches
  - for older emails there is a greater chance of finding data breaches!


![haveibeenpwned.com - clean](.gitbook/assets/image-20230117192114283.png)

![haveibeenpwned.com - breached](.gitbook/assets/image-20230117192336443.png)

------

## Active Information Gathering

### DNS Zone Transfers

> 🗒️ Check my basic DNS theory notes [here](../penetration-testing-prerequisites/networking.md#dns).
>
> 📌 More in depth explanations about DNS can be found at the [Cloudflare Learning Center](https://www.cloudflare.com/learning/dns/what-is-dns/).

- Most common types of DNS:

| Record Type | Description                                                  |
| :---------: | :----------------------------------------------------------- |
|    **A**    | Holds/Resolves the IPv4 address of a domain/hostname         |
|  **AAAA**   | Holds/Resolves the IPv6 address of a domain/hostname         |
|  **CNAME**  | Used for domain aliases, forwards one domain/subdomain to another domain |
|   **MX**    | Resolves a domain to a mail server                           |
|   **TXT**   | Used for admin text notes, often used for email security     |
|   **NS**    | Reference to the domains name server                         |
|   **SOA**   | Stores admin information about a domain (domain authority)   |
|  **HINFO**  | Host information                                             |
|   **SRV**   | Specific services records                                    |
|   **PTR**   | Resolves an IP address to a hostname - reverse lookups       |

*Enumerating DNS records* for a particular domain is done through a procedure known as **DNS Interrogation**.

- Probe a DNS server to provide additional records and information (domain IP address, subdomains, mail server addresses, etc)

To obtain more records from a DNS server with regards to a particular domain, **DNS Zone Transfers** may be useful:
- A **zone transfer** occurs when a system admin may want to *copy or transfer zone files* (containing domain records) from one DNS server to another.
- *This functionality can be abused by attackers when left misconfigured, to copy the zone file from the primary DNS to another DNS server.*
- It can give penetration testers a complete picture of the network architecture of an organization and internal network addresses may be found.

An IP address can be mapped to a local (or external) specific domain name using the **`/etc/hosts`** file:

```bash
# Before DNS, the O.S. would use the host file for DNS resolution:
sudo nano /etc/hosts
    127.0.0.1       localhost
    127.0.1.1       kali

    # The following lines are desirable for IPv6 capable hosts
    ::1     localhost ip6-localhost ip6-loopback
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters
    
    # IP ADDRESS    # Domain Names
```

**`E.g.`** - [**`ZoneTransfer.me`**](https://digi.ninja/projects/zonetransferme.php) can be utilized for educational purposes

- [Passive recon here](#dns-Reconnaissance) - using `dnsdumpster.com`, `dnsrecon`

```bash
[-] DNSSEC is not configured for zonetransfer.me
[*] 	 SOA nsztm1.digi.ninja 81.4.108.41
[*] 	 NS nsztm1.digi.ninja 81.4.108.41
[*] 	 Bind Version for 81.4.108.41 secret"
[*] 	 NS nsztm2.digi.ninja 34.225.33.2
[*] 	 Bind Version for 34.225.33.2 you"
[*] 	 MX ASPMX4.GOOGLEMAIL.COM 142.251.8.27
[*] 	 MX ASPMX5.GOOGLEMAIL.COM 173.194.202.26
[*] 	 MX ALT2.ASPMX.L.GOOGLE.COM 74.125.200.27
[*] 	 MX ASPMX3.GOOGLEMAIL.COM 74.125.200.26
[*] 	 MX ALT1.ASPMX.L.GOOGLE.COM 142.250.150.26
[*] 	 MX ASPMX2.GOOGLEMAIL.COM 142.250.150.27
[*] 	 MX ASPMX.L.GOOGLE.COM 108.177.119.27
[*] 	 MX ASPMX4.GOOGLEMAIL.COM 2404:6800:4008:c15::1b
[*] 	 MX ASPMX5.GOOGLEMAIL.COM 2607:f8b0:400e:c00::1b
[*] 	 MX ALT2.ASPMX.L.GOOGLE.COM 2404:6800:4003:c00::1b
[*] 	 MX ASPMX3.GOOGLEMAIL.COM 2404:6800:4003:c00::1a
[*] 	 MX ALT1.ASPMX.L.GOOGLE.COM 2a00:1450:4010:c1c::1a
[*] 	 MX ASPMX2.GOOGLEMAIL.COM 2a00:1450:4010:c1c::1b
[*] 	 MX ASPMX.L.GOOGLE.COM 2a00:1450:4013:c07::1a
[*] 	 A zonetransfer.me 5.196.105.14
[*] 	 TXT zonetransfer.me google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA
[*] Enumerating SRV Records
[+] 	 SRV _sip._tcp.zonetransfer.me www.zonetransfer.me 5.196.105.14 5060
```

- [**`dnsenum`**](https://www.kali.org/tools/dnsenum/) tool - a multithread Perl script to enumerate DNS information of a domain and to discover non-contiguous ip blocks
  - enumerate public DNS records
  - perform automatic DNS zone transfer
  - perform DNS brute force on subdomains

![dnsenum --help](.gitbook/assets/image-20230118234549826.png)

- The two name server of ZoneTransfer.me are **`nsztm1.digi.ninja`** and **`nsztm2.digi.ninja`**
  - DNS Zone transfer functionality must be ON on the Name Servers.
  - Identify subdomains and internal IP addresses from the Zone Transfer results.

```bash
dnsenum zonetransfer.me

    dnsenum VERSION:1.2.6
    -----   zonetransfer.me   -----

	# PASSIVE RECON
    Host's addresses:
    __________________
    zonetransfer.me.        	5	IN	A	5.196.105.14
	# ^^ Web server IP address ^^
	
    Name Servers:
    ______________
    nsztm2.digi.ninja.      	5	IN	A	34.225.33.2
    nsztm1.digi.ninja.      	5	IN	A	81.4.108.41

    Mail (MX) Servers:
    ___________________
    ALT2.ASPMX.L.GOOGLE.COM.	5	IN	A	74.125.200.27
    ASPMX4.GOOGLEMAIL.COM.  	5	IN	A	142.251.8.26
    ASPMX.L.GOOGLE.COM.     	5	IN	A	108.177.119.27
    ASPMX2.GOOGLEMAIL.COM.  	5	IN	A	142.250.150.27
    ALT1.ASPMX.L.GOOGLE.COM.	5	IN	A	142.250.150.26
    ASPMX3.GOOGLEMAIL.COM.  	5	IN	A	74.125.200.27
    ASPMX5.GOOGLEMAIL.COM.  	5	IN	A	173.194.202.26

	# ACTIVE RECON
    Trying Zone Transfers and getting Bind Versions:
    _________________________________________________
    Trying Zone Transfer for zonetransfer.me on nsztm1.digi.ninja ...
    # Provides all the records stored on the NS nsztm1.digi.ninja
    # Try to access the interesting ones
    
    zonetransfer.me.	7200	IN	SOA	nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
    zonetransfer.me.	300	IN	HINFO	"Casio fx-700G" "Windows XP"
    zonetransfer.me.	301	IN	TXT	"google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
    zonetransfer.me.	7200	IN	MX	0 ASPMX.L.GOOGLE.COM.
    zonetransfer.me.	7200	IN	MX	10 ALT1.ASPMX.L.GOOGLE.COM.
    zonetransfer.me.	7200	IN	MX	10 ALT2.ASPMX.L.GOOGLE.COM.
    zonetransfer.me.	7200	IN	MX	20 ASPMX2.GOOGLEMAIL.COM.
    zonetransfer.me.	7200	IN	MX	20 ASPMX3.GOOGLEMAIL.COM.
    zonetransfer.me.	7200	IN	MX	20 ASPMX4.GOOGLEMAIL.COM.
    zonetransfer.me.	7200	IN	MX	20 ASPMX5.GOOGLEMAIL.COM.
    zonetransfer.me.	7200	IN	A	5.196.105.14
    zonetransfer.me.	7200	IN	NS	nsztm1.digi.ninja.
    zonetransfer.me.	7200	IN	NS	nsztm2.digi.ninja.
    _acme-challenge.zonetransfer.me. 301 IN	TXT	"6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
    _sip._tcp.zonetransfer.me. 14000 IN	SRV	0 0 5060 www.zonetransfer.me.
    14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200	IN PTR www.zonetransfer.me.
    # Some subdomains (found actively):
    asfdbauthdns.zonetransfer.me. 7900 IN	AFSDB	1 asfdbbox.zonetransfer.me.
    asfdbbox.zonetransfer.me. 7200	IN	A	127.0.0.1
    asfdbvolume.zonetransfer.me. 7800 IN	AFSDB	1 asfdbbox.zonetransfer.me.
    canberra-office.zonetransfer.me. 7200 IN A	202.14.81.230
    # If not external, the IP could be an internal DNS Record = Security issue
    cmdexec.zonetransfer.me. 300	IN	TXT	"; ls"
    contact.zonetransfer.me. 2592000 IN	TXT	"Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes"
    dc-office.zonetransfer.me. 7200	IN	A	143.228.181.132
    deadbeef.zonetransfer.me. 7201	IN	AAAA	dead:beaf::
    dr.zonetransfer.me.	300	IN	LOC	53 20 56.558 N 1 38 33.526 W 0.00m 1m 10000m 10m
    DZC.zonetransfer.me.	7200	IN	TXT	"AbCdEfG"
    email.zonetransfer.me.	2222	IN	NAPTR	1 1 "P" "E2U+email" "" email.zonetransfer.me.zonetransfer.me.
    email.zonetransfer.me.	7200	IN	A	74.125.206.26
    Hello.zonetransfer.me.	7200	IN	TXT	"Hi to Josh and all his class"
    home.zonetransfer.me.	7200	IN	A	127.0.0.1
    Info.zonetransfer.me.	7200	IN	TXT	"ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information."
    internal.zonetransfer.me. 300	IN	NS	intns1.zonetransfer.me.
    internal.zonetransfer.me. 300	IN	NS	intns2.zonetransfer.me.
    intns1.zonetransfer.me.	300	IN	A	81.4.108.41
    intns2.zonetransfer.me.	300	IN	A	167.88.42.94
    # ^^ Pay ATTENTION to internal pointing addresses ^^
    office.zonetransfer.me.	7200	IN	A	4.23.39.254
    ipv6actnow.org.zonetransfer.me.	7200 IN	AAAA	2001:67c:2e8:11::c100:1332
    owa.zonetransfer.me.	7200	IN	A	207.46.197.32
    robinwood.zonetransfer.me. 302	IN	TXT	"Robin Wood"
    rp.zonetransfer.me.	321	IN	RP	robin.zonetransfer.me. robinwood.zonetransfer.me.
    sip.zonetransfer.me.	3333	IN	NAPTR	2 3 "P" "E2U+sip" "!^.*$!sip:customer-service@zonetransfer.me!" .
    sqli.zonetransfer.me.	300	IN	TXT	"' or 1=1 --"
    sshock.zonetransfer.me.	7200	IN	TXT	"() { :]}; echo ShellShocked"
    staging.zonetransfer.me. 7200	IN	CNAME	www.sydneyoperahouse.com.
    # ^^ Try this redirection on a browser. If it fails maybe it is an internal record.
    alltcpportsopen.firewall.test.zonetransfer.me. 301 IN A	127.0.0.1
    testing.zonetransfer.me. 301	IN	CNAME	www.zonetransfer.me.
    vpn.zonetransfer.me.	4000	IN	A	174.36.59.154
    www.zonetransfer.me.	7200	IN	A	5.196.105.14
    xss.zonetransfer.me.	300	IN	TXT	"'>alert('Boo')"
    zonetransfer.me.	7200	IN	SOA	nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
    Trying Zone Transfer for zonetransfer.me on nsztm2.digi.ninja ... 
    [...]

    Brute forcing with /usr/share/dnsenum/dns.txt:
    # Used primarily to find subdomains
    _______________________________________________
    office.zonetransfer.me.       	5	IN	A		4.23.39.254
    owa.zonetransfer.me.          	5	IN	A		207.46.197.32
    staging.zonetransfer.me.      	5	IN	CNAME	www.sydneyoperahouse.com.
    www.sydneyoperahouse.com.     	5	IN	CNAME	d3gdbrxsb9xhmf.cloudfront.net.
    d3gdbrxsb9xhmf.cloudfront.net.	5	IN	A		13.224.103.62
    d3gdbrxsb9xhmf.cloudfront.net.	5	IN	A		13.224.103.26
    d3gdbrxsb9xhmf.cloudfront.net.	5	IN	A		13.224.103.84
    d3gdbrxsb9xhmf.cloudfront.net.	5	IN	A		13.224.103.17
    vpn.zonetransfer.me.          	5	IN	A		174.36.59.154
    www.zonetransfer.me.          	5	IN	A		5.196.105.14

    zonetransfer.me class C netranges:
    ___________________________________
     4.23.39.0/24
     5.196.105.0/24
     174.36.59.0/24
     207.46.197.0/24

    Performing reverse lookup on 1024 ip addresses:
    ________________________________________________
    0 results out of 1024 IP addresses.

    zonetransfer.me ip blocks:
    ___________________________
    
    done.
```

- `dnsenum` can fail if zone transfer is disabled (e.g. Cloudflare NS)

![dnsenum hackersploit.org](.gitbook/assets/image-20230119010146033.png)

- [**`dig`**](https://linuxize.com/post/how-to-use-dig-command-to-query-dns-in-linux/) tool - query DNS name servers

```bash
dig axfr @nsztm1.digi.ninja zonetransfer.me 
# axfr is the zone transfer switch
```

- *[**AXFR zone transfers**](https://www.cloudns.net/blog/zone-transfer-zone-file-domain-namespace/) are the full DNS zone transfers of all DNS data. The Primary DNS server sends the whole zone file that contains all the DNS records to the Secondary DNS servers. This assures that the secondary DNS server is well synced. It will have all the latest changes that were made to the Master DNS zone.*

> - [Zone Transfer Online Test](https://hackertarget.com/zone-transfer/)
> - [Linux and Unix dig Command Examples - by Vivek Gite](https://www.cyberciti.biz/faq/linux-unix-dig-command-examples-usage-syntax/)

- [**`fierce`**](https://github.com/mschwager/fierce) - a semi-lightweight scanner that helps locate non-contiguous IP space and hostnames against specified domains, using DNS primarily

```bash
fierce --domain zonetransfer.me

    NS: nsztm2.digi.ninja. nsztm1.digi.ninja.
    SOA: nsztm1.digi.ninja. (81.4.108.41)
    Zone: failure
    Wildcard: failure
    Found: email.zonetransfer.me. (74.125.206.26)
    Nearby:
    {'74.125.206.21': 'wk-in-f21.1e100.net.',
	[...]
     '74.125.206.31': 'wk-in-f31.1e100.net.'}
    Found: home.zonetransfer.me. (127.0.0.1)
    Found: office.zonetransfer.me. (4.23.39.254)
    Found: owa.zonetransfer.me. (207.46.197.32)
```

> ✍️ "Zone transfers are rare these days, but they give us the keys to the DNS castle." [fierce - Geeksforgeeks](https://www.geeksforgeeks.org/fierce-dns-reconnaissance-tool-for-locating-non-contiguous-ip-space/)

### Host Discovery with Nmap

- 

### Port Scanning With Nmap

- 
