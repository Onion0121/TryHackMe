🔵 Bolt - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Easy
    Main Vulnerabilities: Web Enumeration / CMS Exploitation / File Upload / Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

22/tcp   SSH
80/tcp   HTTP

Important findings:

    SSH service available.
    Web application running on port 80.

🌐 Web Enumeration

Access the web server:

http://MACHINE_IP

Look for:

    Website technologies.
    CMS information.
    Hidden directories.
    Possible usernames.

Directory enumeration:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Important:

    Always enumerate web directories.
    Check for exposed files and configuration information.

🔍 CMS Enumeration

Identify the CMS running on the website.

Look for:

    Version information.
    Plugins.
    Themes.
    Default files.

Useful tools:

whatweb http://MACHINE_IP

Search for known vulnerabilities based on the CMS version.

💥 Exploitation

Main attack path:

    1. Enumerate the web application.
    2. Identify vulnerable CMS components.
    3. Exploit the vulnerability.
    4. Obtain a shell on the machine.

After obtaining access, stabilize the shell:

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

Search for:

    Misconfigured sudo permissions.
    SUID binaries.
    Writable files.
    Cron jobs.

SUID enumeration:

find / -perm -4000 2>/dev/null

Check scheduled tasks:

cat /etc/crontab

Use discovered misconfigurations to escalate privileges.

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    Web enumeration is essential before exploitation.
    Identify technologies and versions running on websites.
    CMS vulnerabilities can provide initial access.
    Always enumerate permissions after obtaining a shell.
    Linux privilege escalation depends on finding misconfigurations.
