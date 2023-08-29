---
cover: https://academy.hackthebox.com/storage/modules/21/logo.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
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

# Introduction to Bash Scripting

## Introduction

```bash
#!/bin/bash
​
# check for given arguments
if [ $# -eq 0 ]
then
    echo -e "You need to specify the target domain.\n"
    echo -e "Usage:"
    echo -e "\t$0 <domain>"
    exit 1
elif [ $# -eq 1 ]
then
    domain=$1
else
    echo -e "Too many arguments given."
fi
​
value=$1
​
if [ $value -gt "10" ]
then
    echo "Given argument is greater than 10."
elif [ $value -lt "10" ]
then
    echo "Given argument is less than 10."
else
    echo "Given argument is not a number."
fi
​
var="nef892na9s1p9asn2aJs71nIsm"
​
for counter in {1..40}
do
    var=$(echo $var | base64)
    
    if [ $counter -eq "35" ]
    then
        echo $var|wc -m
    fi
done
​
echo "Number of arguments       : $#"
echo "Array of arguments        : $@"
echo "First argument            : $1"
echo "Process ID                : $$"
echo "Exit status of the script : $?"
​
variable="Declared without an error."
echo $variable
​
domains=(www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com www2.inlanefreight.com)
​
#for counter in ${domains[@]}
#do
#   echo ${domains[$counter]}
#done
```

## Branches

```bash
#!/bin/bash
​
echo -e "Additional options available:"
echo -e "\t1) Identify the corresponding network range of target domain."
echo -e "\t2) Ping discovered hosts."
echo -e "\t3) All checks."
echo -e "\t*) Exit.\n"
​
read -p "Select your option: " opt
​
case $opt in
    "1") network_range;;
    "2") ping_host;;
    "3") network_range && ping_host;;
    "*") exit 0;;
esac
```

## Arithmetic

```bash
#!/bin/bash
​
increase=1
decrease=1
​
echo "Addition       : 10 + 10 = $((10 + 10))"
echo "Substraction   : 10 - 10 = $((10 - 10))"
echo "Multiplication : 10 * 10 = $((10 * 10))"
echo "Division       : 10 / 10 = $((10 / 10))"
echo "Modulus        : 10 % 10 = $((10 % 10))"
​
((increase++))
echo "Increase variable: $increase"
​
((decrease--))
echo "Decrease variable: $decrease"
​
htb="HackTheBox"
echo "Length of '$htb' : ${#htb}"
```

## Comparison

```bash
#!/bin/bash
​
if [ "$1" != "HackTheBox" ]
then
    echo -e "You need to give 'HackTheBox' as argument."
    exit 1
elif [ $# -gt 1 ]
then
    echo -e "Too many arguments given."
    exit 1
else
    domain=$1
    echo -e "Success!" 
fi
​
​
if [ $# -lt 1 ]
then
    echo -e "Number of given arguments is less than 1"
    exit 1
elif [ $# -gt 1 ]
then
    echo -e "Number of given arguments is greater than 1"
    exit 1
else
    domain=$1
    echo -e "Number of given arguments equals 1"
fi
​
​
if [ -e "$1" ]
then
    echo -e "The file exists."
    exit 0
else
    echo -e "The file does not exist."
    exit 2
fi
​
​
if [[ -z $1 ]]
then
    echo -e "Boolean value: True (is null)"
    exit 1
elif [[ $# > 1 ]]
then
    echo -e "Boolean value: True (is greater than)"
    exit 1
else
    domain=$1
    echo -e "Boolean value: False (is equal to)"
fi
​
​
if [[ -e "$1" && -r "$1" ]]
then
    echo -e "We can read the file that has been specified."
    exit 0
elif [[ ! -eq "$1" ]]
then
    echo -e "The specified file does not exist."
    exit 2
elif [[ -e "$1" && ! -r "$1" ]]
then
    echo -e "We don't have read permission for this file."
    exit 1
else
    echo -e "Error occured."
    exit 5
fi
```

## Functions

