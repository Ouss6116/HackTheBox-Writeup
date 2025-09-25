---

# Expressway (Easy): 10.10.11.87

---

## Quick Overview

An easy Linux machine without a web interface. Initial access is gained via:

1. **UDP Port 69 & 500 Enumeration** → Credentials found → **USER.TXT**
2. **Sudo 1.9.17 Privilege Escalation** → [CVE-2025-32463 Exploit ](https://github.com/kh4sh3i/CVE-2025-32463) → *Thanks to @kh4sh3i* → **ROOT.TXT**

---

## USER.TXT

### NMAP Enumeration

We started with a standard TCP nmap scan, which only revealed port 22 (SSH) and no immediately helpful information.

A UDP scan revealed more promising services, so we focused our efforts on ports 69 and 500 with the help of [HackTricks](https://book.hacktricks.wiki/en/index.html)

### Port 69 and TFTP Exploration

Port 69 hosted a TFTP service. Following the guide for [69 - UDP TFTP](https://book.hacktricks.wiki/en/network-services-pentesting/69-udp-tftp.html), we enumerated it using: `nmap -n -Pn -sU -p69 -sV --script tftp-enum 10.10.11.87` 

This scan revealed a configuration file: **ciscortr.cfg**.

We downloaded this file using the Metasploit module: `msf5 > use auxiliary/admin/tftp/tftp_transfer_util`

The configuration file contained a wealth of information, including the username: **ike**.

### Port 500 and IKE Exploration

Next, we examined the IKE service on port 500, referencing the [500/udp - Pentesting IPsec/IKE VPN](https://book.hacktricks.wiki/en/network-services-pentesting/ipsec-ike-vpn-pentesting.html) guide.

We first checked if the server was using Aggressive Mode, which can expose a hash of the pre-shared key (PSK): : `ike-scan -A -M 10.10.11.87 -P crack`

We then attempted to crack the captured hash using **psk-crack** and the **rockyou.txt** wordlist: : `psk-crack -d /usr/share/wordlists/rockyou.txt crack`

This successfully revealed the password: **freakingrockstarontheroad**.

### Final Touch:

Using the credentials **ike:freakingrockstarontheroad**, we logged in via SSH and captured the **USER.TXT flag**.

---

## ROOT.TXT

For privilege escalation, we began by searching for **SUID binaries**: : `find / -type f -perm -u=s 2>/dev/null`

An interesting finding was `/usr/local/bin/sudo`. Checking its version revealed **Sudo version 1.9.17**.

A quick search for this version identified **CVE-2025-32463**. We used the exploit from [kh4sh3i's GitHub repository](thttps://github.com/kh4sh3i/CVE-2025-32463) to escalate our privileges to root and capture the **ROOT.TXT flag**.

---
