# ⚡ AUTOMATIC TRAFFIC MANIPULATION

### 🛠️ WHAT IS AUTOMATIC MODIFICATION?

Manual interception is slow. **Automatic Modification** (Match and Replace) acts like a "Find and Replace" tool for your internet traffic. You set a rule once, and the proxy tool automatically swaps out data in every message that passes through it.

**Why use this?**

- **Bypass Filters:** Automatically change your "User-Agent" (the ID card that says what browser you are using) to bypass blocks.
    
- **Permanent UI Unlocks:** Automatically change "number" fields to "text" fields so they stay unlocked even when you refresh the page.
    

### 🟢 BURP SUITE: MATCH AND REPLACE

#### 1. Changing your "User-Agent" (Request Header)

To make the server think you are using a custom "HackTheBox" browser:

- **Path:** `Proxy` > `Proxy settings` > `HTTP match and replace rules` > `Add`.
    
- **Settings:**
    
    - **Type:** `Request header` (because User-Agent lives in the header).
        
    - **Match:** `^User-Agent.*$` (This is **Regex**; it means "find the line starting with User-Agent and select everything to the end").
        
    - **Replace:** `User-Agent: HackTheBox Agent 1.0`.
        
    - **Regex match:** `True`.
        

#### 2. Unlocking Fields (Response Body)

To make the "Ping" box accept text automatically:

- **Type:** `Response body` (the actual HTML of the page).
    
- **Match:** `type="number"`.
    
- **Replace:** `type="text"`.
    
- **Regex match:** `False` (we know the exact text, so no regex needed).
    

### 🔵 OWASP ZAP: REPLACER

ZAP uses a tool called the **Replacer** (`CTRL+R`).

- **Description:** `HTB User-Agent`.
    
- **Match Type:** `Request Header`.
    
- **Match String:** `User-Agent` (ZAP has a helpful dropdown for this).
    
- **Replacement String:** `HackTheBox Agent 1.0`.
    
- **Initiators:** Set to `Apply to all HTTP(S) messages`.
    

### 💻 CODE LOGIC: REGEX EXPLAINED

In the Burp example, we used a **Regex (Regular Expression)**.

**The Code:** `^User-Agent.*$`

|Part|Meaning|
|---|---|
|`^`|Start at the beginning of the line.|
|`User-Agent`|Look for these exact characters.|
|`.*`|Match **any** character (`.`) that appears **any** number of times (`*`).|
|`$`|Stop at the end of the line.|

**How it works:** It grabs the entire identity string of your browser (which can be very long and messy) and swaps it for your clean "HackTheBox" version.

### 🛰️ VISUAL DATA FLOW (AUTO-MODE)

```
  [ USER ACTION ]             [ PROXY ENGINE ]             [ WEB SERVER ]
        |                            |                            |
  (Click Refresh)                    |                            |
        |                            |                            |
        +----[ REQUEST ]-----------> |                            |
        |                    { MATCH: User-Agent }                |
        |                    { REPLACE: HTB-Agent }               |
        |                            +----[ MODIFIED REQ ]------> |
        |                            |                            |
        |                            |                    (Process Request)
        |                            |                            |
        | <---[ MODIFIED RES ]-------+ <---[ ORIGINAL RES ]-------+
        |                    { MATCH: type="number" }
        |                    { REPLACE: type="text" }
        v
 [ UNLOCKED PAGE ]
(Field is now Text)
```

### 💀 HACKER CHEATSHEET

|Goal|Match (Find)|Replace|Location|
|---|---|---|---|
|**Bypass Browser Limits**|`type="number"`|`type="text"`|Response Body|
|**Bypass Max Length**|`maxlength="3"`|`maxlength="100"`|Response Body|
|**Hide Browser Type**|`^User-Agent.*$`|`User-Agent: Unknown`|Request Header|
|**Enable Buttons**|`disabled`|`(leave blank)`|Response Body|

**Pro Tip:** Be careful with "Match and Replace." If you replace something too common (like the word "div"), you might break the entire website layout!