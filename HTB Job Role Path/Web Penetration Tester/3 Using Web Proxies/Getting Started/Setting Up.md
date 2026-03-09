# How to Set Up Your Web Proxy

### Where to Find the Tools

Both **Burp Suite** and **ZAP** work on Windows, Mac, and Linux. If you are using a professional testing system (like PwnBox, Kali, or Parrot Linux), these tools are usually already installed for you. You can find them in the menu bars or by typing their names into a command terminal.

## Setting Up Burp Suite

### Installation

If you don't have it, go to the Burp download page and run the installer for your computer.

- **Java Requirement:** These tools run on **Java** (a specific software platform). Usually, the installer includes Java automatically. If it doesn't work, you might need to install a "Java Runtime Environment" (JRE) separately.
    
- **The JAR Option:** You can also download a `.jar` file. This is a "universal" version that works on any computer with Java.
    

### Starting Your First Project

When you open Burp, it will ask how you want to handle your work:

1. **Temporary Project (Free Version):** This is the only option for the free version. Your work won't be saved when you close the app.
    
2. **New/Existing Project (Pro Version):** This allows you to save your progress. This is helpful for very long jobs or big scans.
    
3. **Settings:** It will ask if you want "Default Configurations" (standard settings) or to "Load a Configuration File." For now, always choose **Default**.
    

## Setting Up ZAP

### Installation

Go to the ZAP download page and pick the installer for your system. Just like Burp, ZAP can also be run as a universal `.jar` file.

### Starting Up

To start ZAP, you can type `zaproxy` in your terminal or find it in your app menu.

- **Saving Work:** When ZAP opens, it asks if you want to save your session.
    
- **Free Saving:** Unlike Burp, ZAP lets you save your work for free. However, for quick practice, you can choose "No" to start a temporary project.
    

## Pro Tip: Dark Mode

If you prefer a dark screen (better for your eyes):

- **Burp:** Go to Settings -> User Interface -> Display -> Theme -> **Dark**.
    
- **ZAP:** Go to Tools -> Options -> Display -> Look and Feel -> **Flat Dark**.
    

## Code Breakdown

The text mentions a specific command to run these tools if the standard installer isn't used.

### The Command:

```
java -jar </path/to/tool.jar>
```

### What this code does:

- **`java`**: This tells your computer to start the Java software engine.
    
- **`-jar`**: This is a specific instruction (a "flag") telling Java that the file we are about to open is a Java Archive (a package of program files).
    
- **`</path/to/tool.jar>`**: This is a placeholder. You replace this with the actual location of the file on your computer (e.g., `C:/Downloads/burpsuite.jar`).
    

**In short:** This command manually "pulls the lever" to start the program using the Java engine installed on your system.