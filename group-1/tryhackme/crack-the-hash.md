# Crack the Hash

Analyze all hashes with https://www.tunnelsup.com/hash-analyzer/ and `hashid`.

```bash
kali@kali> echo '48bb6e862e54f2a795ffc4e541caed4d' > hash-1
kali@kali> hashcat -a 0 -m 0 hash-1 /usr/share/wordlists/rockyou.txt
```

```bash
kali@kali> echo 'CBFDAC6008F9CAB4083784CBD1874F76618D2A97' > hash-2
kali@kali> hashcat -a 0 -m 100 hash-2 /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
```

```bash
kali@kali> echo '1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032'> hash-3
kali@kali> hashcat -a 0 -m 1400 hash-3 /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
```

Go to the url https://hashes.com/en/decrypt/hash for the hash `$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom`.

Go to the url https://md5decrypt.net/en/Md4/ for the hash `279412f945939ba78ce0758d3fd83daa`.

```bash
kali@kali> echo 'F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85' > hash-6
kali@kali> hashcat -a 0 -m 1400 hash-6 /usr/share/wordlists/rockyou.txt
```

Go to the url https://hashes.com/en/decrypt/hash for the hash `$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.`.

Go to the url https://hashes.com/en/decrypt/hash for the hash `e5d8870e5bdd26602cab8dbe07a942c8669e56d6`.
