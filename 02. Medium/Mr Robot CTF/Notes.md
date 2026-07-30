🔵 Mr Robot CTF - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Medium
    Main Vulnerabilities: WordPress / Hidden Files / Credential Discovery / Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

22/tcp   SSH
80/tcp   HTTP
443/tcp  HTTPS

Important findings:

    Web server running.
    WordPress installation detected.
    HTTPS enabled.

🌐 Web Enumeration

Access:

http://MACHINE_IP

Enumerate directories:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Important files:

robots.txt

Check:

curl http://MACHINE_IP/robots.txt

Look for:

    Hidden directories.
    Interesting files.
    Wordlists.
    Credentials.

🔍 WordPress Enumeration

Identify WordPress:

wpscan --url http://MACHINE_IP

Enumerate users:

wpscan --url http://MACHINE_IP --enumerate u

Check:

    Plugins.
    Themes.
    WordPress version.
    Usernames.

📂 Credential Discovery

Search for exposed files.

Important:

    Hidden files may contain password hashes or keys.

Example:

cat fsocity.dic

Use discovered information for password attacks.

Password cracking:

john --wordlist=passwords.txt hash.txt

💥 Exploitation

Main attack path:

    1. Enumerate WordPress.
    2. Discover usernames and password hashes.
    3. Obtain WordPress access.
    4. Upload a reverse shell.
    5. Gain system access.

Reverse shell:

Create payload and upload through WordPress.

Start listener:

nc -lvnp 4444

After obtaining access:

Upgrade shell:

python3 -c 'import pty; pty.spawn("/bin/bash")'

🖥️ Post Exploitation

Check current user:

whoami

Search for flags:

find / -name "key*" 2>/dev/null

Check system:

uname -a

🔼 Privilege Escalation

Check sudo permissions:

sudo -l

Search SUID binaries:

find / -perm -4000 2>/dev/null

Look for:

    Weak permissions.
    Vulnerable binaries.
    Stored credentials.

Exploit available privilege escalation method to obtain root.

🚩 Flags Location

Key 1:

/key-1-of-3.txt

Key 2:

/home/robot/key-2-of-3.txt

Key 3:

/root/key-3-of-3.txt

🧠 Key Takeaways

    Always check robots.txt during web enumeration.
    WordPress vulnerabilities are common attack vectors.
    Hidden files can reveal important information.
    Password cracking may be required when hashes are discovered.
    Always enumerate privileges after obtaining access.
