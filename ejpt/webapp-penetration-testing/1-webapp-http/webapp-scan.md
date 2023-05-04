# 🔬Web App Scanning

## Lab 1 - [ZAProxy](https://www.zaproxy.org/)

> 🔬 [Scanning Web Application with ZAProxy](https://attackdefense.com/challengedetails?cid=1888)
>
> - Target IP: `192.192.29.3`
> - Scan and identify a vulnerable web app (bWAPP) with **ZAProxy**

```bash
ip -br -c a
	eth1@if203734  UP  192.192.29.2/24

nmap -sS -sV 192.192.29.3
```

- Start **`owasp-zap`** from the start menu
  - Use **`Manual Explore`** and input the URL
    - `http://192.192.29.3/`
  - **Launch Browser** to open the browser session with the ZAP HUB

![](.gitbook/assets/image-20230504161231898.png)

- Login to the web app with `bee`:`bug` credentials
  - set the security level to `low`

![](.gitbook/assets/image-20230504173337687.png)

- Try some HTML and SQL Injection or other bugs from the `https://192.192.29.3/portal.php` page

### ZAProxy

- Configure authenticated session in ZAProxy

![](.gitbook/assets/image-20230504162052925.png)

![](.gitbook/assets/image-20230504162245625.png)

![](.gitbook/assets/image-20230504162342918.png)

- Enable `Forced User mode`

![](.gitbook/assets/image-20230504162512403.png)

- `Include in Context` the Site `https://192.192.29.3/` and confirm with **`OK`**

![](.gitbook/assets/image-20230504162721398.png)

- Run a **`Spider`** attack on the site, select the `bee` user and Start the scan

![](.gitbook/assets/image-20230504162803069.png)

![196 URLs Found](.gitbook/assets/image-20230504162912742.png)

- Run an **`Active Scan`** on the site, select the `bee` user and Start the scan

![](.gitbook/assets/image-20230504163034249.png)

- In the `Alerts` tab check the 🚩`High` risk Alerts

![](.gitbook/assets/image-20230504172932311.png)

- Try to navigate to `https://192.192.29.3/htmli_stored.php`, inject the **`XSS`** (**Cross-site Scripting**) payload and Submit it
  - The XSS payload will be triggered

![](.gitbook/assets/image-20230504173742160.png)

![](.gitbook/assets/image-20230504173751062.png)

- Using the `ZAP HUD`, Site Alerts can be accessed. Every vulnerability is clickable and can be directly tried via the URL

![](.gitbook/assets/image-20230504174218018.png)

![](.gitbook/assets/image-20230504174327973.png)

![](.gitbook/assets/image-20230504174441116.png)

- Try a SQL Injection attack by opening this link
  - `http://192.210.141.3/sqli_1.php?action=search&title=ZAP'+OR+'1'%3D'1'+--+`
  - The table records will be dumped on the web page

![](.gitbook/assets/image-20230504175036074.png)

## Lab 2 - [Nikto](https://github.com/sullo/nikto)

> 🔬 [Scanning Web Application with Nikto](https://attackdefense.com/challengedetails?cid=2128)
>
> - Target IP: `192.157.60.3`
> - Scan and identify web app vulnerabilities (Multillidae II) with **Nikto**
>   - LFI

```bash
ip -br -c a
	eth1@if203734  UP  192.157.60.2/24

nmap -sS -sV 192.157.60.3
```

- Open the browser and navigate to
  - `http://192.157.60.3/`

### Nikto

- In the Bash terminal run **`nikto`** and output the results to a file

```bash
nikto -h http://192.157.60.3 -o niktoscan-192.157.60.3.txt
```

![nikto](.gitbook/assets/image-20230504182151396.png)

- Scan the target web app for **L**ocal **F**ile **I**nclusion (**LFI**) vulnerability by copying the link from the browser
  - `http://192.157.60.3/index.php?page=arbitrary-file-inclusion.php`
  - output to an `HTML` file

```bash
nikto -h http://192.157.60.3/index.php?page=arbitrary-file-inclusion.php -Tuning 5 -o nikto.html -Format htm
```

![IDOR - LFI](.gitbook/assets/image-20230504182309267.png)

```
firefox nikto.html
```

![](.gitbook/assets/image-20230504183330574.png)

![LFI](.gitbook/assets/image-20230504183356515.png)

- *The PHP-Nuke Rocket add-in is vulnerable to **file traversal**, allowing an attacker to view any file on the host*
  - View the contents of the `passwd` file of the target machine
  - `http://192.157.60.3/index.php/index.php?page=../../../../../../../../../../etc/passwd`

![](.gitbook/assets/image-20230504183457260.png)

------

