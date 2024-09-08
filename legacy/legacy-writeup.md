# Legacy

This is the write-up for the box Legacy

I start by adding this box into my /etc/hosts file:
```markdown
10.10.10.4    legacy.htb
```

## Enumeration

Starting with a general Nmap scan on all ports, followed by a detailed scan on the open ports found:

<img src="https://i.imgur.com/hOZLw9D.png" height="80%" width="80%"/> <br>

Saving the output of the detailed nmap scan.

<img src="https://i.imgur.com/mHznxrT.png" height="80%" width="80%"/> <br>

Target is Windows XP and has smb open, did a quick google search. <br>
<img src="https://i.imgur.com/Ag5unbk.png" height="80%" width="80%"/> <br>

Open msfconsole and search for a smb scanner to find out if target is vulnerable. <br>
<img src="https://i.imgur.com/OO0KJdk.png" height="80%" width="80%"/> <br>

Decided to go with the 4th module in msfconsole as ‘ms17_010’ showed up in my google search <br>
Results from the scanner shows that host is likely vulnerable to ms17-010, also known as eternalblue exploit <br>
I've written a blogpost on it [here](https://medium.com/@jaydenxjayden/deep-dive-eternalblue-exploit-6b055dcd312e). <br>
<img src="https://i.imgur.com/H7fqh9r.png" height="80%" width="80%"/> <br>

## Exploit

Search for the exploit within msfconsole, I found that there are a few results that matches, go through 1 by 1. <br>
<img src="https://i.imgur.com/FNK2Moc.png" height="80%" width="80%"/> <br>

The first module failed to give us a meterpreter shell as the exploit only supports x64 targets, while legacy is x86. <br>
<img src="https://i.imgur.com/KU5J0Ao.png" height="80%" width="80%"/> <br>
<img src="https://i.imgur.com/eR7cin4.png" height="80%" width="80%"/> <br>

Proceed back to use the second module from the list, and set the required options accordingly. <br>
<img src="https://i.imgur.com/VS8HVNB.png" height="80%" width="80%"/> <br>

Upon running the exploit, I gained a meterpreter shell with SYSTEM access. <br>
<img src="https://i.imgur.com/3gVTtSb.png" height="80%" width="80%"/> <br>

From here, we can find the user.txt and root.txt in the desktop of john & Administrator. <br>
<img src="https://i.imgur.com/1s3ZsSU.png" height="40%" width="40%"/> <br>
