---
title: "Secure Linux Server"
draft: false
tags:
- cybersecurity
- system-engineering
---
## Securing
### 1.Disable Root Login
#### Why?
Root user has unlimited access, so it is a target for attackers.
We use a limited permission user

#### How to do it
```bash
sudo nano /etc/ssh/sshd_config
```

Set `PermitRootLogin no`

### 2.Use Key-Based Authentication
#### Why?
Password logins can be a weak point in every system

#### How to do it
Let's generate an SSH key on local system
```
ssh-keygen -t rsa -b 4096
```

Then move the key to the server
In windows
```
C:\> type %USERPROFILE%\.ssh\id_rsa.pub | ssh user@host "cat >> .ssh/authorized_keys"
```

In Linux
```
ssh-copy-id username@server_ip
```


Then modify `sshd.conf` and `ssjd_config.d/50-cloud-init.conf` with
`PasswordAuthentication no`

### 3.Enforce Strong Password Policies
#### Why?
To prevent weak, easily guessed passwords

#### How to do it
`/etc/security/pwqualit-conf`'

And set these values
`minlen = 12 minclass = 3`


### 4.Keep the System Updated
#### Why?
Updates contains patches for known vulnerabilities

#### How to do it
```bash
sudo apt update && sudo apt upgrade -y
```

**Enable automatic updates**
```bash
sudo apt install unattended-upgrades
```


### 5.Configure a Firewall
Firewalls limit access to specific services and blocks unauthorized traffic.

#### How to do it
Install and configure `uwf`

```bash
sudo apt install ufw
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

### 6.Install and Configure Intrusion Detection
#### Why?
Fail2Ban protects your server form brute-force attacks by blocking IP's with too many failed login attempts

#### How to do it
```bash
sudo apt install fail2ban
```

Configure
```bash
sudo vi /etc/fail2ban/jail.conf
```

Enable SSH monitoring with
```
[sshd]  
enabled = true  
maxretry = 5  
bantime = 3600
```

### 7.Disable Unnecessary Services
#### Why?
Fewer potential entry points

#### How to do it?
1. List all active services `sudo systemctl list-unit-files -=type=service --state=enable`
2. Disable unneeded services `sudo systectml disable service_name`


### 8.Set proper File Permissions
#### Why?
Prevent unauthorized access or modification

#### How to do it
```bash
sudo chmod 600 /etc/ssh/sshd_config
sudo chmod 640 /var/log/auth.log
```


### 9.Enable Logging and Monitoring
#### Why?
Logging provides a record of system events

#### How to do it
Use `ryslog` to manage logs or a centralized logging solution like ELK

```bash
sudo add-apt-repository ppa:adiscon/v8-devel
sudo apt-get update
sudo apt-get install rsyslog
```

### 10.Implement Auditing with `auditd`
#### Why?
Monitor critical files and actions, alerting when unauthorized changes

#### How to do it?
```bash
sudo apt install auditd
```

Add rules in `/etc/audit/audit.rules`
```
-w /etc/passwd -p wa -k passwd_changes
```

Restart `auditd`
```bash
sudo systemctl restart auditd
```

### 11.Secure SSH Configuration
#### Why?
This hardens the server against attacks by limiting login options

#### How to do it
1. Open `/etc/ssh/sshd_config`
2. Adjust settings
```
Port 2222 # Change the default SSH port  
PasswordAuthentication no # Disable password login  
Protocol 2 # Use SSH protocol 2 only
```
1. Restart `sudo systemctl restart ssh`

### 12.Harder Kernel Parameters
#### Why?
Kernel hardening secures network settings and mitigates certain attacks by restricting network behaviors

#### How to do it?
Open `/etc/sysctl.conf` and add settings:
```
net.ipv4.tcp_syncookies = 1  
net.ipv4.conf.all.rp_filter = 1  
net.ipv4.conf.default.accept_source_route = 0
```

Apply changes
```bash
sudo sysctl -p
```

### 13.Schedule Regular Backups
#### Why?
Backups ensure data is recoverable in case of cyberattack, accidental deletion, or system failure

#### How to do it?
2. Use `rsync` or `tar` 
```bash
rsync -av /import_data /backup_location
```


### 14.Set Resource Limits
#### Why?
This help prevent [[DoS]] attacks by limiting resource consumption

#### How to do it?
Edit `/etc/security/lmits.conf`

```bash
sudo nano /etc/security/limits.conf
```

Set limits
```
* soft nproc 4096
* hard nproc 8192
```

### 15.Use Security Scanning Tools
#### Why?
They identify misconfigurations and vulnerabilities.

#### How to do it?
Install Lynis
```bash
sudo apt install lynis
```

Run a system scan
```bash
sudo lynis audit system
```

### 16.Protect Against Malware
Install ClamAV
```bash
sudo apt install clamav
```

Update and scan
```bash
sudo freshclam
sudo clamscan -r /directory_to_scan
```

### 17.Enable Multi-Factor Authentication (MFA)
It adds a second layer of verification

Install Google Authenticator
```bash
sudo apt install libpam-google-authenticator
```

Set up MFA
```
google-authenticator
```

Enable MFA in PAM configuration
```bash
sudo nano /etc/pam.d/sshd
```

Add:
```bash
auth required pam_google_authenticator.so
```

### 18.Implement Network Segmentation
This limits traffic between different parts of the infrastructure.
By isolating sensitive services on private subnets or VLAN we limit the exposure and protect data

#### How to do it
3. On AWS or cloud platforms use Virtual Private Clouds
4. With Firewalls
5. On Linux configure `iptables`

Example
```bash
sudo iptables -A INPUT -p tcp -s trusted_ip --dport 22 -j ACCEPT
```


### 19.Restrict `sudo` access
`sudo visudo`
Define specific permissions for each user or user group
```
username ALL=(ALL) NOPASSWD: /path/to/specific_command
```

### 20.Enforce AppArmor or SELinux for Mandatory Access Control
They add fine-grained permissions, confining processes to a limited set of resources and actions.

#### How to do it
**For AppArmour** (Ubuntu/Debian)
- Check if AppArmor is enabled
	```bash
	sudo apparmor_status
