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

**Information gathering** is the initial stage of any penetration test and one of the most important phase.

- It involves finding out as much information as possible about a targeted individual, website, company or system.
- The more information a pentester has on a **target**, the more successful and easier the latter stages of a pentest will be. It depends on the **scope** of the penetration test too.
- `E.g.1` - Pentest on a Website: web technology, vulnerabilities, IP address of the hosting server.
- `E.g.2` - Pentest on a public facing assets and some internal systems, there can be more attack vectors:
  - gain access to the internal network through the public facing web server (one **access vector**)
  - during the info-gathering phase, learn more about the company employees (names, email addresses, credentials), getting this important information (useful for exploitation or initial access) by using phishing attacks, malicious attachments via email (another **access vector**)


## Passive Information Gathering

**Passive information gathering** involves *obtaining as much data as possible without actively interacting with the target*. 

- The pentester uses what's available on the Internet.
- `E.g.` - Website: utilizing publicly accessible information and resources of that particular website, through the browser, public IP address of the webserver hosting that website, etc.

### What passive information?

- IP addresses, DNS, domain names and domain ownership
- Email addresses, social media profiles
- Web technologies, subdomains

## Active Information Gathering

**Active information gathering** involves *obtaining as much information as possible by actively engaging with the target*.

- ❗***An authorization is required to conduct active information gathering.***
- The target will be aware of the attacker's engagement.
- `E.g.` - Website: perform a port scan of the webserver IP address (found with passive info gathering) using **`nmap`** tool to identify the open ports and running services. Identify exploitable vulnerabilities on those services and consequently access the web server.

### What active information?

- Open ports, internal network/organization infrastructure
- Enumeration targe info

### Code of conduct

> 📌 From the [The Pentester's Code of Conduct - by Sherri Davidoff](https://www.lmgsecurity.com/the-pentesters-code-of-conduct-rules-that-keep-everyone-safe/)
>
> - **Know your scope.** Make sure that you have reviewed written documentation which clearly describes the goals of testing, what you are testing, how you are testing it, and any constraints.
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

------

## Passive Information Gathering

### Website Recon & Footprinting

### Whois Enumeration

### Website Footprinting with Netcraft

### DNS Reconnaissance

### WAF

### Subdomain Enumeration with Sublist3r

### Google Dorks

### Email Harvesting

### Leaked Password Databases

------

## Active Information Gathering

### DNS Zone Transfers

### Host Discovery with Nmap

### Port Scanning With Nmap
