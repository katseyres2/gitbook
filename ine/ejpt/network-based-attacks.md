# Network-based attacks

### Part 1

Network services : `ARP` `DHCP` `SMB` `FTP` `Telnet` `SSH` Man in the Middle :&#x20;

<figure><img src="../../.gitbook/assets/64.png" alt=""><figcaption></figcaption></figure>

### Tshark

```bash
tshark -D # list interfaces
tshark -i eth0
tshark -r HTTP_traffic.pcap
tshark -r HTTP_traffic.pcap -z io,phs -q  # hierarchy statistics
```

### Filtering basics

```bash
# 1. Command to show only the IP packets sent from IP address 192.168.252.128 to IP address 52.32.74.91?
tshark -r HTTP_traffic.pcap 'ip.src==192.168.252.128 && ip.dst==52.32.74.91'

# 2. Command to print only packets containing GET requests?
tshark -r HTTP_traffic.pcap 'http.request.method==GET'

# 3. Command to print only packets only source IP and URL for all GET request packets?
tshark -r HTTP_traffic.pcap -T fields -e ip.src -e ip.dst 'http.request.method==GET'

# 4. What is the destination IP address for GET requests sent for New York Times (www.nytimes.com)?
tshark -r HTTP_traffic.pcap 'dns'|grep www.nytimes.com
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET && http.host==www.nytimes.com' -T fields -e ip.dst

# 5. What is the session ID being used by 192.168.252.128 for Amazon India store (amazon.in)?
tshark -r HTTP_traffic.pcap -Y 'ip.src==192.168.252.128 && ip contains amazon.in' -T fields -e ip.dst -e http.cookie

# 6. What type of OS the machine on IP address 192.168.252.128 is using (i.e. Windows/Linux/MacOS/Solaris/Unix/BSD)? Bonus: Can you also guess the distribution/flavor?
tshark -r HTTP_traffic.pcap -Y 'ip.src==192.168.252.128 && http' -T fields -e ip.dst -e http.user_agent

# 7. How many HTTP packets contain the "password" string?
tshark -r HTTP_traffic.pcap -Y 'http contains password'
```

### ARP poisoning

```
```

### WiFi traffic analysis

1. What is the name of the Open (No Security) SSID present in the packet dump?

<figure><img src="../../.gitbook/assets/66.png" alt=""><figcaption></figcaption></figure>

2. The SSID 'Home\_Network' is operating on which channel?

<figure><img src="../../.gitbook/assets/65.png" alt=""><figcaption></figcaption></figure>

3. Which security mechanism is configured for SSID 'LazyArtists'? Your options are: OPEN, WPA-PSK, WPA2-PSK.

<figure><img src="../../.gitbook/assets/69.png" alt=""><figcaption></figcaption></figure>

4. Is WiFi Protected Setup (WPS) enabled on SSID 'Amazon Wood'? State Yes or No.
5. What is the total count of packets which were either transmitted or received by the device with MAC e8:de:27:16:87:18?

<figure><img src="../../.gitbook/assets/68.png" alt=""><figcaption></figcaption></figure>

6. What is the MAC address of the station which exchanged data packets with SSID 'SecurityTube\_Open'?

<figure><img src="../../.gitbook/assets/67.png" alt=""><figcaption></figcaption></figure>

7. From the last question, we know that a station was connected to SSID 'SecurityTube\_Open'. Provide TSF timestamp of the association response sent from the access point to this station.

<figure><img src="../../.gitbook/assets/70.png" alt=""><figcaption></figcaption></figure>

### Filtering advanced

1. Which command can be used to show only WiFi traffic?

```
tshark -r WiFi_traffic.pcap 'wlan'
```

2. Which command can be used only view the deauthentication packets?

```
tshark -r WiFi_traffic.pcap 'wlan.fc.type_subtype eq 12'
```

3. Which command can be used to only display WPA handshake packets?

```
tshark -r WiFi_traffic.pcap 'wlan.fc.type_subtype == 0x08 || wlan.fc.type_subtype == 0x05 || eapol'
```

4. Which command can be used to only print the SSID and BSSID values for all beacon frames?

```
tshark -r WiFi_traffic.pcap -T fields -e wlan.ssid -e wlan.bssid 'wlan.fc.type_subtype == 0x08'
```

5. What is BSSID of SSID “LazyArtists”?

```
tshark -r WiFi_traffic.pcap -T fields -e wlan.bssid 'wlan.ssid contains LazyArtists'
```

6. SSID “Home\_Network” is operating on which channel?

```
tashark -r Wifi_traffic.pcap -Y 'wlan.ssid=Home_Network' -T fields -e wlan_radio.channel
```

7. Which two devices received the deauth messages? State the MAC addresses of both.

```
tshark -r WiFi_traffic.pcap -T fields -e wlan.bssid 'wlan.fc.type_subtype eq 12'
```

8. Which device does MAC 5c:51:88:31:a0:3b belongs to? Mention manufacturer and model number of the device.

```
tshark -r 'WiFi_traffic.pcap' -Y 'wlan.bssid==5c:51:88:31:a0:3b' -T fields -e user_agent
```