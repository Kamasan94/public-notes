---
title: "I’m all atomic inside!"
draft: false
tags:
- cybersecurity
- atomicRedTeam
---


### Detection Gaps
There are gaps in the detection
Reasons:
- Red teamers will find new sneaky ways to thwart our detection
- **The line between anomalous and expected behaviour is often very fine and sometims even has significant overlap**

### Cyber Attacks and the Kill Chain
![[AoC24D4Im1.png]]


### MITRE ATT&CK
Collection of  tactics and techniques that threat actors perform through the kill chain
https://attack.mitre.org/

### Atomic Red
It's a library, a collection of red team test cases. The library also supports automation.

### Running an Atomic
Let's recreate the attack emulation 

For searching Atomic Tests
```powershell
Invoke-AtomicTest T1566.001 -ShowDetails
```

