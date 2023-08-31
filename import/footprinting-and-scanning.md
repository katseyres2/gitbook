# Footprinting and scanning

## Learning objectives

* Students will know purpose of network mapping and port scanning with reference to an engagement.
* Students will perform network host discovery.
* Students will perform host port scanning.

## Mapping a network

### Physical access

* Physical security
* OSINT
* Social engineering

### Sniffing

* Passive reconnaissance
* Watch network traffic

### ARP

It means Address Resolution Protocol. It resolves IP to MAC.

Who has 10.10.1.5 ? (tells 10.10.1.7) 10.10.1.5 is at 00:0c:29:af:ea:d2 ARP table

### ICMP

It means Internet Control Message Protocol. You can `traceroute` or `ping` with this protocol.

PING : the echo request.

### Tools

* Wireshark : GUI
* ARP-scan : `sudo arp-scan -I tap0 -g 10.142.111.0/24`
* ping : `ping 10.142.111.213`
* fping : `fping -I tap0 -g 10.124.111.0/24 -a 2>/dev/null`
* nmap : GUI
* zenmap : GUI (like nmap)

The ARP-scan shows the IP addresses and the MAC addresses. Sometimes, the `arp-scan` command shows up IP addresses but not the `ping` command because the machines can block the ICMP requests. On the other hand, `ping` could reveal IP addresses that the `arp-scan` doesn't find.

## Port scanning

* Identify operating system
* Identify services

It's possible to identify OS with 2 ways :

* Revealed by Signatures
* Revealed by Services

```
john@kali$ nmap -iL ips
```

The tools are :

* Zenmap
* NMAP Automator
* Mascan
* Rustscan
* Autorecon

## Scan the server

```bash
john@kali$ ip a
john@kali$ nmap 192.206.172.3 -p 1-250              # range to find opened ports
john@kali$ nmap 192.206.172.3 -p 177 -A             # all information on this port
john@kali$ nmap 192.206.172.3 -p 1-250 -sU          # check UDP ports
john@kali$ nmap 192.206.172.3 -p -134,177,234 -sUV  # Service UDP Version
john@kali$ nmap 192.206.172.3 -p 134 -sUVC          # script default
john@kali$ nmap 192.206.172.3 -p 134 -sUV --script=discovery  # discovery script
john@kali$ tftp 192.206.172.3 -p 134                # hosting the TFTP server
john@kali$ 
```
