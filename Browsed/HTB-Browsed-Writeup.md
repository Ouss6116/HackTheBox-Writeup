---

# Conversor (Medium): 10.10.X.X

![Icon](Images/browsedicon.png)

---

## Quick overview

A Linux machine (Medium) that presented the following path:

1.  → → **USER.TXT**
2.  → → **ROOT.TXT**

---

## USER.TXT

### Enumeration

We start with a nmap enummeration that will help us to discover p22 and p80.

We try to get p80 via the IP adresse or browsed.htb (by ading it in /etc/hosts)

We will see an interessting entry point, by trying to upload of the samples (i choosed Fontify and i will used by all the writeup).

If we scan the displayed logs, we will find browsedinternals.htb, by adding it in /etc/hosts, we will find that host a gitea.
.

