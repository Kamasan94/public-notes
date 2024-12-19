#cybersecurity #tryhackme #firewall #intrusion-detection

Objectives:

- Learn to understand incident analysis through the Diamond Model.
- Identify defensive strategies that can be applied to the Diamond Model.
- Learn to set up firewall rules and a honeypot as defensive strategies.

Intrusion detection and prevention is a critical component of cyber security aimed at identifying and mitigating threats. When set up early, intrusion detection becomes a proactive security measure.

[The DiamondModel](https://tryhackme.com/room/diamondmodelrmuwwg42)[](https://tryhackme.com/room/diamondmodelrmuwwg42) is a security analysis framework that seasoned professionals use to unravel the mysteries of adversary operations and identify the elements used in an intrusion. It comprises four core facets, interconnected to form a well-orchestrated blueprint of the attacker's plans:  

- Adversary
- Victim
- Infrastructure
- Capability

### Adversary
Picture this: a collection of adversaries working together to orchestrate widespread security breaches, just like the enigmatic [advanced persistent threat (APT)](https://csrc.nist.gov/glossary/term/advanced_persistent_threat) groups.

### Victim
The target of the adversary

## Infrastructure
Represents the physical and logical interconnections that an adversary employs.

## Capability
What skills, tools and techniques they employ
Tactics, techniques, and procedures (TTPs)

Some examples:
- [[Phishing]]
- Exploiting vulnerabilities
- [[Social engineering]]
- [[Malware]] attack
- Insider threat
- [[Denial-of-service]] (DoS) Attacks


### Defensive Diamond Model

#### Defense Capability

**Threat hunting** is the proactive and iterative process to actively search for signs of malicious activities or security weaknesses

**Vulnerability management** is a structured process of identifying, assessing, prioritizing, mitigating and monitoring vulnerabilities in an organization's system and application

#### Defense Infrastructure
[[Firewall]], common types:
- **Stateless/packet-filtering**: provides the most straightforward functionality by inspecting and filtering individual network packets based on a set of rules
- **Stateful inspection**: used to track the state of network connections and use this information to make filtering decisions.
- **Proxy service**: deep packet inspection
- **Web application firewall (WAF)** 
- **Next-generation firewall**: combines the functionalities of the stateless, stateful, and proxy



### Configuring Firewalls to Block Traffic

Check firewall status in Ubuntu
```bash
sudo ufw status 
```

Allow outgoing and deny incoming

```bash
sudo ufw default allow outgoing
sudo ufw default deny incoming
```

Allow incoming on port 22 [[SSH]]
```bash
sudo ufw allow 22/tcp
```

Incorporate [[IP]] address and network interfaces
```bash
sudo ufw deny from 192.168.100.25
sudo ufw deny in on eth0 from 192.168.100.26
```

Enable service
```bash
sudo ufw enable
```

Reset to default
```bash
sudo ufw reset
```


### Honeypot

A trap laid for the attackers.
They can be classified into two main types:
- **Low-interaction honeypots**: mimic simple systems
- **High-interaction honeypots**: complex system

We use PenTBox



### Answers
**Which security model is being used to analyse the breach and defense strategies?**
Diamond Model

**Which defence capability is used to actively search for signs of malicious activity?**
Threat hunting

**What are our main two infrastructure focuses?**
Firewall and honeypot

**Which firewall command is used to block traffic?**
Deny

**There is a flag in one of the stories. Can you find it?**
```bash
sudo ufw allow http
sudo ufw allow 8090/tcp
```
THM{P0T$_W@11S_4_S@N7@}


