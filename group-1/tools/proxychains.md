# Proxychains

## Sources

* [https://www.offsec.com/metasploit-unleashed/proxytunnels/](https://www.offsec.com/metasploit-unleashed/proxytunnels/)
* [https://www.offsec.com/metasploit-unleashed/pivoting/](https://www.offsec.com/metasploit-unleashed/pivoting/)

## Tools

* MSF modules
  * auxiliary/scanner/ssh/ssh\_login
  * post/multi/manage/autoroute
  * auxiliary/server/socks\_proxy
  * auxiliary/scanner/portscan/tcp
* proxychains
* ssh

## Description

Before watching this video, let's explain the goal.

I have 2 networks and 3 hosts

* Networks
  * 192.168.81.0/24
  * 192.168.10.0/24
* Hosts
  * attacker : 192.168.81.62
  * exposed : 192.168.81.86 & 192.168.10.3
  * internal : 192.168.10.2

The **attacker** is my Kali Linux.\
The **exposed** is the Ubuntu server I want use for pivoting from 192.168.81.0 to 192.168.10.0.\
The **internal** is the host which is only available on the subnet 192.168.10.0.

1. I'm connecting **attacker** to **exposed** through SSH with msfconsole.
2. I add a route to the subnet 192.168.10.0.
3. I create a socks proxy with the MSF module.
4. I set the **proxychains** configuration file.
5. I get **internal** SSH connection through the socks proxy.

## Video

{% embed url="https://youtu.be/jsidupKaaKE" %}
