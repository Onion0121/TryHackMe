🔵 Ignite - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Easy
    Main Vulnerabilities: Fuel CMS / Default Credentials / Remote Code Execution / Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

80/tcp   HTTP

Important findings:

    Web server running on port 80.
    Fuel CMS detected.
    Possible default credentials or known vulnerabilities.

🌐 Web Enumeration

Access the website:

http://MACHINE_IP

Check:

    Page source.
    Robots.txt.
    Hidden directories.
    CMS information.

Directory enumeration:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Important:

    Look for Fuel CMS administration panels.
    Identify CMS version.

Check technologies:

whatweb http://MACHINE_IP

🔍 Fuel CMS Enumeration

Fuel CMS administration panel:

/fuel

Try default credentials:

Username:

admin

Password:

admin

Important:

    Default credentials may provide administrator access.

💥 Exploitation

Main vulnerability:

    Fuel CMS Remote Code Execution

Search for known exploits:

searchsploit fuel cms

Possible exploitation:

    Access the Fuel CMS admin panel.
    Use vulnerable functionality to execute commands.
    Obtain a reverse shell.

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

Look for:

    Misconfigured sudo commands.
    Writable files.
    Stored credentials.

🔼 Privilege Escalation

Enumerate:

SUID binaries:

find / -perm -4000 2>/dev/null

Cron jobs:

cat /etc/crontab

Check sudo:

sudo -l

Search for:

    Weak permissions.
    Password files.
    Sensitive configuration files.

Use discovered vulnerabilities to escalate privileges.

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    Always identify the technologies running on web servers.
    Default credentials are one of the first things to test.
    CMS versions should always be checked for known vulnerabilities.
    Remote Code Execution can provide initial access.
    After obtaining a shell, always perform privilege escalation enumeration.
