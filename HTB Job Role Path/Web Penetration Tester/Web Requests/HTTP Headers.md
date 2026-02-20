
---

# 🛰️ HTTP HEADERS: THE METADATA COMMAND CENTER 🛰️

## 1️⃣ Simple Summary

If an HTTP request is a letter, the **Headers** are the information written on the envelope and the sticky notes attached to it. They pass extra details between your computer and the server. Headers use a **"Name: Value"** format (example: `Host: google.com`). We group them into five main categories based on what they do:

- **General:** Basic info about the message itself
- **Entity:** Details about the actual file being sent (size, type).
- **Request:** Info from YOU to the server (who are you?).
- **Response:** Info from the SERVER to you (what software is it using?).
- **Security:** Rules that keep the connection safe.
    

## 2️⃣ Header Categories Explained

### 📂 General & Entity Headers (The Basics)
These describe the "package" being delivered.
- **Date:** Exactly when the message was sent.
- **Connection:** Keep the connection open (`keep-alive`) or close it (`close`).
- **Content-Type:** What is inside? (e.g., `text/html`, `application/pdf`).
- **Content-Length:** File size in bytes.
- **Content-Encoding:** Whether the data is compressed (e.g., `gzip`).
    
### 📥 Request Headers (Your Identity)
These tell the server about you.
- **Host:** The address you are trying to reach.
- **User-Agent:** Your digital fingerprint (Chrome, Firefox, cURL, etc.).
- **Referer:** The previous page you came from.
- **Cookie:** Your ID badge (session identifier).
- **Authorization:** Your secret key or access token.
    
### 📤 Response Headers (Server Details)
The server uses these to respond.
- **Server:** Shows what software is running (e.g., Apache, Nginx).
- **Set-Cookie:** Server giving you a session ID to store.
- **WWW-Authenticate:** Tells you what authentication method is required.
    

### 🛡️ Security Headers (The Shield)
These protect the browser.
- **Content-Security-Policy (CSP):** Only allow scripts from trusted sources. Prevents XSS (Cross-Site Scripting).    
- **Strict-Transport-Security (HSTS):** Forces browser to always use HTTPS, never HTTP.
    
## 3️⃣ Code & Tools: Headers in Action
### 🛠️ cURL Command Breakdown
```bash
curl -I https://inlanefreight.com
```
- **-I flag:** Sends a HEAD request (shows only response headers, no body content).
    
```bash
curl -i https://inlanefreight.com
```
- **-i flag:** Shows headers AND content together.
    
```bash
curl -H "X-Custom-Header: HackTheBox" https://inlanefreight.com
```
- **-H flag:** Adds a custom header to the request.
    
```bash
curl -A "HackerBrowser/1.0" https://inlanefreight.com
```
- **-A flag:** Changes the User-Agent value.
    

## 🖥️ Browser DevTools (Checking the Envelope)
![[devtools_network_requests_details.jpg]]
- Open DevTools (`F12`).
- Go to **Network** tab.
- Click a request.
- Open **Headers** sub-tab.
- Toggle **Raw** to see exact transmitted header text.
    

## 4️⃣ Visual Infographic (The Header Map)

```
[ CLIENT ]                                      [ SERVER ]
    |                                               |
    |  --- REQUEST HEADERS --->                     |
    |  "I am Chrome" (User-Agent)                   |
    |  "Here is my ID" (Cookie)                     |
    |  "I want HTML" (Accept)                       |
    |                                               |
    |                                        [ SERVER CHECKS ]
    |                                        (Is the ID valid?)
    |                                               |
    |  <--- RESPONSE HEADERS ---                    |
    |  "I am running Linux/Apache" (Server)         |
    |  "The file is 500 bytes" (Content-Length)     |
    |  "ONLY USE HTTPS!" (Security-Policy)          |
    |                                               |
[ BROWSER READS RULES ]                             |
```

## 5️⃣ Technical Terms Simplified
- **Contextual:** Depends on the situation (example: timestamp).
- **Metadata:** Data about data (headers describe the content).
- **Enumeration:** Methodically collecting information to find weaknesses.
- **Charset:** Text encoding system (usually UTF-8).
- **Boundary:** Separator used when uploading multiple files in one request.
    

