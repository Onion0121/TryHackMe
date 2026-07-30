🔵 Kenobi - Notes

📝 Information

    Platform: TryHackMe
    OS: Linux
    Difficulty: Easy
    Main Vulnerabilities: SMB Enumeration / Anonymous FTP / NFS / SUID Privilege Escalation

🔎 Enumeration

Nmap

First, scan the target to identify open ports and services.

Command:

nmap -sC -sV -oN scan.txt MACHINE_IP

Important ports:

21/tcp    FTP
22/tcp    SSH
111/tcp   RPCBind
139/tcp   SMB
445/tcp   SMB
2049/tcp  NFS

Important findings:

    FTP allows anonymous access.
    SMB shares are exposed.
    NFS service is running.
    Possible privilege escalation through SUID.

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

    Look for configuration files or credentials.

🔍 SMB Enumeration

List SMB shares:

smbclient -L //MACHINE_IP -N

Connect to discovered share:

smbclient //MACHINE_IP/share_name -N

Look for:

    Sensitive files.
    User information.
    Configuration files.

📁 NFS Enumeration

Check NFS exports:

showmount -e MACHINE_IP

Mount available shares:

mkdir /tmp/share

mount MACHINE_IP:/share /tmp/share

Review files:

ls -la /tmp/share

Important:

    NFS misconfigurations can expose sensitive information.

💥 Exploitation

Main attack path:

    1. Enumerate FTP, SMB and NFS services.
    2. Find sensitive files through exposed shares.
    3. Obtain SSH credentials or useful information.
    4. Gain access to the machine.

SSH connection:

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

SUID Enumeration:

Find binaries with SUID permissions:

find / -perm -4000 2>/dev/null

Important finding:

    /usr/bin/menu

Check binary permissions:

ls -la /usr/bin/menu

Analyze the binary:

strings /usr/bin/menu

Exploit the vulnerable SUID binary to obtain root privileges.

After escalation:

whoami

Expected:

root

🚩 Flags Location

User:

/home/username/user.txt

Root:

/root/root.txt

🧠 Key Takeaways

    Enumerate all network services, not only web ports.
    Anonymous FTP can expose sensitive files.
    SMB and NFS shares may reveal credentials.
    SUID binaries are common Linux privilege escalation vectors.
    Always analyze custom binaries with strings and permissions.
