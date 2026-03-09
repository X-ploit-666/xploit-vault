# [MOD-04] GET REQUESTS & AUTHENTICATION BYPASS 🛸

<iframe 
    src="https://x-ploit-666.github.io/xploit-vault/GET.html"
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
When you visit any website, your browser's first move is to send a **GET request**. It’s asking the server: "Hey, give me the files for this page."

Sometimes, the server wants to make sure you are allowed to see those files. One of the oldest ways to do this is called **HTTP Basic Auth**. Instead of a fancy login page with buttons, a small box pops up in your browser asking for a username and password. This is handled by the server software (like Apache or Nginx) before the website even loads.

### 2. HTTP Basic Auth (The Server Gatekeeper)
Basic Auth doesn't use a database-driven login form. Instead, it expects a specific **Header** to be sent with the request.
![[http_auth_login.jpg]]
#### How it works:
1. You try to access a page.
2. The server sends back a **401 Authorization Required** status.
3. Your browser (or cURL) sends back a header called `Authorization`.
4. Inside that header is your username and password, but they are "scrambled" using **Base64** encoding.
    
    - _Example:_ `admin:admin` becomes `YWRtaW46YWRtaW4=`.
    - **⚠️ Security Warning:** Base64 is NOT encryption. Anyone who sees this string can turn it back into "admin:admin" instantly.
        

### 3. GET Parameters (Sending Data in the URL)
When you search for something on a website, the site needs to send your "search term" to the backend. In a GET request, this info is added directly to the end of the URL.
- **The Structure:** `search.php?search=london`
- The `?` marks the start of the data.
- `search` is the **Parameter Name**.
- `london` is the **Value**.
    
### 4. Code & Tools: Terminal Mastery
Here is how we use the command line to handle these secure requests.
#### 🛠️ Code Extraction & Explanation

`curl -u admin:admin http://<IP>:<PORT>/`

- **The `-u` flag:** This is the "User" flag. It tells cURL to take the `username:password` and automatically turn it into the correct `Authorization` header for you.
    

`curl http://admin:admin@<IP>:<PORT>/`

- **Inline Auth:** This is a different way to do the same thing. You put the login info directly inside the URL string.
    

`curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<IP>/`

- **The `-H` flag:** Here, we are manually setting the header. We are pretending to be authenticated by sending the scrambled Base64 string directly. This is how you "replay" an authentication.
    

`curl 'http://<IP>/search.php?search=le' -H 'Authorization: Basic ...'`

- **Requesting a Resource:** This command asks the `search.php` script for all cities starting with "le" while also proving we are logged in.
    

### 5. Visual Infographic (The Auth Flow)
```
[ HACKER / CLIENT ]                             [ WEB SERVER ]
        |                                              |
        | 1. GET / (No Credentials)                    |
        |--------------------------------------------> |
        |                                              | [ 401 DENIED ]
        | <--------------------------------------------| "Who are you?"
        |                                              |
        | 2. GET / (Auth: Basic YWRtaW46YWRtaW4=)      |
        |--------------------------------------------> | [ VERIFYING ]
        |                                              | (Scrambled string
        |                                              |  == admin:admin)
        | <--------------------------------------------|
        |          [ 200 OK + PRIVATE DATA ]           |
        |                                              |
[ ACCESS GRANTED ] <--- (File unlocked)
```

### 6. Technical Terms Simplified

- **Basic Auth:** A simple "ID check" built into the web server.
- **Base64:** A way to turn text into a string of different characters so it's safe for travel. **It is not a secret code.**
- **Verbose (-v):** The cURL mode that lets you see the secret `Authorization` header being sent.
- **Fetch:** A modern way to send web requests using JavaScript directly from your browser's console.
- **GET Parameter:** Data "tacked on" to the end of a URL.