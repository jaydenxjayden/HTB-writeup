# Lame

This is the write-up for the box Lame

I start by adding this box into my /etc/hosts file:
```markdown
10.10.10.3    lame.htb
```

## Enumeration

Starting with a general Nmap scan on all ports, followed by a detailed scan on the open ports found:

```markdown
nmap 10.10.10.3 -Pn -p-
```

```markdown
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds    
3632/tcp open  distccd
```

Saving the output of the detailed nmap scan.

```markdown
nmap 10.10.10.3 -Pn -sC -sV -p21,22,139,445,3632 -oA lame
```

```markdown
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.24
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
3632/tcp open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)
|_clock-skew: mean: 1h51m01s, deviation: 2h49m45s, median: -9m01s
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2024-08-19T09:22:59-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
```
From the output of the nmap scan, I tried to identify potential vulnerable services. <br>
<img src="https://i.imgur.com/hrae59u.png" height="80%" width="80%" alt="lame"/> <br>

### Checking FTP (Port 21)

Starting with FTP, I tried logging in using anonymous credentials to see if there are potential attack vectors that can give me access into the system. <br>
<img src="https://i.imgur.com/xS3ES73.png" height="80%" width="80%" alt="lame"/> <br>

I tried using searchsploit to check if this version of vsftpd is vulnerable and found that potential exploits that can give me backdoor command execution. <br>
<img src="https://i.imgur.com/Huppf8n.png" height="80%" width="80%" alt="lame"/> <br>

I opened up **msfconsole** to search for the exploit.
<img src="https://i.imgur.com/TolcRuq.png" height="80%" width="80%" alt="lame"/> <br>

Selecting the exploit, setting the required options and running it did not give me a session. It was unsuccessful. <br>
<img src="https://i.imgur.com/RoatjkR.png" height="80%" width="80%" alt="lame"/> <br>

### Checking SSH (Port 22)

Moving onto the next service; ssh, I used searchsploit again to check if there are exploits available. <br>
After spending way too much time here trying to get the different scripts to run, I realised that it might be a rabbit hole and decide to move on to the other services. <br>
<img src="https://i.imgur.com/9p3okHv.png" height="80%" width="80%" alt="lame"/> <br>

### Checking SMB (Port 139 & 445)

The next service would be samba service, after searching for the version of samba using searchsploit, I found a potential exploit.
<img src="https://i.imgur.com/oM33ypF.png" height="80%" width="80%" alt="lame"/> <br>

## Exploiting the box

Using **msfconsole** to set the required fields and to run the exploit.
<img src="https://i.imgur.com/2u2Ef8a.png" height="80%" width="80%" alt="lame"/> <br>

Running the exploit gives me a shell session with root access, which I can proceed to retrieve the flag in root & user's home directory. <br>
<img src="https://i.imgur.com/5l61kU7.png" height="80%" width="80%" alt="lame"/> <br>
