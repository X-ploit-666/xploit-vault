# 🎭 Intercepting & Changing Server Responses

### What is Response Interception?

Usually, a website sends data to your browser, and the browser just shows it. **Response Interception** lets you catch the website's answer _before_ your browser renders it.

**Why do we do this?**

- To change how a page looks or behaves.
    
- To turn on "Disabled" buttons that you aren't supposed to click.
    
- To change "Hidden" fields into visible ones so you can see secret data.
    
- To bypass browser-side limits (like character counts or number-only rules).
    

### 🟠 Using Burp Suite

Burp doesn't stop responses by default, so you have to turn it on.

1. **Enable the Rule:** Go to `Proxy` -> `Proxy settings`. Look for `Response interception rules` and check the box to enable it.
    
2. **The Flow:**
    
    - Turn on Intercept (the main button).
        
    - Refresh the page in your browser (`CTRL+SHIFT+R`).
        
    - Click **Forward** on the Request.
        
    - **Stop!** Now you are looking at the **Response** (the HTML code of the site).
        
3. **The Edit:** Find the code for the input box.
    
    - Change `type="number"` to `type="text"`.
        
    - Change `maxlength="3"` to `maxlength="100"`.
        
4. **Result:** Click **Forward**. Your browser now thinks the website _always_ allowed text and long sentences!
    

### 🔵 Using OWASP ZAP

ZAP makes this very simple with the "Step" feature.

1. **The Step Button:** When you catch a request, don't click the big Play button. Click the **Step** button (looks like a small arrow pointing to a line).
    
2. **The Flow:** ZAP sends the request and immediately catches the response for you to edit.
    
3. **The HUD "Magic" Button:** ZAP has a shortcut for lazy (efficient!) testers.
    
    - Look for the **Light Bulb icon** on the left side of your browser.
        
    - Click it, and ZAP automatically finds every hidden or disabled button on the page and turns it on for you. No manual code editing is required!
        

### 💻 Code Breakdown: The HTML Modification

In the exercise, we are looking for a specific line of HTML code that controls the "Ping" box.

**The Original Code:**

```
<input type="number" id="ip" name="ip" min="1" max="255" maxlength="3">
```

- `type="number"`: The browser will block you if you try to type letters.
    
- `maxlength="3"`: You can't type more than 3 characters (e.g., "127" is fine, but ";ls -la" is too long).
    

**The Modified Code:**

```
<input type="text" id="ip" name="ip" min="1" max="255" maxlength="100">
```

- `type="text"`: Now the browser allows letters, symbols, and spaces.
    
- `maxlength="100"`: Now we have plenty of room for our hack commands.
    

### 🗺️ Visual Explanation: The Response Loop

```
[ SERVER ] 
    |
    | (Sends the "Locked" Webpage)
    v
[ PROXY TOOL (Burp/ZAP) ] <--- YOU ARE HERE
    |                          (You unlock the code)
    |                          (Change "number" to "text")
    v
[ YOUR BROWSER ] <--- Browser receives the "Unlocked" version
    |                 (You can now type anything you want!)
    v
[ SUCCESS ]
```

### 💡 Extra Tricks: Comments & Hidden Fields

- **Hidden Fields:** Some sites store data in "hidden" boxes. You can use the Proxy to change `type="hidden"` to `type="text"` to see what's inside.
    
- **Comments:** Developers often leave notes in the code like `<!-- Admin password is Password123 -->`.
    
    - In **ZAP HUD**, click the **Comments (+)** button to see these notes pop up as icons on the screen without looking at the source code.