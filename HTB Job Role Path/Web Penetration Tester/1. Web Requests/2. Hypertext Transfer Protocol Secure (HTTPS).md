<iframe 
    src="https://x-ploit-666.github.io/xploit-vault/https.html"
    style="
        width: 100%;
        height: 700px;
        border: 1px solid #2563EB;
        border-radius: 8px;
        background-color: #f0f9ff;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    "
></iframe>


## Simple Summary

In the last section, we learned about **HTTP**. Its main problem: it sends data in **clear-text**.
![[https_clear.png]]
- **Clear-text** = anyone between you and the website can read your passwords or messages, like reading a postcard.
- This risk is called a **Man-in-the-Middle (MiTM) Attack**.
    
**HTTPS (HTTP Secure)** fixes this by **encrypting** your data:
![[https_google_enc.png]]
- It "scrambles" the information before sending it.
- If a hacker intercepts it, all they see is random gibberish.
    
Today:
- Almost every website uses HTTPS.
- Browsers will soon block old HTTP sites.
    
---

## 2️⃣ HTTPS Overview: Clear-Text vs. Encrypted

### HTTP (The Risk)
- Your login travels exactly as typed.
- On public Wi-Fi, a hacker can "listen" and steal it.
    
### HTTPS (The Shield)
- Login details become a **single encrypted stream**.
- Looks like a long string of random symbols.
    
**How to spot HTTPS:**
- URL starts with `https://`
- Lock icon 🔒 next to the address in the browser
    ![[https_google.png]]
⚠ **Crucial Note:**  
Even with HTTPS, your browser might reveal the website name if you use **clear-text DNS**.
- Use **Encrypted DNS** or a **VPN** to hide it completely.
    

---

## 3️⃣ The HTTPS Flow (The "Handshake")
When you visit a secure site, a handshake happens in milliseconds:
1. **The Redirect**
    - Typing `http://` triggers a server redirect (`301 Moved Permanently`) to **Port 443**.
2. **Client Hello**
    - Browser says: "Hi, here’s the encryption I can handle."
3. **Server Hello & Key Exchange**
    - Server replies with its **SSL Certificate** (digital ID card).
4. **Verification**
    - Browser checks certificate validity.
    - Secret keys are exchanged.
5. **Encrypted Handshake**
    - They test the connection using secret keys.
6. **Secure Communication**
    - All further data is encrypted and safe.
        
---

## 4️⃣ Code & Tools: cURL for HTTPS
cURL automatically handles HTTPS but is strict about certificate safety
### 🛠 Code Examples & Explanations
`curl https://inlanefreight.com`
- **What it does:** Connects to a secure site.
- **Error check:** If the certificate is fake, expired, or invalid → cURL stops with **SSL certificate problem**.
    
`curl -k https://inlanefreight.com
- **-k flag:** "Insecure" (ignore certificate warnings)
- **Why use it:** Penetration testers on local servers or lab environments use it when certificates are not properly set up.
    

---
## 5️⃣ Visual Flow Diagram (The Secure Handshake)

![[HTTPS_Flow.png]]
---

## 6️⃣ Technical Terms Simplified

- **Encryption:** Scrambling data so only the right person can read it.
- **SSL Certificate:** Digital passport proving a website’s identity.
- **MiTM Attack:** Hacker spying in the middle of a conversation.
- **Handshake:** Initial meeting where computers agree on security.
- **Downgrade Attack:** Hacker tries to force old, weak HTTP instead of HTTPS.
    

---
