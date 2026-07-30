🔵 VulnNet: Internal - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Easy
    Main Vulnerabilities: SMB Enumeration / Internal Services / Web Enumeration / Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

21/tcp   FTP
22/tcp   SSH
139/tcp  SMB
445/tcp  SMB

Important findings:

    SMB service exposed.
    FTP service available.
    Internal services may reveal useful information.

📂 FTP Enumeration

Check FTP access:

ftp MACHINE_IP

Try anonymous login:

anonymous

Enumerate files:

ls

Download interesting files:

get filename

Important:

    Search for credentials and configuration files.

🔍 SMB Enumeration

Enumerate SMB shares:

smbclient -L //MACHINE_IP -N

Check available shares:

smbclient //MACHINE_IP/share_name -N

Look for:

    User information.
    Backup files.
    Configuration files.
    Password hashes.

Important:

    SMB shares can contain sensitive internal information.

🌐 Web Enumeration

Check the web server:

http://MACHINE_IP

Directory enumeration:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Look for:

    Hidden directories.
    Internal applications.
    Backup files.
    Exposed credentials.

Check technologies:

whatweb http://MACHINE_IP

💥 Exploitation

Main attack path:

    1. Enumerate SMB shares.
    2. Discover internal files and information.
    3. Find credentials.
    4. Use credentials to access the system.

Possible SSH access:

ssh username@MACHINE_IP

After obtaining access:

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

SUID binaries:

find / -perm -4000 2>/dev/null

Cron jobs:

cat /etc/crontab

Writable files:

find / -writable -type f 2>/dev/null

Search for:

    Weak permissions.
    Misconfigured services.
    Sensitive credentials.
    Internal scripts.

Use discovered vulnerabilities to obtain root access.

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    Internal services often expose important information.
    SMB enumeration is critical when shares are available.
    Always inspect files found during enumeration.
    Credentials found in configuration files can provide access.
    After gaining access, always perform privilege escalation checks.
