# iptables

The command `iptables` is a firewall tool like `ufw`.

* List all rules with verbosity and line numbers.

```
root@kali:~# iptables -L -v --line-numbers
```

* Add a rule, here the rule <mark style="color:yellow;">DROP</mark> all <mark style="color:yellow;">INPUT</mark> entries for port <mark style="color:yellow;">22</mark> and protocol <mark style="color:yellow;">TCP</mark> from the address <mark style="color:yellow;">192.168.81.90</mark>.

```
root@kali:~# iptables -A INPUT -p tcp -s 192.168.81.90 --dport 22 -j DROP
```

* Remove one or more rules, to specify one rule `INPUT/OUTPUT/FORWARD` and the number of the rule.

```
root@kali:~# iptables -D INPUT 1
root@kali:~# iptables -F
```

* Persist the configuration from memory into a file.

```
root@kali:~# iptables-save > /etc/iptables/rules.v6
root@kali:~# iptables-save > /etc/iptables/rules.v4
```

* Load the configuration from a file into memory.

```
root@kali:~# iptables-restore < /etc/iptables/rules.v6
root@kali:~# iptables-restore < /etc/iptables/rules.v4
```

***

* [https://www.hostinger.com/tutorials/iptables-tutorial](https://www.hostinger.com/tutorials/iptables-tutorial)
* [https://www.cyberciti.biz/faq/how-to-save-iptables-firewall-rules-permanently-on-linux/](https://www.cyberciti.biz/faq/how-to-save-iptables-firewall-rules-permanently-on-linux/)
