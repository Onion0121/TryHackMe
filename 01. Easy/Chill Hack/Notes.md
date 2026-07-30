🔵 Chill Hack - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Easy
    Main Vulnerabilities: Web Enumeration / File Upload / Command Injection / Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

21/tcp   FTP
22/tcp   SSH
80/tcp   HTTP

Important findings:

    FTP service available.
    SSH service enabled.
    Web application running on port 80.

📂 FTP Enumeration

Check FTP access:

ftp MACHINE_IP

Try anonymous login:

anonymous

Important:

    Look for accessible files.
    Download interesting files for analysis.

Commands:

ls

get filename

🌐 Web Enumeration

Access the web server:

http://MACHINE_IP

Enumerate directories:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Look for:

    Hidden directories.
    Upload functionality.
    Backup files.
    Source code information.

Check technologies:

whatweb http://MACHINE_IP

🔍 File Analysis

Analyze discovered files:

cat filename

Look for:

    Usernames.
    Passwords.
    Configuration files.
    Hidden information.

Important:

    Exposed credentials can allow SSH access.

💥 Exploitation

Possible attack path:

    1. Enumerate FTP and web services.
    2. Discover hidden files/directories.
    3. Find credentials or vulnerable functionality.
    4. Obtain a reverse shell.

After gaining a shell:

Upgrade shell:

python3 -c 'import pty; pty.spawn("/bin/bash")'

Check current user:

whoami

🖥️ Post Exploitation

System information:

uname -a

Check users:

cat /etc/passwd

Check sudo permissions:

sudo -l

Look for privilege escalation opportunities.

🔼 Privilege Escalation

Enumerate:

Sudo permissions:

sudo -l

SUID binaries:

find / -perm -4000 2>/dev/null

Cron jobs:

cat /etc/crontab

Search for:

    Weak permissions.
    Misconfigured services.
    Password files.
    Writable directories.

Use discovered vulnerabilities to obtain root access.

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    Always enumerate every exposed service.
    FTP may contain sensitive files.
    Web applications often reveal credentials.
    Never ignore file analysis during enumeration.
    After getting a shell, always perform privilege escalation checks.
