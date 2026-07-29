# 🧊 Chill Hack - TryHackMe

## 📌 Information

| Field          | Value                                                                |
| -------------- | -------------------------------------------------------------------- |
| **Machine**    | Chill Hack                                                           |
| **Platform**   | TryHackMe                                                            |
| **Difficulty** | Easy                                                                 |
| **OS**         | Linux                                                                |
| **Category**   | Web Enumeration / File Upload / Steganography / Privilege Escalation |

---

# 🎯 Objective

Gain access to the Linux machine by discovering hidden information, exploiting a vulnerable web application, and escalating privileges to obtain the root flag.

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
| 22   | SSH     | OpenSSH            |
| 80   | HTTP    | Apache HTTP Server |

### Notes

* FTP was exposed and required further enumeration.
* A web application was running on port 80.
* SSH access would require valid credentials.

---

# 🔍 Enumeration

## Web Enumeration

Directory brute forcing was performed:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Interesting directories discovered:

```text
/secret
/uploads
```

The `/secret` directory contained hidden information useful for obtaining credentials.

---

## File Analysis

A suspicious file was discovered and downloaded for analysis.

Using file identification:

```bash
file suspicious_file
```

Steganography analysis was performed with:

```bash
steghide info image.jpg
```

Extract hidden data:

```bash
steghide extract -sf image.jpg
```

The extracted information revealed credentials.

---

# 💥 Exploitation

Using the discovered credentials, SSH access was obtained:

```bash
ssh username@MACHINE_IP
```

### Result

A shell was successfully obtained.

Verify the current user:

```bash
whoami
```

Output:

```text
username
```

---

# ⬆️ Privilege Escalation

## Enumeration

Check sudo permissions:

```bash
sudo -l
```

The user was allowed to execute a command with elevated privileges.

Further enumeration:

```bash
find / -perm -4000 2>/dev/null
```

A vulnerable configuration was identified.

Using the appropriate privilege escalation technique, root access was achieved.

Verify:

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

* Performing web enumeration.
* Discovering hidden directories.
* Analyzing files for hidden information.
* Using steganography tools.
* Extracting credentials from exposed data.
* Using SSH for remote access.
* Performing Linux privilege escalation.

---

# 🛠️ Tools Used

* Nmap
* Gobuster
* Steghide
* SSH
* Linux CLI

---

# 📖 MITRE ATT&CK

| Technique | Description                  |
| --------- | ---------------------------- |
| T1046     | Network Service Discovery    |
| T1083     | File and Directory Discovery |
| T1552.001 | Credentials in Files         |
| T1021.004 | SSH                          |
| T1548.003 | Sudo and Sudo Caching        |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* Gobuster results
* Hidden directory discovery
* Steghide extraction
* Credentials found
* SSH connection
* Sudo permissions
* Root shell
* Flags

---

# ✅ Summary

Chill Hack is a beginner-friendly Linux machine that focuses on information gathering and enumeration. It introduces techniques such as hidden directory discovery, steganography analysis, credential extraction, SSH access, and Linux privilege escalation. The machine demonstrates how small information leaks can lead to complete system compromise.
