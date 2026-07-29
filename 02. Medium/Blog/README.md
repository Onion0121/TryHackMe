# 📝 Blog - TryHackMe

## 📌 Information

| Field          | Value                                                         |
| -------------- | ------------------------------------------------------------- |
| **Machine**    | Blog                                                          |
| **Platform**   | TryHackMe                                                     |
| **Difficulty** | Medium                                                        |
| **OS**         | Linux                                                         |
| **Category**   | WordPress / Enumeration / Exploitation / Privilege Escalation |

---

# 🎯 Objective

Compromise the WordPress-based Linux machine, obtain user access, and escalate privileges to root to retrieve all flags.

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
| 22   | SSH     | OpenSSH            |
| 80   | HTTP    | Apache HTTP Server |

### Notes

* The web server was the main attack surface.
* The website was running **WordPress**.
* Further enumeration was required to identify vulnerable components.

---

# 🔍 Enumeration

## Web Enumeration

Directory scanning was performed using Gobuster:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Interesting paths:

```text
/wp-admin
/wp-content
/wp-login.php
```

---

## WordPress Enumeration

WordPress information was gathered using WPScan:

```bash
wpscan --url http://MACHINE_IP --enumerate u,p
```

Findings:

* WordPress version identified.
* Valid usernames discovered.
* Vulnerable plugins/themes were detected.

---

# 💥 Exploitation

A vulnerable WordPress component was identified.

Using the discovered information, access was obtained through exploitation of the vulnerable plugin/theme.

A reverse shell payload was prepared:

```bash
nc -lvnp 4444
```

After execution:

```text
Reverse shell received
```

Initial access was obtained as:

```bash
whoami
```

Output:

```text
www-data
```

---

# ⬆️ Privilege Escalation

Local enumeration was performed:

```bash
sudo -l
```

Search for interesting files:

```bash
find / -type f -name "*.txt" 2>/dev/null
```

Sensitive information was discovered inside WordPress files:

```bash
wp-config.php
```

The database credentials were recovered and reused to obtain further access.

After enumeration, a privilege escalation path was identified.

Verify root access:

```bash
id
```

Output:

```text
uid=0(root)
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

* WordPress enumeration with WPScan.
* Identifying vulnerable plugins.
* Web exploitation techniques.
* Extracting credentials from configuration files.
* Linux privilege escalation methodology.

---

# 🛠️ Tools Used

* Nmap
* Gobuster
* WPScan
* Netcat
* Linux CLI

---

# 📖 MITRE ATT&CK

| Technique | Description                       |
| --------- | --------------------------------- |
| T1046     | Network Service Discovery         |
| T1190     | Exploit Public-Facing Application |
| T1505.003 | Web Shell                         |
| T1552.001 | Credentials in Files              |
| T1068     | Privilege Escalation              |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* WordPress page
* WPScan results
* Exploit execution
* Reverse shell
* Credential discovery
* Root shell
* Flags

---

# ✅ Summary

Blog is a medium-level Linux machine focused on WordPress exploitation. It demonstrates the importance of web enumeration, identifying vulnerable CMS components, obtaining initial access, extracting sensitive information, and escalating privileges to achieve full system compromise.
