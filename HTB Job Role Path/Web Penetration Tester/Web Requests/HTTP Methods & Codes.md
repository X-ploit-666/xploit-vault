
<iframe 
    src="https://x-ploit-666.github.io/xploit-vault/http-methods.html"
    style="
        width: 100%;
        height: 700px;
        border: 1px solid #2563EB;
        border-radius: 8px;
        background-color: #f0f9ff;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    "
></iframe>


---

# ⚡ HTTP PROTOCOL: VERBS & VIBRATIONS ⚡

## 📡 1️⃣ THE ACTION: REQUEST METHODS (VERBS)

HTTP methods tell the server what operation you want to perform on a resource. In modern web development (especially REST APIs), these are the fundamental verbs of the web.

|Method|Cyber Operation|Description|
|---|---|---|
|**GET**|READ|Requests data. Parameters are visible in the URL (Query Strings).|
|**POST**|CREATE|Sends data in the request body. Used for logins and uploads.|
|**PUT**|REPLACE|Replaces an entire resource with new data.|
|**PATCH**|MODIFY|Applies partial updates (change only one field, not the whole resource).|
|**DELETE**|REVISE|Removes a specific resource from the server.|
|**HEAD**|PEEK|Same as GET but returns headers only (no body). Useful for checking file size.|
|**OPTIONS**|SCAN|Asks the server which methods are allowed on a resource.|

## 🚦 2️⃣ THE RESULT: STATUS CODES (RESPONSE CLASSES)

Status codes are 3-digit numbers returned by the server to indicate the outcome of a request.

### 🔹 [1xx] Informational
Server says: "Processing, hold on."
- **101 Switching Protocols:** Upgrading from HTTP to WebSockets.
    
### 🔹 [2xx] Success
Server says: "Request successful."
- **200 OK:** Request completed successfully.
- **201 Created:** New resource successfully created.
- **204 No Content:** Success but no response body (common in DELETE).
    

### 🔹 [3xx] Redirection
Server says: "The resource is somewhere else."
- **301 Moved Permanently:** Resource moved permanently to new URL.
- **302 Found:** Temporary redirect.
- **304 Not Modified:** Use cached version; no changes detected.
    
### 🔹 [4xx] Client Error
Server says: "Problem with your request."
- **400 Bad Request:** Invalid syntax.
- **401 Unauthorized:** Missing or invalid authentication.
- **403 Forbidden:** Authenticated but not allowed.
- **404 Not Found:** Resource does not exist.
- **429 Too Many Requests:** Rate limit exceeded.
    
### 🔹 [5xx] Server Error
Server says: "Problem on my side."
- **500 Internal Server Error:** Generic backend failure.
- **502 Bad Gateway:** Invalid response from upstream server.
- **503 Service Unavailable:** Server overloaded or under maintenance.
    
## 💻 3️⃣ FIELD COMMANDS (cURL & DEVTOOLS)

### 🛠 Overriding Methods with cURL
By default, cURL uses GET. Use `-X` to specify a method:
```bash
# Delete a specific user resource
curl -X DELETE https://api.targetsite.com/users/1337
```

```bash
# Check allowed methods on a server
curl -X OPTIONS https://api.targetsite.com/ -v
```

### 🖥 Monitoring in the Browser
- Open DevTools → Network tab.
- Check the **Method** column (GET, POST, etc.).
- Check the **Status** column (200, 404, etc.).
    
## 🧠 4️⃣ PRO TIP: IDEMPOTENCY
An operation is **Idempotent** if repeating it multiple times produces the same result as doing it once.
- **Idempotent Methods:** GET, PUT, DELETE. (Deleting a file twice still results in the file being gone.)
- **NOT Idempotent:** POST. (Submitting the same form twice creates duplicate resources.)
    

---

