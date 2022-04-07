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

![hpbn.co/http1x/](.gitbook/assets/image-20220407181153473.png)

### HTTP Requests

| *The structure of an HTTP message* |
| ---------------------------------- |
| **Start line**\r\n                 |
| **Headers**\r\n                    |
| \r\n                               |
| **Message Body**\r\n               |

- **`\r\n`** (carriage return & newline) are used to the lines in HTTP.

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

The web server processes the request and sends an HTTP response back to the client:

  - The start-line, called the `status-line`, contains the protocol version, a status code and a status text.
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

|        Status code        | Meaning                                                      |
| :-----------------------: | :----------------------------------------------------------- |
|          200 OK           | the request has succeeded (***Successful codes 2xx***)       |
|   301 Moved Permanently   | the target resource has been assigned a new permanent URI (***Redirection codes 3xx***) |
|         302 Found         | the target resource resides temporarily under a different URI (Redirection codes 3xx) |
|       403 Forbidden       | the server understood the request but refuses to authorize it, client doesn't have enough privileges (***Client Error codes 4xx***) |
|       404 Not Found       | the origin server did not find a current representation for the target resource or is not willing to disclose that one exists (Client Error codes 4xx) |
| 500 Internal Server Error | the server encountered an unexpected condition that prevented it from fulfilling the request (***Server Error codes 5xx***) |
|      502 Bad Gateway      | the server, while acting as a gateway or proxy, received an invalid response from an inbound server it accessed while attempting to fulfill the request (Server Error codes 5xx) |

> 📌 For more in depth info refer to [RFC 7321 -  Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content](https://httpwg.org/specs/rfc7231.html)
>

## HTTPS Protocol Basics

