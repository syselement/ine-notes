# Intro to Web App Pentesting

> #### ⚡ Prerequisites
>
> * Basic Network and Cybersecurity Concepts
>
> #### 📕 Learning Objectives
>
> * Understand **Web protocols**
> * Perform **webapps enumeration**
> * Perform **SQL injection**, **XSS** and **brute-force** attacks
>
> #### 🔬 Training list - PentesterAcademy/INE Labs
>
> `subscription required`
>
> - [Web Application Basics](https://attackdefense.com/listing?labtype=webapp-web-app-basics&subtype=webapp-web-app-basics-getting-started)
> - [Web Apps Tools of Trade](https://attackdefense.com/listing?labtype=webapp-tools-of-trade&subtype=webapp-tools-of-trade-getting-started)

**Web application penetration testing** is a process of identifying and exploiting vulnerabilities in web applications to assess their security posture.

## Web and [HTTP Protocol](https://developer.mozilla.org/en-US/docs/Web/HTTP)

> 🔗📝 Some **Web Applications Basics** notes are already covered [here](../penetration-testing-prerequisites/web-applications.md) (from the PTSv1 Course)

🗒️ **`HTTP`** (**H**yper**T**ext **T**ransfer **P**rotocol) is a protocol used for communication between web servers and clients, such as web browsers. **`HTTP`** key features are:

- Client-Server Architecture
- Stateless Protocol
- Request Methods
- Status Codes (`200`,`404`,`500`, etc)
- [Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) (additional information about the request/response)
- Cookies (store info on the client-side)
- Encryption (`HTTPS`)

> 📌 [RFC 9110 - HTTP Semantics](https://httpwg.org/specs/rfc9110.html)

### [Request Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

*HTTP defines a set of **request methods** to indicate the desired action to be performed for a given resource.* Commonly used HTTP requests are:

`GET` - retrieve data from the server

`HEAD` - retrieve metadata about a resource from the server

`POST` - submit data to the server

`PUT` - update an existing resource on the server

`DELETE` - delete a specified resource

`CONNECT` - establish a tunnel to the server identified by the target resource

`OPTIONS` - describe the communication options for a resource

`TRACE` - perform a message loop-back test along the path to the resource

`PATCH` - apply partial modifications to a resource

### [Response Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

*HTTP **response status codes** indicate whether a specific `HTTP` request has been successfully completed.* They are grouped in five classes:

- `100-199` - Informational responses
- `200-299` - Successful responses
- `300-399` - Redirection messages
- `400-499` - Client error responses
- `500-599` - Server error responses

### [Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

*An HTTP **cookie** is a small piece of data that a server sends to a user's web browser. The web browser may store the cookie and send it back to the same server with later requests.* Cookies are mainly used for these purposes:

- Session management
- Personalization
- Tracking

### [HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/HTTPS)

🗒️ **`HTTPS`** (HTTP Secure) is the encrypted version of `HTTP` that uses a combination of Transport Layer Security (**`TLS`**) or Secure Sockets Layer (**`SSL`**) protocol and HTTP protocol to provide secure communication.

When a client connects to an HTTPS-enabled website, the server sends its `SSL`/`TLS` **certificate** to the client. The client verifies the certificate to ensure that it is issued by a **trusted certificate authority** and that it is valid. If the certificate is valid, the client and the server establish a secure connection using a unique **session key**.

> 🔬 There are many vulnerable testing web apps like:
>
> - [Juice Shop - Kali Install](https://www.kali.org/tools/juice-shop/)
> - [DVWA - Kali Install](https://www.kali.org/tools/dvwa/)
> - [bWAPP](http://www.itsecgames.com/)
> - [Mutillidae II](https://github.com/webpwnized/mutillidae)
>
> 📝 Check the HackerSploit's [Web App Penetration Testing Tutorials](https://www.youtube.com/playlist?list=PLBf0hzazHTGO3EpGAs718LvLsiMIv9dSC)
>
> ```bash
> # bWAPP with Docker - by HackerSploit
> sudo docker pull hackersploit/bwapp-docker
> 
> sudo docker run -d -p 80:80 hackersploit/bwapp-docker
> # Open http://127.0.0.1/install.php
> 
> sudo docker container ls
> sudo docker container stop <CONTAINER_NAME>
> sudo docker container start <CONTAINER_NAME>
> ```

```bash
nmap -sV -p 80,443,3306 demossl.ine.local
```

## Scanning & Enumeration

### Directory Enumeration - [Gobuster](https://github.com/OJ/gobuster)

[**`Gobuster`**](https://www.kali.org/tools/gobuster/) - *a tool used to brute-force URIs including directories and files as well as DNS subdomains.*

```bash
# Kali Linux Install
sudo apt update && sudo apt install -y gobuster

# Go Install
go install github.com/OJ/gobuster/v3@latest
```

### Directory Enumeration - [BurpSuite](https://portswigger.net/burp/documentation/desktop)

[**`BurpSuite`**](https://www.kali.org/tools/burpsuite/) - *an integrated platform for performing security testing of web applications.*

```bash
# Kali Linux Install
sudo apt update && sudo apt install -y burpsuite
```

> 🔬 Check [HTTP Web App Enumeration lab](1-webapp-http/http-enum.md) covering HTTP Method and Directory Enumeration Techniques

### Scanning WebApp - [ZAProxy](https://www.zaproxy.org/)

[**`Zaproxy`**](https://www.kali.org/tools/zaproxy/) - *OWASP Zed Attack Proxy (**ZAP**) is an easy to use integrated penetration testing tool for finding vulnerabilities in web applications.*

```bash
# Kali Linux Install
sudo apt update && sudo apt install -y zaproxy
```

### Scanning WebApp - [Nikto](https://github.com/sullo/nikto)

[**`Nikto`**](https://www.kali.org/tools/nikto/) - *a pluggable web server and CGI scanner written in Perl, using rfp’s LibWhisker to perform fast security or informational checks.*

```bash
# Kali Linux Install
sudo apt update && sudo apt install -y nikto
```

> 🔬 Check [HTTP Web App Scanning lab](1-webapp-http/webapp-scan.md) covering Web Apps scanning techniques
>

## Attacks

[**`SQLMap`**](https://www.kali.org/tools/sqlmap/) - *an open source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over of database servers.*

```bash
# Kali Linux Install
sudo apt update && sudo apt install -y sqlmap
```

[**`XSSer`**](https://www.kali.org/tools/xsser/) (Cross-Site Scripter) - *an automatic framework to detect, exploit and report XSS vulnerabilities in web-based applications.*

```bash
# Kali Linux Install
sudo apt update && sudo apt install -y xsser
```

### SQLi

🗒️ [**SQL Injection**](https://owasp.org/www-community/attacks/SQL_Injection) *attacks consist of insertion or “**injection**” of a `SQL` query via the input data from the client to the application, allowing an attacker to interfere with the database queries of the vulnerable web application.*

- [What is a SQLi? - PortSwigger](https://portswigger.net/web-security/sql-injection)

### XSS

🗒️ **Cross-Site Scripting** ([**XSS**](https://owasp.org/www-community/attacks/xss/#)) *attacks are a type of injection, in which malicious scripts are injected into otherwise benign and trusted websites, allowing an attacker to compromise the interactions that users have with a vulnerable application.*

- [How does XSS Work? - PortSwigger](https://portswigger.net/web-security/cross-site-scripting)

> 🔬 Check [Web App Attacks lab](1-webapp-http/webapp-attacks.md) covering Web Apps Attacking techniques

------

