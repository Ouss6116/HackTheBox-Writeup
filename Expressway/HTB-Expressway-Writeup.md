---

# Expressway (Easy): 10.10.11.87

![Icon](Images/expressicon.png)

---

## Quick Overview

easy linux machine without a web interface, so we get in via:

1. **UDP Port 69:user and 4500:password** → user:password → **USER.TXT**
2. **Sudo 1.9.17** → [Sudo 1.9.17 PrivEsc](https://github.com/kh4sh3i/CVE-2025-32463) → *Thanks to @kh4sh3i* → **ROOT.TXT**

---

## USER.TXT

### NMAP enumation

as usually we start with nmap, but we get only p22, and no helpful information.

![Icon](Images/nmaptcpscan.png)

so we try and UDP scan, and as you see we get some helpful information.

![Icon](Images/nmaudpscan.png)

so we will explore p69 and p500 with the help of [hacktricks](https://book.hacktricks.wiki/en/index.html)

### Port 69 and TFTP explore:

the Port 69 host a TFTP service, we have a valuable help in [69 - UDP TFTP](https://book.hacktricks.wiki/en/network-services-pentesting/69-udp-tftp.html)

so firt of all we will enumarte it with `nmap -n -Pn -sU -p69 -sV --script tftp-enum 10.10.11.87` and we get this file **ciscortr.cfg**

and by using metasploit `msf5> auxiliary/admin/tftp/tftp_transfer_util` we can get that file.

there a lot of valuable info, and i took from there the **user: ike**

### Port 500 and IKE explore:

as we did before, we will get help from [500/udp - Pentesting IPsec/IKE VPN](https://book.hacktricks.wiki/en/network-services-pentesting/ipsec-ike-vpn-pentesting.html)




