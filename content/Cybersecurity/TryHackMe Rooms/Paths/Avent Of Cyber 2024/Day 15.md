---
title: Day 15 - Be it ever so heinous, there's no place like Domain Controller.
draft: false
tags:
  - cybersecurity
  - active-directory
---

### Investigating an [[Active Directory]] Breach
Reviewing [[Group Policy]] Objects (GPO) is a great investigation step.
```powershell
Get-GPO -All
```

There is a `Malicious GPO - Glitch_Malware Persistence`

We export the report to HTML so we can read it in a better way
```powershell
Get-GPOReport -Name "Malicious GPO - Glitch_Malware Persistence" -ReportType HTML -Path ".\malicious.html"
```



### Event Viewer
This is a repository that stores a record of system activity, including security events.
This is a table of the most important event ID 

| Event ID | Description                                                           |
| -------- | --------------------------------------------------------------------- |
| 4624     | A user account has logged on                                          |
| 4625     | A user account failed to log on                                       |
| 4672     | Special privileges (i.e. SeTcbPrivilege) have been assigned to a user |
| 4768     | A TGT (Kerberos) ticket was requested for a high-privileged account   |


### User Auditing
We can use PowerShell to audit the user status.
```powershell
Get-ADUser -Filter * -Properties MemberOf | Select-Object Name, SamAccountName, @{Name="Groups";Expression={$_.MemberOf}}
```



### Reviewing PowerShell History and Logs
It is a fantastic way to see recent actions taken by user account on the machine
`%APPDATA%\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt.`

Additionally, logs are recorded for every PowerShell process executed on a system. These logs are located within the Event Viewer under `Application and Services Logs -> Microsoft -> Windows -> PowerShell -> Operational` or also under `Application and Service Logs -> Windows PowerShell`. The logs have a wealth of information useful for incident response.