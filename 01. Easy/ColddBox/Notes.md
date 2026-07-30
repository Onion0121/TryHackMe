🔵 ColddBox - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Easy
    Main Vulnerabilities: WordPress Enumeration / Credential Discovery / SSH / Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

80/tcp   HTTP
4512/tcp SSH

Important findings:

    Web server running on port 80.
    SSH service available on a non-standard port.

🌐 Web Enumeration

Access the website:

http://MACHINE_IP

Check for:

    CMS information.
    Hidden directories.
    Exposed files.
    User information.

Directory enumeration:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Important:

    Always check robots.txt.

Command:

curl http://MACHINE_IP/robots.txt

🔍 WordPress Enumeration

Identify WordPress installation.

Check:

    Login panel.
    Users.
    Plugins.
    Themes.

Useful tool:

wpscan --url http://MACHINE_IP --enumerate u

Look for:

    Valid usernames.
    Vulnerable plugins.
    Information disclosure.

💥 Exploitation

Main attack path:

    1. Enumerate WordPress.
    2. Discover valid usernames.
    3. Find credentials.
    4. Access the system through SSH.

SSH connection:

ssh username@MACHINE_IP -p 4512

After obtaining access:

Check current user:

whoami

🖥️ Post Exploitation

System information:

uname -a

Check user permissions:

sudo -l

Look for available commands that can run with elevated privileges.

Check home directories:

ls /home

🔼 Privilege Escalation

Enumerate possible escalation paths:

Sudo permissions:

sudo -l

SUID binaries:

find / -perm -4000 2>/dev/null

Search for:

    Misconfigured sudo rules.
    Writable files.
    Weak permissions.
    Sensitive credentials.

Use the discovered misconfiguration to obtain root access.

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    WordPress enumeration can reveal usernames and attack paths.
    Always check robots.txt and hidden directories.
    Non-standard SSH ports are common in CTF machines.
    After gaining SSH access, enumerate sudo permissions.
    Privilege escalation depends on finding system misconfigurations.
