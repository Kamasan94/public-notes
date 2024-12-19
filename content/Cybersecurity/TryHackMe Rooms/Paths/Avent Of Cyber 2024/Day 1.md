---
title: "<% tp.file.title %>"
draft: false
tags:
- cybersecurity
- opsec
---
# Maybe SOC-mas music, he thought, doesn't come from a store
### Investigating the Website

It's a YouTube to Mp3 Converter
These websites has been observed to have significant risks:

- **Malvertising**: malicious ads
- **Phishing Scams**
- **Bundled malware**


### Getting some tunes

https://www.youtube.com/watch?v=dQw4w9WgXcQ

![[AoC24D1Im1.png]]


We see a file called `somg.mp3`
```bash
file somg.mp3 
somg.mp3: MS Windows shortcut, Item id list present, Points to a file or directory, Has Relative path, Has Working directory, Has command line arguments, Archive, ctime=Sat Sep 15 07:14:14 2018, mtime=Sat Sep 15 07:14:14 2018, atime=Sat Sep 15 07:14:14 2018, length=448000, window=hide
```

It's a link file type used in Windows

Let's try `eixiftool`

```
exiftool somg.mp3
```

We see a link to a PowerShell script.
This link also shows the GitHub account of the offender.