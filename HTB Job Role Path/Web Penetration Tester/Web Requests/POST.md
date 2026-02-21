# 💀 [MOD-05] POST REQUESTS & SESSION HIJACKING 💀


<iframe 
    src="https://x-ploit-666.github.io/xploit-vault/POST.html" 
    style="
        width: 100%;
        height: 700px;
        border: 1px solid #2563EB;
        border-radius: 8px;
        background-color: #f0f9ff;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    "
></iframe>





### 1. Simple Summary

While **GET** requests are for "looking," **POST** requests are for "sending." When you log in to a site or upload a file, you are likely using POST.

Unlike GET, which puts your data in the URL for everyone to see, POST hides your data inside the **Request Body**. This is better for three reasons:

1. **Privacy:** Your password isn't saved in the browser history or server logs.
2. **Size:** You can send massive files (URLs are usually limited to 2,000 characters).
3. **Binary:** You can send raw data like images or PDFs, not just text.
    

### 2. Sessions & Cookies (The Digital Hall Pass)

Websites have "short-term memory loss." They forget who you are the second you click a new link. To stay logged in, the server gives you a **Cookie** (like `PHPSESSID`).

- **Set-Cookie:** The server sends this header after you log in.
- **Cookie:** Your browser sends this back with every future request so the server knows, "Oh, it's still the Admin."
    

### 3. JSON Data (The API Language)

Modern apps often send data in **JSON** format (looks like `{"key":"value"}`). When sending JSON, you MUST tell the server what it is by using the `Content-Type: application/json` header, or the server might get confused and ignore you.

### 4. Code & Tools: Crafting POST Requests

Here is how to manually take over a session and send data using the terminal.

#### 🛠️ Code Extraction & Explanation

`curl -X POST -d 'username=admin&password=admin' http://<IP>/`
- **`-X POST`**: Forces cURL to use the POST method.
- **`-d`**: Stands for "Data." This is where you put your login info (`username=admin&password=admin`).
    

`curl -X POST -d '{"search":"london"}' -H 'Content-Type: application/json' http://<IP>/search.php`

- **JSON Handling**: Notice the data is in curly braces `{}`.
- **`-H`**: We manually add the `Content-Type` header so the server knows we are sending JSON.
    

`curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<IP>/`

- **`-b`**: This is the "Cookie" flag. Instead of logging in with a password, we are just handing the server a valid **Session ID**. If the ID is active, you are IN.
    

`curl -L ...`

- **`-L`**: If the website tries to redirect you (like sending you to `/dashboard.php`), this flag tells cURL to follow the move automatically.
    

### 5. Visual Infographic (The POST & Cookie Flow)

```
[ YOU / HACKER ]                                [ WEB SERVER ]
       |                                               |
       | 1. POST /login.php (User/Pass)                |
       |---------------------------------------------> | [ VERIFYING ]
       |                                               | (Valid Credentials)
       | <-------------------------------------------- |
       |    [ 200 OK + Set-Cookie: PHPSESSID=XYZ ]     |
       |                                               |
       |          ( Now you have the "Key" )           |
       |                                               |
       | 2. POST /search.php (Cookie: PHPSESSID=XYZ)   |
       |    { "search": "london" }                     |
       |---------------------------------------------> | [ RECOGNIZED ]
       |                                               | (Session is Active)
       | <-------------------------------------------- |
       |           [ JSON DATA RETURNED ]              |
```

### 6. Technical Terms Simplified

- **Request Body:** The "trunk" of the HTTP message where the heavy data is hidden.
- **PHPSESSID:** A common name for a session cookie used by PHP websites.
- **Base64:** (Reminder) How binary data is often encoded for transport.
- **JSON:** A lightweight way to store and transport data.
- **XSS (Cross-Site Scripting):** An attack where a hacker steals your Cookie to take over your account.