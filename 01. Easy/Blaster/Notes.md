🔵 Blaster - Notes

📝 Information

    Platform: TryHackMe
    OS: Windows
    Difficulty: Easy
    Main Vulnerabilities: RDP / Hidden Directories / Windows Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

80/tcp    HTTP
3389/tcp  RDP

Important findings:

    Windows machine detected.
    RDP service exposed.
    Web server available for enumeration.

🌐 Web Enumeration

Access the web server:

http://MACHINE_IP

Enumerate directories:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Look for:

    Hidden directories
    Sensitive files
    User information

Important:

    Hidden files or directories may contain credentials.

🔐 RDP Enumeration

Check Remote Desktop service:

nmap --script rdp-enum-encryption -p 3389 MACHINE_IP

Important:

    RDP is enabled.
    Credentials are required for access.

👤 Credential Discovery

Search for exposed credentials through web enumeration.

Possible locations:

    Hidden files
    Backup files
    Web directories

After obtaining credentials:

Connect using RDP:

xfreerdp /u:USERNAME /p:PASSWORD /v:MACHINE_IP

🖥️ Post Exploitation

After gaining RDP access:

Check current user:

whoami

System information:

systeminfo

Check privileges:

whoami /priv

Look for privilege escalation methods.

🔼 Privilege Escalation

Common checks:

Check users:

net users

Check groups:

net localgroup administrators

Search for vulnerable services or misconfigurations.

Tools that can help:

winPEAS

Command:

winpeas.exe

Review:

    Installed software
    Services
    Scheduled tasks
    Stored credentials

🚩 Flags Location

User:

C:\Users\USERNAME\Desktop\user.txt

Root:

C:\Users\Administrator\Desktop\root.txt

🧠 Key Takeaways

    Always enumerate web services before attacking.
    RDP can provide full graphical access when valid credentials are found.
    Hidden directories may expose sensitive information.
    After obtaining access, always check privileges.
    Windows privilege escalation requires checking misconfigurations.
