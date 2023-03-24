# 🔬Tshark, ARP, WiFi

## Lab 1

>  🔬 [Getting Started: Tshark](https://attackdefense.com/challengedetails?cid=1)
>
>  - **Tshark** usage

```bash
tshark -v
```

```bash
TShark (Wireshark) 2.6.1 (Git v2.6.1 packaged as 2.6.1-0ubuntu2~16.04.0)
```

> 📌 Running Tshark version is `2.6.1`

```bash
tshark -D
```

- These are all supported network interfaces for monitoring

![tshark -D](.gitbook/assets/image-20230324111823330.png)

- Sniff some traffic on `eth0`

```bash
tshark -i eth0
```

![tshark -i eth0](.gitbook/assets/image-20230324112011882.png)

> 📌 `student` user don't have permission to capture on `eth0` interface

- Display the packet list of the `.pcap` file

```bash
tshark -r HTTP_traffic.pcap
```

- Count `.pcap` file number of lines to find total number of packets

```bash
tshark -r HTTP_traffic.pcap | wc -l
	30418 # lines in the file
```

![tshark -r HTTP_traffic.pcap](.gitbook/assets/tshark_pcap.gif)

- Read first 100 packets from the `.pcap` file

```bash
tshark -r HTTP_traffic.pcap -c 100
```

- List the Protocol Hierarchy Statistics from the `.pcap` file

```bash
tshark -r HTTP_traffic.pcap -z io,phs -q
```

![tshark -r HTTP_traffic.pcap -z io,phs -q](.gitbook/assets/image-20230324113306740.png)

------

## Lab 2

>  🔬 [Filtering Basics: HTTP](https://attackdefense.com/challengedetails?cid=2)
>
>  - **Tshark** usage and filtering

### Filtering

- Show the `HTTP` traffic from a `.pcap` file

```bash
tshark -r HTTP_traffic.pcap -Y 'http'
tshark -r HTTP_traffic.pcap -Y 'http' | more
```

![tshark -r HTTP_traffic.pcap -Y 'http'](.gitbook/assets/image-20230324115801714.png)

- Show only the IP packets sent from IP address `192.168.252.128` to IP address `52.32.74.91`

```bash
tshark -r HTTP_traffic.pcap -Y "ip.src==192.168.252.128 && ip.dst==52.32.74.91"
```

![tshark -r HTTP_traffic.pcap -Y "ip.src==192.168.252.128 && ip.dst==52.32.74.91"](.gitbook/assets/image-20230324120102950.png)

- Print only packets containing GET requests

```bash
tshark -r HTTP_traffic.pcap -Y "http.request.method==GET"
```

![tshark -r HTTP_traffic.pcap -Y "http.request.method==GET"](.gitbook/assets/image-20230324120221796.png)

- Print only packets with frame time, source IP and URL for all GET requests

```bash
tshark -r HTTP_traffic.pcap -Y "http.request.method==GET" -Tfields -e frame.time -e ip.src -e http.request.full_uri
```

![tshark -r HTTP_traffic.pcap -Y "http.request.method==GET" -Tfields -e frame.time -e ip.src -e http.request.full_uri](.gitbook/assets/image-20230324120636235.png)

- Print packets containing a string

```bash
tshark -r HTTP_traffic.pcap -Y "http contains password"
```

![tshark -r HTTP_traffic.pcap -Y "http contains password"](.gitbook/assets/image-20230324120756645.png)

> 📌 `4` HTTP packets contain the `password` string

- Check the **destination IP** for `GET` requests sent to `www.nytimes.com`

```bash
tshark -r HTTP_traffic.pcap -Y "http.request.method==GET && http.host==www.nytimes.com" -Tfields -e ip.dst
```

![tshark -r HTTP_traffic.pcap -Y "http.request.method==GET && http.host==www.nytimes.com" -Tfields -e ip.dst](.gitbook/assets/image-20230324121036977.png)

> 📌 `170.149.159.130` is the destination IP of `www.nytimes.com`

- Check the **session ID** used by `192.168.252.128` for `amazon.in`

```bash
tshark -r HTTP_traffic.pcap -Y "ip contains amazon.in && ip.src==192.168.252.128" -Tfields -e ip.src -e http.cookie
```

![tshark -r HTTP_traffic.pcap -Y "ip contains amazon.in && ip.src==192.168.252.128" -Tfields -e ip.src -e http.cookie](.gitbook/assets/image-20230324121407664.png)

> 📌 `278-7381968-4337153` is the session ID

- Find the **OS** type on the machine with IP `192.168.252.128`

```bash
tshark -r HTTP_traffic.pcap -Y "ip.src==192.168.252.128 && http" -Tfields -e http.user_agent
```

![tshark -r HTTP_traffic.pcap -Y "ip.src==192.168.252.128 && http" -Tfields -e http.user_agent](.gitbook/assets/image-20230324121744533.png)

- Use the `User_agent` string to find the specific distribution

`Mozilla/5.0 (X11; Linux x86_64; rv:31.0) Gecko/20100101 Firefox/31.0 Iceweasel/31.8.0`

> 📌 The OS is `Linux x86_64; rv:31.0` - [user-agents.net](https://user-agents.net/string/mozilla-5-0-x11-linux-x86-64-rv-31-0-gecko-20100101-firefox-31-0-iceweasel-31-8-0)

------

## Lab 3 - ARP Poisoning

>  🔬 *INE Platform Lab*
>
>  - **ARP Poisoning** attack to a `telnet` server
>    - the client machine authenticates with the `telnet` server every 30sec
>  - Find the `telnet` login credentials
>  - Tools: **`arpspoof`** & **`Wireshark`**

- Monitor the traffic on `eth1` interface of the Kali Attacker machine.

```bash
ip -br -c a
	eth1  UP  10.100.13.140/24
```

### Enumeration

```bash
nmap 10.100.13.0/24
```

```bash
Nmap scan report for 10.100.13.1
    22/tcp   open  ssh
    3389/tcp open  ms-wbt-server
# ^^ Gateway machine, do not attack

Nmap scan report for 10.100.13.36
    22/tcp open  ssh
    23/tcp open  telnet
# ^^ Telnet server machine

Nmap scan report for 10.100.13.140
    3389/tcp open  ms-wbt-server
    5910/tcp open  cm
# ^^ Client machine
```

![nmap 10.100.13.0/24](.gitbook/assets/image-20230324125014246.png)

### ARP Poisoning Attack

- To see traffic of other machines, configure Kali Attacker VM to forward IP packets

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

- Start the ARP poisoning attack with the [**`arpspoof`**](https://www.kali.org/tools/dsniff/#arpspoof) tool

```bash
arpspoof -i eth1 -t 10.100.13.37 -r 10.100.13.36
# -t = target
# -r = host
```

- Open `Wireshark` and start capture `eth1` traffic
  - Apply `telnet` filter

![](.gitbook/assets/image-20230324130442438.png)

- Follow the **TCP stream** and find the `telnet` credentials
  - Stop the capture and the `arpspoof` tool

![](.gitbook/assets/image-20230324130614957.png)

> 📌 `Telnet` credentials are `admin`:`MyS3cr3tP455`

```bash
telnet 10.100.13.36
```

![](.gitbook/assets/2023-03-24_13-11.png)

------

## Lab 4 - WiFi Traffic Analysis

> 🔬 [WiFi Security: Traffic Analysis I](https://attackdefense.com/challengedetails?cid=1141)
>
> - **WiFi** basic traffic analysis with `Wireshark`

- Find the name of the Open SSID in the packet dump, using the filter
  - Beacon frame = `0x0008`
  - Wlan tag `48` = **[RSN-IE](https://mrncciew.com/2014/08/21/cwsp-rsn-information-elements/)** (Robust Security Network Information Element)

```bash
(wlan.fc.type_subtype == 0x0008) && (!(wlan.wfa.ie.wpa.version == 1)) &&
!(wlan.tag.number == 48)
```

![](.gitbook/assets/image-20230324134003319.png)

<details>
<summary>Reveal Flag - The name of the Open SSID is: 🚩</summary>



`SecurityTube_Open`

</details>



- Find the channel `Home_Network` is operating

```bash
wlan contains Home_Network
```

![](.gitbook/assets/image-20230324134539505.png)

<details>
<summary>Reveal Flag - Home_Network operates on channel: 🚩</summary>



`6`

</details>



- Find which security mechanism has `LazyArtists` SSID

```bash
wlan contains LazyArtists
```

![](.gitbook/assets/image-20230324134946480.png)

<details>
<summary>Reveal Flag - LazyArtists configured security is: 🚩</summary>



`WPA2-PSK`

</details>



- Check the **WPS** setup for `Amazon Wood` SSID

```bash
(wlan.ssid contains "Amazon") && (wlan.fc.type_subtype == 0x0008)
```

> 📌  WPS is enabled

![](.gitbook/assets/image-20230324142851202.png)

- Count the packets of the device with MAC `e8:de:27:16:87:18`
  - wlan.ta = transmitted
  - wlan.ra = received

```bash
(wlan.ta == e8:de:27:16:87:18)  ||  (wlan.ra == e8:de:27:16:87:18)
```

![](.gitbook/assets/image-20230324143309045.png)

<details>
<summary>Reveal Flag - Total packets number is: 🚩</summary>



`5701`

</details>



- Find a **specific MAC address** which exchange data with `SecurityTube_Open` SSID
  - `SecurityTube_Open` is hosted on BSSID `e8:de:27:16:87:18`

```bash
(wlan.bssid == e8:de:27:16:87:18) && (wlan.fc.type_subtype == 0x0020)
```

![](.gitbook/assets/image-20230324143952217.png)

<details>
<summary>Reveal Flag - The MAC address is: 🚩</summary>



`5c:51:88:31:a0:3b`

</details>



- Find TSF timestamp of the **Association Response** sent from the `SecurityTube_Open` access point to a station

```bash
((wlan.bssid == e8:de:27:16:87:18) && (wlan.addr==5c:51:88:31:a0:3b)) &&
(wlan.fc.type_subtype == 0x0001)
```

![](.gitbook/assets/image-20230324144333613.png)

<details>
<summary>Reveal Flag - The TSF timestamp is: 🚩</summary>



`115152625`

</details>

------

## Lab 5 - WiFi Traffic Filtering

> 🔬 [Filtering Advanced: WiFi](https://attackdefense.com/challengedetails?cid=4)
>
> - **Tshark** usage and filtering

- Show only WiFi traffic

```bash
tshark -r WiFi_traffic.pcap -Y "wlan"
```

![tshark -r WiFi_traffic.pcap -Y "wlan" | more](.gitbook/assets/image-20230324145113459.png)

- Show only the deauthentication packets
  - *Every management frame in WiFi has can be classified under a **type** and **subtype***
  - `wlan.fc.type_subtype == 0x000c` - AP sends deauthentication frames

```bash
tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==0x000c"
```

![tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==0x000c"](.gitbook/assets/image-20230324145545783.png)

- Show only the `WPA handshake` packets
  - **EAPoL** (Extensible Authentication Protocol over LAN) is used for WPA handshake

```bash
tshark -r WiFi_traffic.pcap -Y "eapol"
```

![tshark -r WiFi_traffic.pcap -Y "eapol"](.gitbook/assets/image-20230324145657998.png)

- Show only **SSID** and **BSSID** values of all beacon frames

```bash
tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==8" -Tfields -e wlan.ssid -e wlan.bssid
```

![tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==8" -Tfields -e wlan.ssid -e wlan.bssid](.gitbook/assets/image-20230324150035789.png)



- Check the BSSID of `LazyArtists` SSID

```bash
tshark -r WiFi_traffic.pcap -Y "wlan.ssid==LazyArtists" -Tfields -e wlan.bssid
```

![tshark -r WiFi_traffic.pcap -Y "wlan.ssid==LazyArtists" -Tfields -e wlan.bssid](.gitbook/assets/image-20230324150413400.png)

> 📌  `LazyArtists` BSSID is **`fc:b0:c4:91:71:e0`**.



- Show the channel on which `Home_Network` operates

```bash
tshark -r WiFi_traffic.pcap -Y "wlan.ssid==Home_Network" -Tfields -e wlan_radio.channel
```

![tshark -r WiFi_traffic.pcap -Y "wlan.ssid==Home_Network" -Tfields -e wlan_radio.channel](.gitbook/assets/image-20230324150612379.png)

> 📌  `Home_Network` operating channel is **`6`**.



- Show the two devices that received the deauth messages

```bash
tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==0x000c" -Tfields -e wlan.ra
```

![tshark -r WiFi_traffic.pcap -Y "wlan.fc.type_subtype==0x000c" -Tfields -e wlan.ra](.gitbook/assets/image-20230324150852601.png)

> 📌  The MAC address of the two devices are `6c:19:8f:5f:81:74` and `bc:ae:c5:c3:5e:01`.



- Check vendor and model of the device with MAC `5c:51:88:31:a0:3b`

```bash
tshark -r WiFi_traffic.pcap -Y "wlan.ta==5c:51:88:31:a0:3b && http" -Tfields -e http.user_agent
```

![tshark -r WiFi_traffic.pcap -Y "wlan.ta==5c:51:88:31:a0:3b && http" -Tfields -e http.user_agent](.gitbook/assets/image-20230324151147182.png)

> 📌 The device is a **Motorola MotoG3**.

------

