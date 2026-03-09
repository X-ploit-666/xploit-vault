# 🛠️ Intercepting & Manipulating Web Requests

### What is Interception?

**Interception** is like a "pause button" for the internet. When you click a button on a website, the message (request) stops at your proxy tool first. It stays there until you decide to look at it, change it, or let it go.

**Why do we do this?**

- To see what the website is hiding from you.
    
- To bypass "rules" set by the browser (like fields that only let you type numbers).
    
- To test for security holes like **SQL Injection** or **XSS**.
    

### 🟢 Using Burp Suite

In Burp, the pause button is usually **ON** by default.

1. **Find the toggle:** Go to `Proxy` -> `Intercept`.
    
2. **The Button:** Look for the `Intercept is on/off` button.
    
3. **The Flow:**
    
    - Open the browser inside Burp.
        
    - Visit your target site.
        
    - Burp will "catch" the request. It will glow or pop up.
        
    - Click **Forward** to send that specific message to the website.
        
    - _Note:_ You might have to click Forward a few times if your browser is sending background "noise" requests.
        

### 🔵 Using OWASP ZAP

In ZAP, the pause button is **OFF** by default.

1. **The Green/Red Button:** Look for a green circular button in the top bar. Green means "traffic flows freely." Click it to turn it **Red** (Interception ON).
    
2. **Shortcut:** Press `CTRL+B`.
    
3. **The HUD (Heads Up Display):** This is a special overlay that appears _inside_ your browser.
    
    - Click the HUD icon in ZAP's top bar to enable it.
        
    - Use the icons on the left side of your browser screen to pause traffic.
        
    - **Step:** Send the current message and pause at the next one.
        
    - **Continue:** Let all remaining messages go through.
        

### 💻 Code Breakdown: The "Ping" Request

When you click a "Ping" button on a website, the browser sends a secret message called an **HTTP POST Request**.

**The Original Code:**

```
POST /ping HTTP/1.1
Host: 94.237.62.138:32306
Content-Type: application/x-www-form-urlencoded

ip=1
```

**What this code means:**

- `POST /ping`: This tells the server we are sending data to the "/ping" folder.
    
- `Host`: The address of the computer we are talking to.
    
- `Content-Type`: Tells the server the data is formatted like a web form.
    
- `ip=1`: This is the actual data. The website thinks we are just asking it to ping IP address "1".
    

### ⚡ The Manipulation (The Hack)

The website might have a rule in the browser: _"You can only type numbers here!"_ By using the proxy, we ignore the browser's rules. We change the data **after** it leaves the browser but **before** it hits the server.

**The Manipulated Data:** Change `ip=1` to `ip=;ls;`

**What happened?**

1. `;` (Semicolon): In computer commands, this means "Finish the last job and start a new one."
    
2. `ls`: This is a command that tells a computer to "List all files in this folder."
    
3. **The Result:** The server was told to "Ping nothing, then show me all your private files." Because the server didn't double-check our input, it obeyed! This is called **Command Injection**.
    

### 🗺️ Visual Explanation

```
[ 1. BROWSER ] -----> [ 2. PROXY TOOL ] -----> [ 3. WEB SERVER ]
   (User clicks)         (Message Paused)         (Processes Code)
         |                      |                        |
         |                      v                        |
         |              YOU CHANGE THE DATA              |
         |              From: "ip=1"                     |
         |              To:   "ip=;ls;"                  |
         |                      |                        |
         \--------------------->/                        |
                                |                        |
                                \----------------------->/
                                           |
                                           v
                                [ SERVER SENDS BACK ]
                                [ LIST OF PRIVATE FILES ]
```

### 📝 Summary of Key Terms

- **Forward/Step:** Let the current message go to the server.
    
- **Drop:** Delete the message so the server never sees it.
    
- **HUD:** A control panel that appears on top of the website you are testing.
    
- **Validation:** When a server checks if your data is "safe." (In our example, the server failed to do this).