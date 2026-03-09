# 🌐 Web Proxy Setup: The Simple Guide

### What is a Web Proxy?

A **Web Proxy** is a tool that sits in the middle between your computer and the internet. Instead of your browser talking directly to a website, it talks to the proxy first.

**Why do we do this?**

- To see exactly what hidden messages (requests) your apps are sending.
    
- To catch a message before it leaves your computer (intercepting).
    
- To change the message to see how the website reacts (testing).
    

### Method 1: The Easy Way (Pre-Configured Browser)

Both Burp Suite and ZAP come with their own "built-in" browser. These browsers are already set up to talk to the proxy, so you don't have to change any settings.

- **In Burp:** Go to the `Proxy` tab, then `Intercept`, and click `Open Browser`.
    
- **In ZAP:** Look for the Firefox icon at the end of the top bar and click it. Using these is the fastest way to start testing because everything is ready to go.
    

### Method 2: Using Your Own Browser (Manual Setup)

If you want to use a real browser like Firefox, you have to tell it to send its traffic to your proxy tools.

#### 1. The Listening Port

By default, both tools listen on **Port 8080**. Think of a "port" as a specific door on your computer where data enters.

- You must make sure your browser and your tool are using the same door number.
    
- If you use a door that is already busy, the tool will give you an error.
    
- **To change the door (Port):**
    
    - **Burp:** `Proxy` > `Proxy settings` > `Proxy listeners`.
        
    - **ZAP:** `Tools` > `Options` > `Network` > `Local Servers/Proxies`.
        

#### 2. Using FoxyProxy (The Fast Switch)

Instead of typing settings into Firefox every time, use an extension called **FoxyProxy**.

- It is already on PwnBox. If you're on your own computer, find it on the Firefox Extensions page.
    
- **Setup:** Click the FoxyProxy icon -> `Options` -> `Add`.
    
- **Details to enter:**
    
    - **IP:** `127.0.0.1` (This means "this computer").
        
    - **Port:** `8080`.
        
    - **Name:** `Burp` or `ZAP`.
        
- To start, just click the icon and select the name you saved.
    

### Method 3: The "ID Card" (Installing CA Certificates)

Websites use **HTTPS** for security. If you don't install a **CA Certificate** (which acts like a digital ID card), your browser won't trust the proxy, and you'll see "Warning" screens or blocked traffic.

#### 1. Get the Certificate

- **For Burp:** Turn on the Burp proxy in FoxyProxy, go to `http://burp` in your browser, and click `CA Certificate`.
    
- **For ZAP:** Go to `Tools` -> `Options` -> `Network` -> `Server Certificates` and click `Save`.
    

#### 2. Put it in Firefox

1. Go to `about:preferences#privacy` in your browser address bar.
    
2. Scroll to the bottom and click `View Certificates`.
    
3. Click the `Authorities` tab, then click `Import`.
    
4. Pick the file you just saved.
    
5. **Crucial Step:** Check the boxes that say "Trust this CA to identify websites" and "Trust this CA to identify email users."
    

### 📊 Visual Flowchart

```
[ YOUR BROWSER ] 
       |
       | (Sends request)
       v
[ FOXYPROXY (Port 8080) ] <--- Redirects traffic
       |
       v
[ WEB PROXY TOOL (Burp/ZAP) ] <--- You catch & edit here
       |
       | (CA Certificate confirms it's safe)
       v
[ THE INTERNET / WEBSITE ]
```

### 🛠️ Key Concepts Breakdown

| Term               | Simple Explanation                                                                        |
| ------------------ | ----------------------------------------------------------------------------------------- |
| **Proxy**          | A middle-man that passes data between two points.                                         |
| **Intercept**      | Catching a message before it reaches its destination.                                     |
| **CA Certificate** | A digital "trust badge" that lets your browser talk to the proxy without security errors. |
| **IP 127.0.0.1**   | The code for "this current computer" (the local machine).                                 |
| **Port**           | A specific software "door" number used for communication.                                 |
| **HTTPS**          | A secure way of sending data that the proxy needs a certificate to "read."                |