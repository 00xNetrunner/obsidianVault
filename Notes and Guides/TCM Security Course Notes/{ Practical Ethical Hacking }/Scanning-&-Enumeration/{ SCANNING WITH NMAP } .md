---
sticker: lucide//eye
tags:
  - TCM-Security
  - Ethical_Hacking
---
NMAP Scans work in 3 stages, `SYN SYNACK ACK`
NMAP Reaches out to a network and attempts to make a connection with the ports the first scan we are going to do is called a stealth scan. 

> Just because its called stealth doesn't mean its stealthy anymore, if a network you are scanning has decent security it will notice the scan, but saying this most companies don't have the best security. 

a stealth scan works similar to `SYN SYNACK ACK` 

a stealth scan will work like `SYN SYNACK RST`, **RST** stands for restart, so when nmap finds this open port it cancels the connection. but it knows the port is open. 

lets beggin with our first scan. 
```bash
nmap -T4 -p- -A 192.168.122.171 -vv
```
**Breakdown**
- `-T4` - sets the speed of an nmap scan T5 being the highest.
- `-p-` - scans all ports on target machine
- `-A` - Gather host information
- `-vv` - very verbose