---
title: Day 14 - Even if we're horribly mismanaged, there'll be no sad faces on SOC-mas!
draft: false
tags:
  - cybersecurity
  - certificates
---
### Topics
- Self-signed certificates
- Man-in-the-middle attacks
- Using Burp Suite proxy to intercept traffic

### Certified
- **Public key**: made available to anyone, used to encrypt data
- **Private key**: remains secret, used by the website or sever to decrypt the data
- **Metadata**: additional information, CA, subject, UID, valid period ecc...

### Certificate Authority (CA)
It is a trusted entity that issues certificates.

The browser trusts it and performs these checks:
- **Handshake**
- **Verification**
- **Key exchange** 
- **Decryption**



### The room
There is a self-signed certificate vulnerability inside the machine, we want to hack the GIft Scheduler.
To prevent any DNS logs from alerting enemies, we will resolve the Gift Scheduler FQDN locally
```bash
echo "10.10.255.33 gift-scheduler.thm" >> /etc/hosts
```

Now let's navigate to the machine via browser

![[Pasted image 20241219142452.png]]



With this credentials we can do nothing, so we sniff the traffic with Burp Proxy

The configuration must be like this
![[Pasted image 20241219142813.png]]


### Sniff from the middle
We have to reroute all the traffic to our machine.
We will set our own machine as a gateway for all other machines.
```bash
echo "10.10.249.114 wareville-gw" >> /etc/hosts
```
The ip is our machine

We must start a custom script to simulate the users’ requests to the Gift Scheduler
