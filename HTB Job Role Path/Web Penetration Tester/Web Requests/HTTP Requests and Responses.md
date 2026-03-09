


# 🌐 HTTP Requests & Responses Breakdown

---

## 1️⃣ Simple Summary

Every time you use the internet, a conversation happens between:
- **Client** → Your computer / browser
- **Server** → A remote computer hosting the website
    
This conversation has two main parts:
### 📤 The Request
Your computer asks for something.  
Example: a webpage, an image, or data.

### 📥 The Response
The server processes that request and sends back:
- The file you asked for
- Or an error message
    

---
## 2️⃣ The HTTP Request (The Ask)

When your browser sends a request, the **first line is the most important**.
Example:
```
GET /users/login.html HTTP/1.1
```
![[raw_request 1.png]]
### 🔎 Breaking It Down

|Field|Simple Explanation|Example|
|---|---|---|
|**Method**|The action word. What do you want to do?|`GET` (Give me the file)|
|**Path**|The location of the file on the server|`/users/login.html`|
|**Version**|The HTTP rules being used|`HTTP/1.1`|

---
### 🏷 After the First Line → Headers
Headers send extra details about the request.
Common ones:
```
Host: hackthebox.com
User-Agent: Chrome
Cookie: sessionid=abc123
```
- **Host** → Which website you’re talking to
- **User-Agent** → What browser/tool you’re using
- **Cookie** → Secret ID tags that help the site remember you
    
---
## 3️⃣ The HTTP Response (The Answer)
When the server replies, the first line tells you the result.
Example:
```
HTTP/1.1 200 OK
```
![[raw_response.png]]
### 🔎 Breaking It Down
- **Version** → Matches request version
- **Response Code** → Status number
- **Message** → Human-readable meaning
    
Examples:
- `200 OK` → Success
- `401 Unauthorized` → Not allowed
- `404 Not Found` → Doesn’t exist
    
---

### 🏷 Response Structure
Just like a request, the response contains:
- **Headers**
- **Body**
    
Headers might include:
```
Content-Type: text/html
Date: Tue, 18 Feb 2026
```

The **Body** is the real content:
- HTML → Builds the website
- JSON → Data format
- Image / PDF → Raw file data
    
---
## 4️⃣ Code & Tools — Inspecting the Conversation
Developers and hackers don’t just see the final website.
They inspect the **raw conversation**.

---

### 🛠 curl Command
```
curl inlanefreight.com -v
```
**What it does:**  
Sends a request and shows verbose (chatty) output.
### Symbol Meaning:
- `>` → Request (what your computer sent)
- `<` → Response (what the server sent back)
- `*` → Behind-the-scenes connection details
    
---
### Extra Verbose Mode
```
curl -vvv inlanefreight.com
```
Shows deeper technical details about:
- Connection
- TLS handshake
- Data transfer
    
---
## 🖥 Browser DevTools (F12)
Your browser has a built-in inspection tool.
![[devtools_network_requests.jpg]]
### How to Use:
1. Press `F12` or `CTRL + SHIFT + I`
2. Go to the **Network** ta
3. Refresh the page
    
You’ll see every request made.
You can inspect:
- Raw headers
- Raw response
- Unrendered code (before it becomes a styled webpage)
    

## 5️⃣ Visual Breakdown (Flow)

```
[ CLIENT ]                                      [ SERVER ]
    |                                               |
    | (1) REQUEST                                   |
    | GET /index.html HTTP/1.1  ------------------> |
    | Host: hackthebox.com                          | [ PROCESSING ]
    | User-Agent: Chrome                            | (Finds file...)
    |                                               |
    | (2) RESPONSE                                  |
    | <------------------  HTTP/1.1 200 OK          |
    |                      Content-Type: text/html  |
    |                      [ HTML DATA STARTS ]     |
    |                                               |
[ SHOWS WEBSITE ] <--- (User sees page)
```

---

## 6️⃣ Technical Terms Simplified
### Clear-Text
Text that is not encrypted.  
Easy to read. Easy to steal.

---
### Binary Data
Data sent as 1s and 0s.  
Used in HTTP/2 for better speed.

---

### Method / Verb
The command sent to the server.  
Examples: `GET`, `POST`.

---

### Verbose (`-v`)
Tells a tool:
> "Show me everything you’re doing."
---

### Body
The actual content of the message.  
Could be:
- HTML
- JSON
- Image
- PDF
    
---
