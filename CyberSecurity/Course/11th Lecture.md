
## THM Room -> Source

https://tryhackme.com/r/room/source

1. Nmap scan of the machine
```
nmap -F -sV -Pn -vv -oN report.txt "ip of machine"
```   

![[Pasted image 20241102164031.png]]
Running a searchsploit scan on webmin we got vuln report


2. Now Scan port 10000 for vulnerabilities
   ```
   nmap -p10000 -sV --script vuln -vv -oN report.txt "ip"
	```


now use msfconsole for metaexploit
(suggested by rapid7.com) > msf

R Host - mentioning ip address of source (machine to be hacked) in msf console is called ....

L Host - mentioning kali ip address for source to provide kali info

set LHOSTS and RHOSTS in msfconsole 

type exploit command


it will gain control of the machine / server 
become root
```
id
```
to check weather root or not 
*by default root*

make terminal user friendly by firefox extension
navigate through files and complete the task
