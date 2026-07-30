🔵 Blog - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Medium
    Main Vulnerabilities: WordPress / CVE Exploitation / File Upload / Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

22/tcp   SSH
80/tcp   HTTP

Important findings:

    Web server running.
    SSH service available.
    Possible CMS installation detected.

🌐 Web Enumeration

Access the website:

http://MACHINE_IP

Enumerate directories:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Check:

    robots.txt
    Hidden directories
    Backup files
    Source code comments

Command:

curl http://MACHINE_IP/robots.txt

🔍 WordPress Enumeration

Identify WordPress:

wpscan --url http://MACHINE_IP

Enumerate users:

wpscan --url http://MACHINE_IP --enumerate u

Look for:

    WordPress version.
    Vulnerable plugins.
    Usernames.
    Exposed information.

💥 Exploitation

Main attack path:

    1. Enumerate WordPress.
    2. Discover vulnerable plugins or themes.
    3. Exploit the vulnerability.
    4. Obtain a shell.

Possible exploitation methods:

    WordPress plugin vulnerabilities.
    Malicious file upload.
    Remote Code Execution.

After obtaining access:

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

Search for:

    Credentials.
    Configuration files.
    Backup files.

🔼 Privilege Escalation

Enumerate privilege escalation vectors:

SUID binaries:

find / -perm -4000 2>/dev/null

Cron jobs:

cat /etc/crontab

Writable files:

find / -writable -type f 2>/dev/null

Look for:

    Vulnerable binaries.
    Weak permissions.
    Misconfigured services.

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    WordPress enumeration is essential for CMS-based machines.
    Always identify versions before exploitation.
    Plugins and themes can introduce vulnerabilities.
    After gaining access, enumerate the Linux system.
    Privilege escalation requires finding misconfigurations.
