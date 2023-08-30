# Weak RSA

Download the file : https://app.hackthebox.com/challenges/6 Try to crack the public key with the Python tool [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool?ref=technicalciso.com).

```bash
git clone https://github.com/RsaCtfTool/RsaCtfTool.git
python3 RsaCtfTool.py --publickey id_rsa.pub --private
```

```bash
echo '-----BEGIN RSA PRIVATE KEY-----
MIICOQIBAAKBgQMwO3kPsUnaNAbUlaubn7ip4pNEXjvUOxjvLwUhtybr6Ng4undL
tSQPCPf7ygoUKh1KYeqXMpTmhKjRos3xioTy23CZuOl3WIsLiRKSVYyqBc9d8rxj
NMXuUIOiNO38ealcR4p44zfHI66INPuKmTG3RQP/6p5hv1PYcWmErEeDewKBgGEX
xgRIsTlFGrW2C2JXoSvakMCWD60eAH0W2PpDqlqqOFD8JA5UFK0roQkOjhLWSVu8
c6DLpWJQQlXHPqP702qIg/gx2o0bm4EzrCEJ4gYo6Ax+U7q6TOWhQpiBHnC0ojE8
kUoqMhfALpUaruTJ6zmj8IA1e1M6bMqVF8srlb/NAiBhwngxi+Cbie3YBogNzGJV
h10vAgw+i7cQqiiwEiPFNQJBAYXzr5r2KkHVjGcZNCLRAoXrzJjVhb7knZE5oEYo
nEI+h2gQSt1bavv3YVxhcisTVuNrlgQo58eGb4c9dtY2blMCQQIX2W9IbtJ26KzZ
C/5HPsVqgxWtuP5hN8OLf3ohhojr1NigJwc6o68dtKScaEQ5A33vmNpuWqKucecT
0HEVxuE5AiBhwngxi+Cbie3YBogNzGJVh10vAgw+i7cQqiiwEiPFNQIgYcJ4MYvg
m4nt2AaIDcxiVYddLwIMPou3EKoosBIjxTUCQQCnqbJMPEQHpg5lI6MQi8ixFRqo
+KwoBrwYfZlGEwZxdK2Ms0jgeta5jFFS11Fwk5+GyimnRzVcEbADJno/8BKe
-----END RSA PRIVATE KEY-----' > id_rsa
```

```bash
openssl pkeyutl -decrypt -inkey id_rsa -in flag.enc -out flag.dec
cat flag.dec
```

The flag is `HTB{s1mpl3_Wi3n3rs_4tt4ck}`.
