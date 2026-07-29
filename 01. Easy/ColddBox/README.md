# 🧊 ColddBox - TryHackMe

## 📌 Information

| Field          | Value                                                     |
| -------------- | --------------------------------------------------------- |
| **Machine**    | ColddBox                                                  |
| **Platform**   | TryHackMe                                                 |
| **Difficulty** | Easy                                                      |
| **OS**         | Linux                                                     |
| **Category**   | Web Enumeration / CMS Exploitation / Privilege Escalation |

---

# 🎯 Objective

Gain root access to the Linux machine by enumerating a vulnerable web application, obtaining an initial shell, and escalating privileges.

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
| 4512 | SSH     | OpenSSH            |

### Notes

* The web server was the primary attack surface.
* SSH was available but required valid credentials.
* The website appeared to be running **ColdFusion CMS (ColddBox)**.

---

# 🔍 Enumeration

I performed directory enumeration to identify hidden resources.

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Interesting directories discovered:

```text
/admin
/login
/uploads
```

Further inspection identified the CMS version and revealed a publicly known vulnerability.

---

# 💥 Exploitation

A public exploit allowed authenticated remote code execution through the CMS.

After obtaining valid credentials during enumeration, I authenticated to the administration panel.

A PHP reverse shell was uploaded through the file management functionality.

Start a Netcat listener:

```bash
nc -lvnp 4444
```

Execute the uploaded payload.

### Result

A reverse shell was successfully obtained.

Verify the current user:

```bash
whoami
```

Output:

```text
www-data
```

---

# ⬆️ Privilege Escalation

Upgrade the shell:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Check sudo permissions:

```bash
sudo -l
```

During local enumeration, a binary with elevated privileges was identified.

Using the appropriate **GTFOBins** technique allowed privilege escalation to **root**.

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

* Web application reconnaissance.
* Directory brute forcing with Gobuster.
* CMS enumeration.
* Uploading and executing a reverse shell.
* Linux privilege enumeration.
* Exploiting sudo misconfigurations.
* Using GTFOBins for privilege escalation.

---

# 🛠️ Tools Used

* Nmap
* Gobuster
* Netcat
* Burp Suite (optional)
* GTFOBins
* Linux CLI

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
* Login page
* Admin dashboard
* Reverse shell upload
* Netcat listener
* Initial shell
* Sudo enumeration
* Root shell
* Flags

---

# ✅ Summary

ColddBox is a beginner-friendly Linux machine that focuses on web application enumeration, CMS exploitation, and Linux privilege escalation. The room demonstrates the importance of identifying vulnerable web applications, leveraging authenticated functionality to gain remote code execution, and performing local enumeration to obtain **root** privileges.
