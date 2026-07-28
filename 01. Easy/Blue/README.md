# 🔵 Blue - TryHackMe

## Information

| Machine | Blue |
|---|---|
| Platform | TryHackMe |
| Difficulty | Easy |
| OS | Windows |
| Category | SMB / EternalBlue / Privilege Escalation |

---

## Objective

Gain administrator access to the Windows machine and retrieve the flags.

---

# 1. Reconnaissance

First, I started with an Nmap scan to discover open ports and services.

Command:

```bash
nmap -sC -sV -oN scan.txt MACHINE_IP
