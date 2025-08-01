
**Date** : 2025-07-30
**Link** :   https://tryhackme.com/room/snortchallenges2
**Description** : Put your snort skills into practice and defend against a live attack


# Brute Force
## Answer the questions below

First of all, start Snort in sniffer mode and try to figure out the attack source, service and port.  
  

Then, write an IPS rule and run Snort in IPS mode to stop the brute-force attack. Once you stop the attack properly, you will have the flag on the desktop!

Here are a few points to remember:

- Create the rule and test it with "-A console" mode. 
- Use **"-A full"** mode and the **default log path** to stop the attack.
- Write the correct rule and run the Snort in IPS "-A full" mode.
- Block the traffic at least for a minute and then the flag file will appear on your desktop.  
    

Stop the attack and get the flag (which will appear on your Desktop)

![[Pasted image 20250730214414.png]]

![[Pasted image 20250730214707.png]]

**target ip :** 10.10.140.29:22
**Malicious ip :** 10.10.245.36

What is the name of the service under attack?
Ans : SSH ( See in first image)

What is the used protocol/port in the attack?
Ans: TCP/22

To get the flag edit the /etc/snort/snort.conf and add the rule below

Rule : `drop tcp 22 <> any any (msg:"Attack" ; sid:1000001 ; rev:1;)`

```shell
sudo snort -c /etc/snort/snort.conf -q -Q --daq afpacket -i eth0:eth1 -A full
```

[[2. Snort]]

# Reverse Shell

## Answer the questions below

First of all, start Snort in sniffer mode and try to figure out the attack source, service and port.  
  

Then, write an IPS rule and run Snort in IPS mode to stop the brute-force attack. Once you stop the attack properly, you will have the flag on the desktop!

Here are a few points to remember:

- Create the rule and test it with "-A console" mode. 
- Use "-A full" mode and the default log path to stop the attack.
- Write the correct rule and run the Snort in IPS "-A full" mode.
- Block the traffic at least for a minute and then the flag file will appear on your desktop.

Stop the attack and get the flag (which will appear on your Desktop)

