# CodeTwo (Easy): 10.10.11.80

![Icon](Images/codetwoicon.png)

## Quick Overview

A very easy peasy quick machine, that can be pawnd in the two next steps:
1. js2py 0.74 => [CVE-2024-28397](https://github.com/waleed-hassan569/CVE-2024-28397-command-execution-poc) - Thanks to @waleed-hassan569 => USER.TXT
2. npbackup 3.0.1 => [npbackup3.0.1](https://github.com/AliElKhatteb/npbackup-cli-priv-escalation) - Thanks to @AliElKhatteb => ROOT.TXT

## USER.TXT
### Enumeration and the Entry Point:

We start with nmap that will lead us to port 8000.

### Exploiting the RCE:

there we will go on two steps :
1.  Download the app that will help to discover **js2py==0.74** in **requiement file**, and will exploit it with RCE [CVE-2024-28397](https://github.com/waleed-hassan569/CVE-2024-28397-command-execution-poc)
2.  in dir instance, there is a **users.db**, remember it will be important to check it later.
3.  after register we will exploit js2py to get user info from user.db : `sqlite3 instance/users.db 'SELECT * FROM user;'`

### thank you marco:

after getting marco creds, simple [CrackSation](https://crackstation.net/) use to crack the passowrd, and get the **USER.TXT**

## ROOT.TXT

### First discovery:

lets start with `sudo -l` , we see the privelege for **npbackup 3.0.1**

a small research to find the [PRivEscalPOC](https://github.com/AliElKhatteb/npbackup-cli-priv-escalation)

### PrivEscal with npbackup:

following the instruction, and we will get the **ROOT.TXT**

`sudo /usr/local/bin/npbackup-cli -c npbackup.conf --backup` => creat backup

`sudo /usr/local/bin/npbackup-cli -c npbackup.conf -s` => list snap id

`sudo /usr/local/bin/npbackup-cli -c npbackup.conf --dump FILE --snapshot-id SNAP ID` => read the file in backup
