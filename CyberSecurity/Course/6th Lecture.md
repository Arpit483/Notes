# Nmap

Used for scanning networks
```
nmap localhost
```

command is used to scan your localhost and services running on it.
EX : *Start a apache service and run it on port (default 80) set to 8088 and with TCP open*
[[Ports]]
By default the ip for local host  is **127.0.0.1**
---
ip - internet protocol
---

```
nmap -sV localhost
```

-sV = command used for detecting or understanding what service is running on port number **also tells the version**
Tells us the exact service running on a specific port
Ex : Here apache service is running which is shown in the scan with the name of the port.

Now we start a SSh service using apache
```
sudo systemctl start ssh
```


![[Pasted image 20241024173410.png]]


The service is running
run
```
nmap -sV localhost
```
we can see ssh port available on port 22 / tcp the port state and service is shown


![[Pasted image 20241024173537.png]]


Now we try to run some scans on websites that are safe to scan
Ex : http://zero.webappsecurity.com
```
nmap zero.webappsecurity.com
```
by default nmap scans first 1000 ports 

![[Pasted image 20241024174111.png]]

```
nmap -p80,443,8080 -sV zero.webappsecurity.com
```

**-p** : used to scan specific ports
**-sV** : used to enumurate service on the specific port 
![[Pasted image 20241024174454.png]]


When FireWall is detected 
![[Pasted image 20241024174606.png]]
tcp ports are filtered


Nmap has different scripts scans 
to view 
```
cd usr/share/nmap/scripts
```

**Script Scans are dedicated scans used to identify Venerability's in a website**

```
nmap -sC -p80,443,8080 -sV zero.webappsecurity.com
```

**-Sc** : Used for default script scan.
 ==-sC: equivalent to --script=default==

![[Pasted image 20241024175713.png]]

*When this is shown it shows that the site used API of RestfulApi*

[[Ciphers]]

> [!NOTE]
> Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-24 08:22 EDT
> Nmap scan report for zero.webappsecurity.com (54.82.22.214)
> Host is up (0.21s latency).
> rDNS record for 54.82.22.214: ec2-54-82-22-214.compute-1.amazonaws.com
> 
> PORT     STATE SERVICE  VERSION
> 80/tcp   open  http     Apache Tomcat/Coyote JSP engine 1.1
> | http-methods: 
> |_  Potentially risky methods: PUT DELETE TRACE PATCH
> |_http-title: Zero - Personal Banking - Loans - Credit Cards
> 443/tcp  open  ssl/http Apache httpd 2.2.6 ((Win32) mod_ssl/2.2.6 OpenSSL/0.9.8e mod_jk/1.2.40)
> | sslv2: 
> |   SSLv2 supported
> |   ciphers: 
> |     SSL2_RC4_128_EXPORT40_WITH_MD5
> |     SSL2_DES_64_CBC_WITH_MD5
> |     SSL2_RC4_128_WITH_MD5
> |     SSL2_DES_192_EDE3_CBC_WITH_MD5
> |     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
> |_    SSL2_RC2_128_CBC_WITH_MD5
> |_http-server-header: Apache/2.2.6 (Win32) mod_ssl/2.2.6 OpenSSL/0.9.8e mod_jk/1.2.40
> | http-methods: 
> |_  Potentially risky methods: TRACE
> |_ssl-date: 2024-10-24T12:23:31+00:00; +8s from scanner time.
> | ssl-cert: Subject: commonName=zero.webappsecurity.com/organizationName=Micro Focus LLC/stateOrProvinceName=California/countryName=US
> | Subject Alternative Name: DNS:zero.webappsecurity.com
> | Not valid before: 2021-04-26T00:00:00
> |_Not valid after:  2022-05-04T23:59:59
> |_http-title: Site doesn't have a title (text/html).
> 8080/tcp open  http     Apache Tomcat/Coyote JSP engine 1.1
> |_http-title: Zero - Personal Banking - Loans - Credit Cards
> | http-methods: 
> |_  Potentially risky methods: PUT DELETE TRACE PATCH
> |_http-server-header: Apache-Coyote/1.1
> 
> Host script results:
> |_clock-skew: 7s
> 
> Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
> Nmap done: 1 IP address (1 host up) scanned in 28.63 seconds


```
 nmap -p80,443,8080 -sV --script vuln zero.webappsecurity.com

```

Used to do a specific vulnerability scan

```
in cd of nmap
ls| grep vuln
```

to find all vulnerabilities

Now in real world approach 
```
 nmap -p80,443,8080 -sV --script vuln -v -oN vuln zero.webappsecurity.com
```

**-v** : for a detailed report (it show report in real time.)
**-v**v : for a highly detailed report
**-vvv** : for a highly highly detailed report

**-oN** : used to save the report in a text file

This command will create a detailed report on port scanned an there vulnerabilities and save it in a file. 


```
nmap -Pn -p- -vv zero.webappsecurity.com
```

used to scan ports 
**-Pn** : tells the nmap that don't go and ping the ip so that the firewall detects us 
Tells us various Ports

```
sudo nmap -p80 -O zero.webappsecurity.com
```

used to define os of server which the website is running on


![[Pasted image 20241024183116.png]]
it shows that the firewall is running and blocking the namp















