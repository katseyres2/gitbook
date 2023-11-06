# ffuf

* Simple enumeration.

```bash
ffuf -vcw /tmp/wordlist:FUZZ -u http://127.0.0.1/FUZZ
```

* Host enumeration.

```bash
ffuf -w /tmp/wordlist:FUZZ -u http://127.0.0.1 -H "Host: FUZZ" -mc 200
```

* Recursive enumeration.

```bash
ffuf -w /tmp/wordlist:FUZZ -u http://127.0.0.1/FUZZ -maxtime-job 60 -recursion -recursion-depth 2
```

* File extension enumeration.

```bash
ffuf -w /tmp/wordlist:FUZZ -u http://159.65.54.124:31621/blog/indexFUZZ
# if the extension is php
ffuf -w /tmp/wordlist:FUZZ -u http://159.65.54.124:31621/blog/FUZZ.php
```

* File enumeration with `.php` extension.

```bash
ffuf -vcw /tmp/wordlist:FUZZ -u http://127.0.0.1/FUZZ -e .php
```
