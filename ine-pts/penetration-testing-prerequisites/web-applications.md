# Web Applications

- **`Web applications`** are software that runs on web servers and are accessible via web browsers.
- The web app world is heterogeneous, since every web app can be differently developed to accomplish the same task.
- This great flexibility in development results in more flexibility in creating insecure code and messing things up.

> 📕 *"With great power comes great responsibility."* {Stan Lee}

## HTTP Protocol Basics

> ⚡ P.T. Usage:
>
> * Ability to *exploit* web applications and find *vulnerabilities* in web servers and services
> * Web app technology is used market-wide also by desktop and mobile apps

**`HTTP`** (**H**ypertext **T**ransfer **P**rotocol) is one of the most used application protocol on the Internet, built on top of TCP.

- It is the **client-server protocol** used to transfer web pages and web application data.
- Is is a **stateless** protocol.
- The client (a web browser) connects to a web server (usually Microsoft IIS, Apache HTTP Server, or others) by sending HTTP **requests** to the server and getting back HTTP **responses**.
- First a TCP connection is established, then the HTTP messages are sent.

![HTML Request - image by hpbn.co](.gitbook/assets/image-20220407181153473.png)

### HTTP Requests

| *The structure of an HTTP message* |
| ---------------------------------- |
| "**Start line**\r\n"               |
| "**Headers**\r\n"                  |
| "\r\n"                             |
| "**Message Body**\r\n"             |

- **`"\r\n"`** (carriage return & newline) are used to the lines in HTTP.

- Each start-line contains 3 elements:

  1. an `HTTP method` (verb/noun) that states the type of the request, like: GET, PUT, POST, HEAD, OPTIONS
  2. the `request target` (URL/path), which resource the browser is asking for
  3. the *protocol version* (`HTTP/1.x`)which defines the structure of the remaining message, telling the server how to communicate with the browser

  ```http
  GET / HTTP/1.1
  ```

- Each header is represented in a single line, composed of a String followed by a colon (**:**) and a value:

  - *Header-name: value*

  ```http
  Host: example.com
  Upgrade-Insecure-Requests: 1
  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36
  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
  Accept-Encoding: gzip, deflate
  Accept-Language: en-US,en;q=0.9
  Connection: close
  
  (body)
  ```
  
  - The `Host` value is obtained from the URI (**U**niform **R**esource **I**dentifier) of the resource.
  - `User-Agent` - reveals the client software issuing the request (Chrome, Safari, Firefox, mobile app ...) and the operating system version.
  - `Accept` - expected document type.

### HTTP Responses

The web server *processes the request and sends an HTTP response* back to the client:

  - The start-line, called the **`status-line`**, contains the protocol version, a status code and a status text.
  - HTTP headers same as above, different headers may appear, such as:
    - `Cache-Control` - informs the client about cached content (saves bandwidth)
    - `Content-Type` - lets the client know how to interpret the body
    - `Server` - header of the web server, software running (useful in pentesting!)
    - `Content-Length` - length in bytes of the message body

  - The `body` is separated from the header by 2 empty lines (\r\n\r\n).

```http
HTTP/1.1 200 OK
Accept-Ranges: bytes
Age: 259261
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Thu, 07 Apr 2022 17:07:05 GMT
Etag: "3147526947"
Expires: Thu, 14 Apr 2022 17:07:05 GMT
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Server: ECS (dcb/7EA5)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1256
Connection: close

<!doctype html>
<html>
...
</html>

```

- *HTTP Request / Response example*

![](.gitbook/assets/image-20220407192415082.png)

- Common status codes:

|          Status code          | Meaning                                                      |
| :---------------------------: | :----------------------------------------------------------- |
|          **200** OK           | the request has succeeded (***Successful codes 2xx***)       |
|   **301** Moved Permanently   | the target resource has been assigned a new permanent URI (***Redirection codes 3xx***) |
|         **302** Found         | the target resource resides temporarily under a different URI (Redirection codes 3xx) |
|       **403** Forbidden       | the server understood the request but refuses to authorize it, client doesn't have enough privileges (***Client Error codes 4xx***) |
|       **404** Not Found       | the origin server did not find a current representation for the target resource or is not willing to disclose that one exists (Client Error codes 4xx) |
| **500** Internal Server Error | the server encountered an unexpected condition that prevented it from fulfilling the request (***Server Error codes 5xx***) |
|      **502** Bad Gateway      | the server, while acting as a gateway or proxy, received an invalid response from an inbound server it accessed while attempting to fulfill the request (Server Error codes 5xx) |

