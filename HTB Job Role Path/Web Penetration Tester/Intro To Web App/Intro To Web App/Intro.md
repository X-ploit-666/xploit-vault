# Understanding Web Applications: A Simple Guide

<iframe 
    src="https://x-ploit-666.github.io/xploit-vault/intro-Webapps.html"
    style="
        width: 100%;
        height: 700px;
        border: 1px solid #2563EB;
        border-radius: 8px;
        background-color: #f0f9ff;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    "
></iframe>

### What is a Web Application?

A **Web Application** is a program that you use through your internet browser (like Chrome or Safari). Unlike a simple poster on a wall, it is interactive—it talks back to you.

Web apps use a **Client-Server Architecture**.

- **The Client (Front End):** This is what you see on your screen. It is the "face" of the app.
    
- **The Server (Back End):** This is the "brain" located far away. It stores your data (in a **Database**) and does the heavy thinking.
    

Because they live on the internet, companies can update them instantly for everyone at once. Examples include Gmail, Amazon, and Google Docs.

### Web Apps vs. Websites (Web 1.0 vs. Web 2.0)

- **Old Websites (Web 1.0):** These were **Static**. They were like digital brochures. If a developer wanted to change a word, they had to edit the file by hand. They didn't "do" anything.
    
- **Web Applications (Web 2.0):** These are **Dynamic**. The content changes based on what you click or type. They are "modular," meaning they are built in pieces, and they work on any screen size (phone or PC).
    

### Web Apps vs. Computer Apps (Native Apps)

- **Native Apps:** These are programs you install (like Microsoft Word or a heavy game). They are fast because they use your computer's "muscles" (hardware), but they take up space on your hard drive.
    
- **Web Apps:** You don't install them. They run in the browser, so they don't take up space. Everyone always uses the newest version because the update happens on the server, not your computer.
    

### Key Terms Defined

- **Open Source:** Apps where the "recipe" (code) is free for anyone to see and change (e.g., WordPress).
    
- **Closed Source (Proprietary):** Apps owned by a company; you usually pay to use them (e.g., Shopify).
    
- **Penetration Testing (Pentesting):** Hiring a "good hacker" to try and break the app to find its weaknesses before the "bad hackers" do.
    
- **Vulnerability:** A mistake in the code that acts like an unlocked door for hackers.
    

### Security Risks: How Hackers Attack

Since web apps are open to the whole world, they are big targets. If a hacker breaks in, they can steal sensitive info from the server's database.

**Common Attack Types:**

1. **SQL Injection:** A hacker sends "evil commands" into a text box to trick the database into giving up secrets.
    
2. **IDOR:** You change a number in the web link (like changing `/user/10` to `/user/11`) to see someone else’s private profile.
    
3. **File Upload Flaw:** A hacker uploads a virus disguised as a profile picture to take over the server.
    
4. **Password Spraying:** Trying one common password (like `Password123`) against thousands of different usernames at once.
    

### Code Analysis: The Registration Flaw

The text mentions a specific vulnerability during user registration. Let's look at the "hidden" data being sent to the server.

**The Code (HTTP Request):** `POST /register HTTP/1.1` `Content-Type: application/x-www-form-urlencoded`

`username=bjones&password=Welcome1&email=bjones@inlanefreight.local&roleid=3`

**What this code does:** When a user clicks "Sign Up," the browser sends this list of information to the server.

- **`username`, `password`, `email`**: These are standard details for a new account.
    
- **`roleid=3`**: This is the "important part." In this system, the number `3` might stand for a "Basic User."
    

**The Hack:** If the developers didn't lock this down, a hacker could manually change that last part to `roleid=0` or `roleid=1`. If `0` stands for "Admin," the hacker has just tricked the website into giving them full control over the entire system just by changing one single digit.

### Visual Explanation (Infographic)

**1. HOW IT WORKS** [ YOU (Client) ] <----Internet----> [ THE BRAIN (Server) ] | | Clicks a button Processes logic Sees the result Checks Database

**2. THE "FRONT END" TRINITY**

- **HTML:** The skeleton (labels, boxes).
    
- **CSS:** The skin (colors, fonts).
    
- **JS:** The muscles (buttons that actually do things).
    

**3. THE ATTACK CHAIN** [ Find a Bug ] --(leads to)--> [ Steal Email List ] --(leads to)--> [ Guess Passwords ] ^ | |___**[ TOTAL SYSTEM ACCESS ]**_|