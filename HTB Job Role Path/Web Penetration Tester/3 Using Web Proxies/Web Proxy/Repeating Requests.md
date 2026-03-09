# ⚡ [SYSTEM] :: REQUEST REPEATING PROTOCOL

### 🔍 THE PROBLEM: MANUAL OVERHEAD

When you "break" a site with a command like `;ls;`, you often want to try 100 other commands (like `whoami`, `pwd`, or `cat /etc/passwd`).

- **The Slow Way:** Go to browser -> Type input -> Intercept in Proxy -> Edit -> Forward -> View Browser. (5-6 steps)
    
- **The Fast Way:** Use the **Repeater** or **Request Editor**. (1 step)
    

### 📂 STEP 1: LOCATING THE TARGET (HISTORY)

Before you can repeat a request, you have to find it in your logs.

- **BURP SUITE:** Go to `Proxy` > `HTTP History`.
    
    - _Pro Tip:_ Burp lets you toggle between the **Original Request** and the **Edited Request** so you can see exactly what you changed earlier.
        
- **OWASP ZAP:** Check the `History` tab at the bottom of the main UI or the bottom pane in the HUD.
    
- **Filtering:** Both tools let you sort or filter. Use this to find your target IP address quickly if your history is cluttered with background traffic.
    

### 🔄 STEP 2: FIRING THE REPEATER

#### 🟢 BURP SUITE: THE REPEATER

1. Find the request in your History.
    
2. Press `CTRL+R` (Sends it to the Repeater tab).
    
3. Press `CTRL+SHIFT+R` (Switches your view to the Repeater tab).
    
4. **Action:** Change the payload in the text box (e.g., change `ip=;ls;` to `ip=;whoami;`).
    
5. Click **Send**. The response appears instantly on the right.
    

#### 🔵 OWASP ZAP: REQUEST EDITOR

1. Right-click the request in History.
    
2. Select `Open/Resend with Request Editor`.
    
3. **Action:** Edit the text directly in the window.
    
4. Click **Send**.
    
5. **HUD Method:** Click the request in the HUD History pane. Choose **Replay in Console** to see the raw text or **Replay in Browser** to see the visual page.
    

### 💻 CODE ANALYSIS: THE REPEATER PAYLOAD

In the tool's editor, you will see the raw data. This is where you perform your "surgery."

**Original Captured Request:**

```
POST /ping HTTP/1.1
Content-Type: application/x-www-form-urlencoded

ip=;ls;
```

**What the Code does in Repeater:**

- You can manually delete `;ls;` and type `;cat /etc/passwd;`.
    
- You don't need to refresh the website.
    
- You don't need to bypass browser "number-only" rules because you are talking directly to the server.
    
- **Method Swapping:** You can right-click and "Change Request Method" to turn a `POST` into a `GET`. This tests if the server is lazy with how it receives data.
    

### 🗺️ VISUAL FLOW: REPEATER VS. MANUAL

```
[ MANUAL FLOW - SLOW 🐢 ]
BROWSER -> INTERCEPT -> EDIT -> FORWARD -> BROWSER (Repeat for every command)

[ REPEATER FLOW - FAST 🚀 ]
          +---------------------------+
          |      PROXY HISTORY        |
          +-------------+-------------+
                        |
            [ SEND TO REPEATER (CTRL+R) ]
                        |
          +-------------v-------------+
          |      REPEATER WINDOW      | <---+
          +-------------+-------------+     |
          |  1. Edit Payload          |     | (Instant Loop)
          |  2. Click 'Send'          |     |
          |  3. View Response         | ----+
          +---------------------------+
```

### ⚠️ IMPORTANT: URL ENCODING

Notice how the data in your `POST` request might look weird (like `%20` for spaces). This is **URL Encoding**.

- If you want to send a command with spaces, like `;ls -la;`, you usually need to encode it so the server understands it.
    
- **Burp Tip:** Highlight your text and press `CTRL+U` to encode it automatically.