```
- Configure specific profiles for services in `/etc/apparmor.d`

**For SELinux** (CentOS/RHEL)
- Enable SELinux
	`sudo setenforce 1`
- Use `semanage` to define policies
	`sudo semanage fcontext -as -t https_sys_content_t "/wev(/.*)?`


### 21.Use Port Knocking fo SSH Access
Port Knocking helps hide the SSH port by requiring a sequence of port knocks to open the SSH port

Install `knockd`
```bash
sudo apt install knockd
```

Configure port knocking in `/etc/knockd.conf`
```
[openSSH]  
sequence = 7000,8000,9000  
seq_timeout = 5  
command = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
```

Start 
```bash
sudo systemctl start knockd
```

Now, only knocking on port 7000, 8000, and 9000 in that order will port 22 open for SSH

### 22.Limit open ports to reduce attack surface
Use `netstat` or `ss`
```bash
sudo ss -tuln
```

### 23.Use File Integrity Monitoring (FIM)
FIM detects unauthorized changes to critical system files.

```bash
sudo apt install aide
```

Initialize AIDE database:
```bash
sudo aideinit
```

Set up a cron job to run regular AIDE checks
```bash
sudo crontab -e
```

Add
```
0 3 * * * /usr/bin/aide --check
```
### 24.Implement Rate Limiting
It protects against DoS attacks by limiting the number of requests

Use `iptables` to limit SSH connections
```bash
sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set  
sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP
```

### 25.Encrypt Sensitive Data
Encryption protects data in case of a security breach by making unreadable 

**Data at Rest**: 
```bash
sudo apt install ecryptfs-utils sudo ecryptfs-setup-private
```

### 26.Setup DNS security extensions (DNSSEC)
It protects DNS records from tampering by adding verification

**With BIND**: Enable DNSSEC in the `named.conf` by adding
```
dnssec-enable yes; dnssec-validation auto;
```

### 27.Use a Host-Based Intrusion Detection System (HIDS)
```bash
sudo apt install ossec-hids
```
https://www.rapid7.com/blog/post/2017/06/30/how-to-install-and-configure-ossec-on-ubuntu-linux/
### 28.Regularly Rotate Encryption Keys and Credentials
6. Use a **Credential Management System** 
7. Rotate SSH Keys, API Keys and passwords

### 29.Apply Principle of Leas Privilege (PoLP)
8. Assign specific permission to each user in `/etc/sudoers`
9. For database, grant access only to specific tables or operations
10. Example SQL
```sql
GRANT SELECT, INSERT ON database.* TO 'user'@'host';
```
