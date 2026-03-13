![[Gemini_Generated_Image_uggu9huggu9huggu.png]]
### Simple Explanation

**Token Cracking** is when an attacker uses a computer program to guess secret codes, like coupons 🎟️, gift card numbers 💳, or discount tokens. Instead of a human typing one code at a time, a bot tries thousands of different combinations every minute until it finds "live" codes that actually give a discount or a cash balance.

### Attack Overview Table

|**Item**|**Explanation**|
|---|---|
|**Attack Name**|Token Cracking (OAT-002) 🔨|
|**What the attacker does**|Guesses many combinations of codes automatically.|
|**Why attackers do it**|To get free products, money, or access to special deals.|
|**Main target**|E-commerce "Promo Code" boxes or Gift Card balance checkers.|
|**Real-world impact**|Financial loss for the company and theft of user balances.|

### Attacker Mindset

The attacker looks for **patterns**. If a website gives out a coupon like `SAVE10`, they might think: "Maybe there is a `SAVE11` or `SAVE12`?" 🧠 They look for codes that are short or easy to guess. They want to find a spot on the website that lets them try many codes quickly without getting blocked.

### Attack Flow Diagram

Attacker Script 🤖 → List of Potential Tokens 📋 → Discount/Coupon Input ✍️ → Server Check 🖥️ → Success / Failure 📢 → Valid Tokens Collected ✅

**How it works:**

1. **Script**: The bot starts the process.
    
2. **List**: It pulls from a list of "guesses" (like 1234, 1235).
    
3. **Input**: It automatically types the code into the website's form.
    
4. **Server**: The website checks if the code is in its database.
    
5. **Response**: The site says "Success!" or "Invalid Code."
    
6. **Collection**: The bot saves the "Success" codes for the attacker to use later.
    

---

### Step-by-Step Attack Methodology

- **Step 1 – Target Identification**: Find a page (like a checkout or balance checker) that doesn't stop you from trying too many codes.
    
- **Step 2 – Code Generation**: Create a list of codes based on patterns (e.g., `GIFT-1001`, `GIFT-1002`) or common words.
    
- **Step 3 – Automated Testing**: Use a tool to send these codes to the website as fast as possible. ⚡
    
- **Step 4 – Result Harvesting**: The bot logs every code that results in a discount or a balance.
    

---

### Real Web Application Example: The Subscription Service 📺

Imagine a streaming service that gives out `FREE-MONTH` codes. An attacker knows the format is `FREE-XXXX` (where X is a number). They set their bot to try every number from `0000` to `9999`. Even if only 1% of those codes are active, the attacker just stole 100 months of free service! 💸


### Detection Clues (What Defenders Notice) 🚩

|**Signal**|**Why it happens**|
|---|---|
|**High failure rate** 📉|Bots guess wrong thousands of times for every one "hit."|
|**Traffic spikes at checkout** 📈|Bots hit the coupon box much faster than any human could.|
|**Uniformity in attempts** 🤖|Many attempts might use the same "User-Agent" (browser identity) or IP address.|

---

### Prevention Methods 🛡️

- **Rate Limiting 🚦**: Restricting how many times a single user or IP can try a code within a minute.
    
- **CAPTCHA 🧩**: Triggering a "human test" (like picking out traffic lights) after 3–5 failed attempts to stop the script.
    
- **Token Complexity 🔐**: Making codes long and random (e.g., `ABCD-1234-EFGH`) instead of easy words like `SAVE10`.
    

---

### Related Security References 📚

- **CAPEC (Attack Patterns)**: A dictionary of _how_ hackers think (e.g., "Brute Force").
    
- **CWE (Weaknesses)**: A list of _bugs_ in code (e.g., "Improper limitation of attempts").
    
- **WASC (Threats)**: A standard used to classify web security risks.
    
- **OWASP Category**: This attack is a form of **Abuse of Functionality**.

### Diagram
![[OAT-002.png]]