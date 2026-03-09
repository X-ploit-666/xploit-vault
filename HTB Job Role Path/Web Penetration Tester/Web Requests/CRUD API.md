# [MOD-06] CRUD API INTERACTION 🛠️

<iframe 
    src="https://x-ploit-666.github.io/xploit-vault/CRUD-API.html"
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

An **API** (Application Programming Interface) is like a waiter at a restaurant. You tell the waiter what you want (your request), the waiter goes to the kitchen (the database), and brings you back your food (the data).

Most APIs follow the **CRUD** model, which represents the four basic actions you can take on a database:

1. **C**reate: Add something new.
2. **R**ead: Look at existing data.
3. **U**pdate: Change existing data.
4. **D**elete: Remove data.
    

### 2. The CRUD & HTTP Mapping

To tell the API which action you want to perform, you use specific **HTTP Methods**.

| Operation  | HTTP Method | Simple Meaning                                 |
| ---------- | ----------- | ---------------------------------------------- |
| **Create** | `POST`      | "Add this new item to the list."               |
| **Read**   | `GET`       | "Show me what's in the list."                  |
| **Update** | `PUT`       | "Replace this old item with this new version." |
| **Delete** | `DELETE`    | "Get rid of this item."                        |

_Note: Sometimes **PATCH** is used for updates. **PUT** replaces the whole item, while **PATCH** just changes one tiny part (like changing a password without touching the username)._

### 3. Code & Tools: Mastering the API

We use `curl` to talk to the API and a tool called `jq` to make the results look pretty (JSON formatting).

#### 🛠️ Code Extraction & Explanation

`curl -s http://<IP>/api.php/city/london | jq`

- **READ Operation:** This asks for the "london" entry.
- **`-s`**: Silent mode (hides progress bars).
- **`| jq`**: Takes the messy one-line text and turns it into a clean, readable list.
    

`curl -X POST -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json' http://<IP>/api.php/city/`

- **CREATE Operation:** Adds a new city.
- **`-H 'Content-Type: application/json'`**: Essential! It tells the API we are sending JSON data, not a regular form.
    

`curl -X PUT -d '{"city_name":"New_Name", "country_name":"HTB"}' -H 'Content-Type: application/json' http://<IP>/api.php/city/london`

- **UPDATE Operation:** Changes "london" to "New_Name".
- **The URL:** Notice we specify `/london` at the end so the API knows _which_ row to change.
    

`curl -X DELETE http://<IP>/api.php/city/HTB_City`

- **DELETE Operation:** Permanently removes "HTB_City" from the server.
    

### 4. Visual Infographic (The API Cycle)

```
[ YOU / HACKER ]                                [ API ENDPOINT ]
       |                                               |
       |  --- (GET /city/london) --------------------> | [ READ ]
       |  <--- (JSON: "London", "UK") ---------------- |
       |                                               |
       |  --- (POST /city/ + {JSON Data}) -----------> | [ CREATE ]
       |  <--- (Success: Resource Created) ----------- |
       |                                               |
       |  --- (PUT /city/london + {New Data}) -------> | [ UPDATE ]
       |  <--- (Success: Resource Modified) ---------- |
       |                                               |
       |  --- (DELETE /city/london) -----------------> | [ DELETE ]
       |  <--- (Success: Resource Gone) -------------- |
       |                                               |
[ DATABASE UPDATED ] <---------------------------------┘
```

### 5. Technical Terms Simplified

- **Endpoint:** The specific URL used to talk to the API (e.g., `/api.php`).
- **JSON:** The "Universal Language" of APIs. It uses curly braces `{ }` and quotes `" "`.
- **jq:** A terminal tool that acts like a "pretty-printer" for JSON code.
- **Pipe (|):** A command that takes the output of one tool and shoves it into the next tool.
- **REST API:** A popular style of API that uses standard HTTP methods for all actions.