```bash
#!/bin/bash
​
function print_pars {
    echo $1 $2 $3
}
​
one="First parameter"
two="Second parameter"
three="Third parameter"
​
print_pars "$one" "$two" "$three"
​
# Return values
# 1     : General errors
# 2     : Misuse of shell builtins
# 126       : Command invoked cannot execute
# 127       : Command not found
# 128       : Invalid argument to exit
# 128+n     : Fatal error signal "n"
# 130       : Script terminated by Control-C
# 255\*     : Exit status out of range
​
function given_args {
    if [ $# -lt 1 ]
    then
        echo -e "Number of arguments: $#"
        return 1
    else
        echo -e "Number of arguments: $#"
        return 0
    fi
}
​
given_args
echo -e "Function status code: $?\n"
​
given_args "argument"
echo -e "Function status code: $?\n"
​
content=$(given_args "argument")
echo -e "Content of the variable:\n\t$content"
```

## Input Control

```bash
#!/bin/bash
​
function network_range {
    for ip in $ipaddr
    do
        netrange=$(whois $ip|grep "NetRange\|CIDR"|tee -a "CIDR.txt")
        cidr=$(whois $ip|grep "CIDR"|awk '{print $2}')
        cidr_ips=$(prips $cidr)
        echo -e "\nNetRange for $ip:"
        echo -e "$netrange"
    done
}
​
$hosts=$(host $domain|grep "has address"|cut -d" " -f4|tee "discovered_hosts.txt")
```

## Loops

```bash
#!/bin/bash
​
​
for var in 1 2 3 4
do
    echo $var
done
​
​
for var in file1 file2 file3
do
    echo $var
done
​
​
#for ip in "10.10.10.170 10.10.10.174 10.10.10.175"
#do
#   ping -c 1 $ip
#done
​
​
# inline
#for up in 10.10.10.170 10.10.10.174;do ping -c 1 $ip;done
​
host="127.0.0.1"
stat=1
while [ $stat -eq 1 ]
do
    ping -c 2 $host >/dev/null 2>&1
    if [ $? -eq 0 ]
    then
        echo "$host is up."
        ((stat--))
        ((host_up++))
        ((hosts_total++))
    else
        echo "$host is down."
        ((stat--))
        ((hosts_total++))
    fi
done
​
​
counter=0
while [ $counter -lt 10 ]
do
    ((counter++))
    echo "Counter: $counter"
    if [ $counter == 2 ]
    then
        continue
    elif [ $counter == 4 ]
    then
        break
    fi
done
​
​
counter=0
until [ $counter -eq 10 ]
do
    ((counter++))
    echo "Counter: $counter"
done
```

## Exercises

### Exercise 1

```bash
#!/bin/bash
​
var="8dm7KsjU28B7v621Jls"
value="ERmFRMVZ0U2paTlJYTkxDZz09Cg"
​
for i in {1..40}
do
        var=$(echo $var | base64)
        hasValue=$(echo $var|grep -c "$value")
        
    
    if [[ $hasValue -gt 0 && $(echo $var|wc -m) -gt "113450" ]]
    then
        echo $var|tail -c 20
    fi
done
```

### Exercise 2

```bash
#!/bin/bash
​
# Decrypt function
function decrypt {
    MzSaas7k=$(echo $hash | sed 's/988sn1/83unasa/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/4d298d/9999/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/3i8dqos82/873h4d/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/4n9Ls/20X/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/912oijs01/i7gg/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/k32jx0aa/n391s/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/nI72n/YzF1/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/82ns71n/2d49/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/JGcms1a/zIm12/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/MS9/4SIs/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/Ymxj00Ims/Uso18/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/sSi8Lm/Mit/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/9su2n/43n92ka/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/ggf3iunds/dn3i8/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/uBz/TT0K/g')
​
    flag=$(echo $MzSaas7k | base64 -d | openssl enc -aes-128-cbc -a -d -salt -pass pass:$salt)
}
​
# Variables
var="9M"
salt=""
hash="VTJGc2RHVmtYMTl2ZnYyNTdUeERVRnBtQWVGNmFWWVUySG1wTXNmRi9rQT0K"
​
# Base64 Encoding Example:
#        $ echo "Some Text" | base64
​
# <- For-Loop here
​
for i in {1..28}
do
    var=$(echo $var|base64)
    salt=$(echo $var|wc -m)
    echo "salt $i: $salt"
done
​
# Check if $salt is empty
if [[ ! -z "$salt" ]]
then
    decrypt
    echo $flag
else
    exit 1
fi
```