> 📌 For more in depth info refer to [RFC 7321 -  Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content](https://httpwg.org/specs/rfc7231.html)
>

## HTTPS Protocol Basics

HTTP (a clear-text protocol) can be protected using an **encryption** layer, by using a *cryptographic protocol* like SSL/TLS.

**`HTTPS`** (HTTP Secure) is a method to run HTTP over SSL/TLS. 

- This encryption layer protects data exchanged between the client and the server, but it does *not protect against web application flaws*. XSS and SQL injections attack will still work.
- It provides **confidentiality**, **integrity protection** and **authentication** to the HTTP protocol.
- Application layer communication cannot be sniffed or altered by an eventual attacker.
- Traffic can be sniffed and inspected by a user, but he cannot know HTTP headers, body, target domain or what data is exchanged.
  - Target IP address and target port can be recongnized
  - DNS (or similar protocols) may disclose which domain user tries to resolve

![TLS Handshake - image by hpbn.co](.gitbook/assets/image-20220411201109560.png)

> 📌 Refer to the [High Performance Browser Networking](https://hpbn.co/transport-layer-security-tls/) book for more in depth information.

## Command Line / GUI Tools

### Netcat

**`netcat`** or **`nc`** is a tool that reads and writes data accross network connections (opens a raw connection to a service port for example), using TCP or UDP protocol.

- considered as a Swiss army knife of networking tools
- used to debug and monitor network connections, scan for open ports, transfer data, listen for incoming connections (reverse shells) and more

**`nc -h`**

![](.gitbook/assets/image-20220411221502839.png)

- Establishing a connection to a server and communicate via HTTP:

> - **`nc -v example.com 80`**
>
> ```bash
> $ nc -v example.com 80
> Warning: inverse host lookup failed for 93.184.216.34: Unknown host
> example.com [93.184.216.34] 80 (http) open
> GET / HTTP/1.1			# Input Request
> Host: example.com		# 2 lines \r\n after this line
> 
> HTTP/1.1 200 OK			# Response - status line
> Accept-Ranges: bytes
> Age: 414218
> Cache-Control: max-age=604800
> Content-Type: text/html; charset=UTF-8
> Date: Mon, 11 Apr 2022 20:05:16 GMT
> Etag: "3147526947"
> Expires: Mon, 18 Apr 2022 20:05:16 GMT
> Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
> Server: ECS (dcb/7F84)
> Vary: Accept-Encoding
> X-Cache: HIT
> Content-Length: 1256
> 
> <!doctype html>
> <html>
> <head>
>     ...
> </head>
> 
> <body>
> ...
> </body>
> </html>
> 
> ```
>
> - **`nc -v my.ine.com 80`**
>
> ```bash
> $ nc -v my.ine.com 80                                                     
> DNS fwd/rev mismatch: my.ine.com != server-108-138-36-10.muc50.r.cloudfront.net
> DNS fwd/rev mismatch: my.ine.com != server-108-138-36-101.muc50.r.cloudfront.net
> DNS fwd/rev mismatch: my.ine.com != server-108-138-36-96.muc50.r.cloudfront.net
> DNS fwd/rev mismatch: my.ine.com != server-108-138-36-29.muc50.r.cloudfront.net
> my.ine.com [108.138.36.10] 80 (http) open
> GET / HTTP/1.1			# Input Request
> Host: my.ine.com		# 2 lines \r\n after this line
> 
> HTTP/1.1 301 Moved Permanently	# Response - status line
> Server: CloudFront
> Date: Mon, 11 Apr 2022 20:18:56 GMT
> Content-Type: text/html
> Content-Length: 183
> Connection: keep-alive
> Location: https://my.ine.com/	# Index page moved here
> X-Cache: Redirect from cloudfront	# X- headers are for non-standard usage
> Via: 1.1 210c8ad3e752d602af05a2de06eb2ff8.cloudfront.net (CloudFront)
> X-Amz-Cf-Pop: MUC50-P2	
> X-Amz-Cf-Id: 7zDSRlBNtqmIjJuUwda9jFQkwxA0lzp4w0Lv3R0vNlkzdapfvQPRcQ==
> 
> <html>
> <head><title>301 Moved Permanently</title></head>
> <body bgcolor="white">
> <center><h1>301 Moved Permanently</h1></center>
> <hr><center>CloudFront</center>
> </body>
> </html>
> 
> HEAD / HTTP/1.1			# Input HEAD Request
> Host: my.ine.com		# 2 lines \r\n after this line
> 
> ...
> ```

- Check if a port is opened:

> - **`nc -zv HOST PORT#`**
>   - **-z** performs a port scan against a given host and port or port range.
>
> ```bash
> $ nc -zv my.ine.com 443 
> DNS fwd/rev mismatch: my.ine.com != server-143-204-98-103.fra50.r.cloudfront.net
> DNS fwd/rev mismatch: my.ine.com != server-143-204-98-77.fra50.r.cloudfront.net
> DNS fwd/rev mismatch: my.ine.com != server-143-204-98-59.fra50.r.cloudfront.net
> DNS fwd/rev mismatch: my.ine.com != server-143-204-98-6.fra50.r.cloudfront.net
> my.ine.com [143.204.98.103] 443 (https) open	# Port 443 open
> ```

### Burp Suite

**`Burp Suite`** is an integrated platform and graphical tool for performing security testing of web apps, by testing, mapping and analyze the application's attack surface.

- it is written in Java, developed by [PortSwigger](https://portswigger.net/).
- can be used as proxy server, scanner, intruder, repeater, spider, decoder, comparer, sequencer, extender and more.

Using the ***Repeater*** tool:

- Example of a GET request on port 80:
![](.gitbook/assets/image-20220412002111740.png)
- Example of a *301 Moved Permanently* response and redirection:
  - 301 response received:
  ![](.gitbook/assets/image-20220412002857935.png)
  - By clicking the **`Follow redirection`** button, it follows the redirection (to the *Location*) in the current response without manually modifying the request to the target domain. I this case it got redirected to the HTTPS page:
  ![](.gitbook/assets/image-20220412003409074.png)

### OpenSSL

...
