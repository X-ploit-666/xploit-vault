# Nibbles

# Info
**IP** 10.10.10.75
**Difficulty** Easy
**OS** Ubuntu
**Ports** 
- 22   ---- OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0) ---
- 80    ---- Apache httpd 2.4.18 ((Ubuntu)) ---
**CVE-2015-6967**
**TAGS** #Apache #WEB #Upload #BruteForce #Fuzizing #php #sudo #Metasploit #UnFiinished #CVE #ReverseShell 
---

# CVE-2015-6967 Summary
- **SUMMARY** 
	- Enumeration ---> Default OR Weak login credentials ---> Upload php WebShell ---> RevShell  ---> priv-esc

---

# Recon
1. Found Hidden path **`/nibbleblog/`** in the source view main page of http://10.10.10.75/

2. **Fuzzing** http://nibleblog.com/nibbleblog/FUZZ
	- **Found** **`README`** Directory by fuzzing 
		- **Web App** Nibbleblog
		- **Version**  v4.0.3
		- **CodeName**  Coffe
		- **SubDomains Found** http://blog.nibbleblog.com

3. **Enumerating The New Blog** http://blog.nibbleblog.com
	- **Fuzzing The New subdomain** http://blog.nibbleblog.com/nibbleblog
		- Found **`/Content`** --> http://blog.nibbleblog.com/nibbleblog/content/private/user.xml  & other interesting Directories 
			- Found 
				- **username** admin
				- **email** admin@nibbles.com

---

# IPPsec Video Walkthrough 
[IPPsec Manual Walkthough](https://youtu.be/s_0GcRGv6Ds)

---

# PDF WalkThough
![[Nibbles.pdf]]