

<iframe 
    src="https://x-ploit-666.github.io/xploit-vault/webrecon-intro.html"
    style="
        width: 100%;
        height: 700px;
        border: 1px solid #2563EB;
        border-radius: 8px;
        background-color: #f0f9ff;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    "
></iframe>


# 💀 WEB RECONNAISSANCE: THE TERMINAL GUIDE

![[PT-process.png]]

## 🌐 WHAT IS IT?


Web Reconnaissance is the very first step in checking a website’s security. It is like a detective doing "Information Gathering" before a case. This process is systematic (follows a plan) and meticulous (very careful with details). You collect information about a target website to prepare for deeper analysis or exploitation (finding and using weaknesses).

## 🎯 THE 4 PRIMARY GOALS

1. **Identifying Assets:** Finding everything the target owns online. This includes web pages, subdomains (like `blog.website.com`), IP addresses (the digital address of a computer), and the technologies they use.
    
2. **Discovering Hidden Information:** Finding things the owner didn't mean to show. This includes backup files, setup files (configuration files), or internal manuals. These are "entry points" for an attack.
    
3. **Analyzing the Attack Surface:** Looking at the "Attack Surface" (all the places where an outsider can try to get in) to find weak spots or mistakes in how things are set up.
    
4. **Gathering Intelligence:** Collecting data like email addresses or employee names. This is used for "Social Engineering"—which is tricking people into giving up secrets.
    

> **Why do it?** Attackers use this to pick the best way to break in. Defenders use it to find and fix holes before they get hurt.

## 🛠️ THE TWO METHODS OF RECON

### 1. ⚡ ACTIVE RECONNAISSANCE

In this method, you **directly touch** the target system. It gives great information but is "High Risk" because security systems (like Firewalls or Intrusion Detection Systems) will notice you.

|TECHNIQUE|WHAT IT DOES|SIMPLE EXAMPLE|RISK|
|---|---|---|---|
|**Port Scanning**|Checks which "doors" (ports) are open.|Using `Nmap` to see if the web door (Port 80) is open.|**HIGH**|
|**Vulnerability Scanning**|Looks for known bugs or old software.|Running `Nessus` to see if the site has common flaws.|**HIGH**|
|**Network Mapping**|Draws a map of how computers connect.|Using `Traceroute` to see the path data takes.|**MED-HIGH**|
|**Banner Grabbing**|Asking a service "Who are you?"|Looking at a "Banner" to see the server version.|**LOW**|
|**OS Fingerprinting**|Guessing the Operating System.|Checking if the server runs Windows or Linux.|**LOW**|
|**Service Enumeration**|Finding exact software versions.|Checking if the site runs Apache 2.4.|**LOW**|
|**Web Spidering**|A bot "crawls" every link on the site.|Using `Burp Suite` to find every hidden page.|**LOW-MED**|

### 2. 🕵️ PASSIVE RECONNAISSANCE

This involves gathering info **without touching** the target. You look at what is already public. It is very "stealthy" (hard to catch) but might not give you the full picture.

- **Search Engine Queries:** Using Google or "Shodan" (a search engine for connected devices) to find info.
    
- **WHOIS Lookups:** Checking a public database to see who bought the domain name and their contact info.
    
- **DNS Analysis:** Checking the "Phonebook of the Internet" (DNS) to find subdomains or mail servers. Tools: `dig` or `nslookup`.
    
- **Web Archive Analysis:** Using the "Wayback Machine" to see what the website looked like in the past.
    
- **Social Media Analysis:** Looking at LinkedIn or Twitter to learn about employees and their jobs.
    
- **Code Repositories:** Searching sites like GitHub to see if developers left secrets or passwords in their code.
    

## 💻 CODE ANALYSIS

The paragraph mentions specific commands and tools. Here is how they work:

### 1. Nmap OS Detection

`nmap -O [target]`

- **What it does:** Sends specific data packets to the target.
    
- **How it works:** Different systems (Windows vs Linux) respond in slightly different ways. Nmap compares these responses to a list to guess the OS.
    

### 2. Nmap Service Detection

`nmap -sV [target]`

- **What it does:** Grabs the version number of software.
    
- **How it works:** It talks to the open ports and asks the software for its "ID card."
    

### 3. DNS Lookup

`dig [domain_name]`

- **What it does:** Asks a DNS server for records.
    
- **How it works:** It returns the IP address and other infrastructure details (like where emails go) without ever contacting the website itself.
    

## 📊 THE RECON FLOW (VISUAL)

```
[ START ]
    |
    v
+---------------------------+       +---------------------------+
|   PASSIVE RECON (Silent)  |       |   ACTIVE RECON (Loud)     |
|   "Look but don't touch"  |       |   "Knock on the doors"    |
+-------------+-------------+       +-------------+-------------+
              |                                   |
              | Search Engines                    | Port Scanning
              | Social Media                      | Vuln Scanning
              | WHOIS / DNS                       | Web Spidering
              v                                   v
+---------------------------------------------------------------+
|                      COLLECTED INTELLIGENCE                   |
| (IPs, Subdomains, Emails, Software Versions, Vulnerabilities) |
+-------------------------------+-------------------------------+
                                |
                                v
                   [ TARGETED ATTACK / DEFENSE ]
```

**Summary:** Recon is the foundation. You start quiet (Passive) to stay hidden, then move to direct checks (Active) to get the final details