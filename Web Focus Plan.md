
| Day |                        Lab/Challenge                        |                    Focus/Goal                     |                      Notes                      |
| :-: | :---------------------------------------------------------: | :-----------------------------------------------: | :---------------------------------------------: |
|  1  |              HTTP Basics (PortSwigger Academy)              | Understand requests, responses, cookies, sessions |    Very important to understand web traffic     |
|  2  |                      Burp Suite Basics                      |    Practice intercepting and modifying traffic    |       Install Burp, intercept login forms       |
|  3  |              SQL Injection (PortSwigger Labs)               |              Basic SQLi exploitation              |          Focus on manual exploitation           |
|  4  |      Authentication Vulnerabilities (PortSwigger Labs)      |         Weak login, bypass authentication         |              Practice logic flaws               |
|  5  |           IDOR (Insecure Direct Object Reference)           |             Access other users' data              |             Very common in bounties             |
|  6  |                 File Upload Vulnerabilities                 |          Bypass file upload restrictions          |                Upload PHP shells                |
|  7  |                     Directory Traversal                     |             Access unauthorized files             |      Simple payloads (`../../etc/passwd`)       |
|  8  |              Cross-Site Scripting (XSS) Basics              |               Stored, reflected XSS               |              Essential bounty bug               |
|  9  |                 Advanced XSS (PortSwigger)                  |                      DOM XSS                      |              Learn finding JS bugs              |
| 10  |              CSRF (Cross-Site Request Forgery)              |                 Forging requests                  |             Understand CSRF tokens              |
| 11  |                       Open Redirects                        |          Redirect user to malicious site          |             Easy bug for fast money             |
| 12  |               Logic Flaws (PortSwigger Labs)                |                Breaking app flows                 |          Examples: price manipulation           |
| 13  |                      JWT Attack Basics                      |              JWT token manipulation               |            Changing roles and userID            |
| 14  |                   CORS Misconfigurations                    |             Misusing CORS for access              |            Find CORS issues in APIs             |
| 15  |                     Web Cache Poisoning                     |                   Cache attack                    |              Target public content              |
| 16  |          SSRF Basics (Server Side Request Forgery)          |             Make server send requests             |              Internal IP discovery              |
| 17  |                  File Inclusion (LFI/RFI)                   |          Local and Remote File Inclusion          |            Combine with upload bugs             |
| 18  |              XXE (XML External Entity) Attacks              |            XML parsing vulnerabilities            |          Practice reading server files          |
| 19  |                      Basic API Testing                      |            API enumeration and testing            |             Bounties love API bugs!             |
| 20  |                       GraphQL Hacking                       |         GraphQL introspection, data leaks         |            More apps use GraphQL now            |
| 21  |                     Rate Limit Testing                      |           Finding broken rate limiting            |            Spam, brute force bypass             |
| 22  |                    Password Reset Flaws                     |               Reset flow weaknesses               |              Hijack other accounts              |
| 23  |                  Subdomain Takeover Basics                  |           Takeover abandoned subdomains           |           Good bug for bounty hunting           |
| 24  |                    Broken Access Control                    |                 Bypass user roles                 |          "Low privilege to admin" bugs          |
| 25  |        Bug Bounty Report Reading (HackerOne Reports)        |               Read 10 real reports                |    Understand how real hackers write reports    |
| 26  |               Hacker101 CTF Challenges (easy)               |     Practice real CTF-style bounty challenges     |          Free challenges by HackerOne           |
| 27  |              Recon Basics (Amass + Sublist3r)               |                  Find subdomains                  |             Recon = hidden targets              |
| 28  |                     Parameter Tampering                     |         Bypass controls with URL changes          |                 Manual testing                  |
| 29  |                  Open Bug Bounty Practice                   |             Try simple public targets             |                Play safe & legal                |
| 30  | Pick a small Public Program on HackerOne and start testing! |              Apply what you learned               | Time to start your real bounty hunting journey! |