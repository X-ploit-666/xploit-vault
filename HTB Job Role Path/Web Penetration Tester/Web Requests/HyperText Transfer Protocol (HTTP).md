
<iframe 
    src="https://x-ploit-666.github.io/xploit-vault/http-protocol.html"
    style="
        width: 100%;
        height: 700px;
        border: 1px solid #2563EB;
        border-radius: 8px;
        background-color: #f0f9ff;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    "
></iframe>


---

# 💀 HTTP PROTOCOL & URL BREAKDOWN 💀

---

## 🌐 1. Simple Summary

Most modern apps on phones and computers constantly communicate over the internet.  
They use something called **HTTP (HyperText Transfer Protocol)**.

**HTTP** = a set of rules that computers follow to request and send data over the web.
**Hypertext** = text that contains links and is structured so humans can navigate it easily.
When HTTP communication happens, there are two main sides:

### 🖥 Client
- This is **you**.
- Your browser or app sends a request.
- It asks for a **resource** (web page, image, file, API data).
    
### 🗄 Server
- A powerful remote computer.
- Stores websites and data.
- Receives requests and sends responses back.
    
Communication usually happens through:
- **Port 80** → Default port for HTTP
- If configured differently, another port number may be used.
    
To access a website, we use a:
- **URL (Uniform Resource Locator)** → Full web address
- Also called **FQDN (Fully Qualified Domain Name)**
    

Example:
```
www.hackthebox.com
```
---

## 🔍 2. URL Anatomy (Address Breakdown)
A URL is a structured address. Each part has meaning.
![[url_structure.png]]

|Part|Meaning|Example|
|---|---|---|
|**Scheme**|Protocol being used|`http://` or `https://`|
|**User Info**|Login credentials (username:password)|`admin:password@`|
|**Host**|Server name or IP address|`inlanefreight.com`|
|**Port**|Server “door” number|`:80`|
|**Path**|Specific file or folder|`/dashboard.php`|
|**Query String**|Extra data sent to page|`?login=true`|
|**Fragment**|Jump to specific section|`#status`|

### ⚠ Important
Minimum required parts:
- **Scheme**
- **Host**
    
Everything else is optional.

---

## 🔄 3. How Data Flows (Step‑by‑Step)
![[HTTP_Flow 1.png]]
### 1️⃣ DNS Lookup
You type:
```
inlanefreight.com
```
Your computer does not know its IP address.
So it asks:

**DNS (Domain Name System)**  
→ The internet's phonebook  
→ Converts domain names into IP addresses

Example:
```
inlanefreight.com → 1.2.3.4
```

#### 🧠 Pro Tip
Your system first checks:

```
/etc/hosts
```
If the mapping exists there, it skips DNS.

---
### 2️⃣ HTTP Request

Once the IP is known:
The browser sends a **GET request** to:
```
IP address : Port 80
```

A **GET request** = “Please give me this resource.”

---

### 3️⃣ Server Processing
If no file is specified, the server sends:
```
index.html
```
This is the default homepage file.

---

### 4️⃣ HTTP Response
The server replies with:
- Requested file
- **Status Code**
    
Example:
```
200 OK
```

Meaning: Request successful.
---

### 5️⃣ Browser Rendering
The browser:
- Reads HTML
- Loads CSS
- Executes JavaScript
- Displays the website
    
---
## 🛠 4. Code & Tools: cURL

Normal users → Browser (Chrome, Firefox)
Security testers → **cURL**

**cURL (client URL)**  
Command-line tool used to send HTTP requests manually.

---
## 💻 Code Extraction & Explanation
---
### Basic Request

```bash
curl inlanefreight.com
```

**What it does:**
- Sends an HTTP request.
- Prints raw HTML in terminal.
- Does NOT render it like a browser.
    
---
### Download File (Keep Original Name)

```bash
curl -O inlanefreight.com/index.html
```

**-O flag**
- Saves file using original filename from server.
    
---
### Download File (Custom Name)

```bash
curl -o my_page.html inlanefreight.com/index.html
```

**-o flag**
- Allows custom filename.

---
### Silent Mode
```bash
curl -s -O inlanefreight.com/index.html
```
**-s flag**

- Silent mode.
- Hides progress and extra output.
    

---
### Help Menu

```bash
curl -h
```

Shows all available options.

---
## 🧠 5. Visual Concept Map

```
[ CLIENT ]
    |
    | 1. "Where is inlanefreight.com?"
    v
[ DNS SERVER ]
    |
    | 2. "IP = 1.2.3.4"
    v
[ CLIENT ]
    |
    | 3. HTTP GET (Port 80)
    v
[ WEB SERVER ]
    |
    | 4. HTTP 200 OK + index.html
    v
[ CLIENT ]
    |
    v
[ BROWSER RENDERS WEBSITE ]
```

---
## 🔎 URL Structure Diagram
```
https://admin:pass@website.com:80/folder/file?id=1#top
│       │           │          │     │         │
│       │           │          │     │         └─ Fragment
│       │           │          │     └────────── Query
│       │           │          └──────────────── Path
│       │           └─────────────────────────── Port
│       └─────────────────────────────────────── Host
└─────────────────────────────────────────────── Scheme
```

---

