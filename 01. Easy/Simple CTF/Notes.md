🔵 Simple CTF - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Easy
    Main Vulnerabilities: Web Enumeration / CMS Exploitation / SSH / Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

21/tcp   FTP
80/tcp   HTTP
2222/tcp SSH

Important findings:

    FTP service available.
    Web server running on port 80.
    SSH running on a non-standard port.

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

    Search for credentials or sensitive information.

🌐 Web Enumeration

Access the website:

http://MACHINE_IP

Check:

    Page source.
    Hidden directories.
    CMS information.
    Robots.txt.

Directory enumeration:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Important:

    Identify the web technology and possible CMS.

🔍 CMS Enumeration

Identify the CMS running on the website.

Check version information:

    CMS Made Simple detected.

Search for vulnerabilities:

searchsploit CMS Made Simple

Important:

    Vulnerable CMS versions may allow SQL injection or remote code execution.

💥 Exploitation

Main attack path:

    1. Enumerate services.
    2. Identify CMS version.
    3. Exploit the CMS vulnerability.
    4. Obtain credentials.
    5. Access SSH.

SSH connection:

ssh username@MACHINE_IP -p 2222

After obtaining access:

Check current user:

whoami

🖥️ Post Exploitation

System information:

uname -a

Check sudo permissions:

sudo -l

Look for:

    Commands allowed with sudo.
    Misconfigured permissions.
    Possible privilege escalation paths.

🔼 Privilege Escalation

Enumerate the system:

SUID binaries:

find / -perm -4000 2>/dev/null

Check sudo permissions:

sudo -l

Search for:

    Writable files.
    Cron jobs.
    Weak configurations.

Use the discovered vulnerability to obtain root privileges.

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    Always enumerate web applications and their versions.
    CMS vulnerabilities can provide initial access.
    FTP and SSH services may reveal attack paths.
    Non-standard ports are common in CTF machines.
    After gaining access, always enumerate privilege escalation vectors.
