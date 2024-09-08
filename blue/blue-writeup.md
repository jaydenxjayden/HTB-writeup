# Blue

This is the write-up for the box Blue

I start by adding this box into my /etc/hosts file:
```markdown
10.10.10.40    blue.htb
```

## Enumeration

Start with enumeration using nmap, I found that the target is using windows 7, and based on the name of the machine, it is likely that its vulnerable to the eternalblue exploit. <br>

<img src="https://i.imgur.com/QGSxcP7.png" height="80%" width="80%"/> <br>

Run the nmap scripts from the vuln category, to see if I can find any potential vulnerabilities. Results shows that target is vulnerable to the ms17-010 remote code execution vulnerability. <br>

<img src="https://i.imgur.com/qwLzVnQ.png" height="80%" width="80%"/> <br>

Find the exploit within metasploit. Typed in `search ms17-010 ` and immediately saw various exploits avavailable. <br>
<img src="https://i.imgur.com/RrNUz6m.png" height="80%" width="80%"/> <br>

I started from the first exploit available in the list, proceeded to set the required fields accordingly. <br>
<img src="https://i.imgur.com/nETpBKv.png" height="80%" width="80%"/> <br>

Run the exploit and gain a meterpreter shell. This is one of the most straightforward boxes, if done using Metasploit. <br>
<img src="https://i.imgur.com/frmSey2.png" height="80%" width="80%"/> <br>

I get NT AUTHORITY\SYSTEM access using this exploit and no privesc is required. <br>
<img src="https://i.imgur.com/lkTC4aM.png" height="40%" width="40%"/> <br>

From here, we can find the user.txt and root.txt in the desktop of haris & Administrator. <br>
<img src="https://i.imgur.com/W7NXEHl.png" height="40%" width="40%"/> <br>
