# 🕵️ VulnNet: Internal - TryHackMe

## 📌 Information

| Field          | Value                                                             |
| -------------- | ----------------------------------------------------------------- |
| **Machine**    | VulnNet: Internal                                                 |
| **Platform**   | TryHackMe                                                         |
| **Difficulty** | Medium                                                            |
| **OS**         | Linux                                                             |
| **Category**   | Enumeration / Web Exploitation / SMB / SSH / Privilege Escalation |

---

# 🎯 Objective

Gain access to the internal Linux machine, enumerate hidden services, obtain a user shell, and escalate privileges to root to retrieve all flags.

---

# 🛰️ Reconnaissance

The first step was identifying exposed services using **Nmap**.

### Scan

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
```

### Results

| Port | Service | Version            |
| ---- | ------- | ------------------ |
| 22   | SSH     | OpenSSH            |
| 80   | HTTP    | Apache HTTP Server |
| 139  | SMB     | Samba              |
| 445  | SMB     | Samba              |

### Notes

* SSH was exposed but required valid credentials.
* A web application was running on port 80.
* SMB services were available and required further enumeration.

---

# 🔍 Enumeration

## Web Enumeration

Directory brute forcing was performed using Gobuster:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Interesting directories discovered:

```text
/internal
```

The directory contained an internal application with additional information.

---

## SMB Enumeration

Enumerate SMB shares:

```bash
smbclient -L //MACHINE_IP/ -N
```

Further enumeration:

```bash
enum4linux MACHINE_IP
```

### Findings

Available shares:

```text
anonymous
shares
```

Connecting to the share:

```bash
smbclient //MACHINE_IP/shares -N
```

Files containing useful information were discovered.

---

# 💥 Exploitation

During enumeration, credentials were discovered inside accessible files.

Example:

```text
username: <username>
password: <password>
```

Using the recovered credentials, SSH access was obtained:

```bash
ssh username@MACHINE_IP
```

### Result

A shell was successfully obtained.

Verify access:

```bash
whoami
```

Output:

```text
username
```

---

# ⬆️ Privilege Escalation

## Local Enumeration

Check sudo permissions:

```bash
sudo -l
```

Search for SUID binaries:

```bash
find / -perm -4000 2>/dev/null
```

Review running processes:

```bash
ps aux
```

During enumeration, a vulnerable binary or misconfiguration was identified.

Using the corresponding privilege escalation technique, root access was achieved.

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

```text
<user_flag>
```

## Root Flag

```text
<root_flag>
```

---

# 📚 What I Learned

* Performing internal network enumeration.
* Enumerating SMB shares.
* Discovering hidden web directories.
* Extracting credentials from exposed files.
* Using SSH for remote access.
* Performing Linux privilege escalation.

---

# 🛠️ Tools Used

* Nmap
* Gobuster
* smbclient
* enum4linux
* SSH
* Linux CLI

---

# 📖 MITRE ATT&CK

| Technique | Description                           |
| --------- | ------------------------------------- |
| T1046     | Network Service Discovery             |
| T1135     | Network Share Discovery               |
| T1087     | Account Discovery                     |
| T1552.001 | Credentials in Files                  |
| T1021.004 | SSH                                   |
| T1068     | Exploitation for Privilege Escalation |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* Web enumeration
* SMB shares
* Discovered credentials
* SSH login
* Local enumeration
* Privilege escalation
* Root shell
* Flags

---

# ✅ Summary

VulnNet: Internal is a medium-level Linux machine focused on internal enumeration and post-exploitation. The machine demonstrates the importance of discovering hidden services, analyzing exposed SMB shares, finding sensitive information, obtaining SSH access, and performing privilege escalation to achieve full system compromise.
