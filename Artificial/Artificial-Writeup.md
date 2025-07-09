# Artificial (Easy) : 10.10.11.74
## USER.TXT
### Enum and first entry point:
We start with enum using Nmap (ports) => P22/80 => we add artificial.htb in /etc/hosts
then use Ffuf (Dir, Files and Sub) => /login is the most interesting, this will be our entry point
### exploiting the entry point:
via /login we find the /dashboard page
two main things : 
  1/ requirement: tensorflow-cpu==2.13.1
  2/ upload filter: .h5 
we can use https://github.com/Splinter0/tensorflow-rce to generate reverse shell with exploit.py and nc from our local machine.
we get in as app, small check we found another user: gael
we use find command to find something interesting : sudo find / -type f -name "*.db" 2>/dev/null
we find a db: 1|gael|gael@artificial.htb|c99175974b6e192936d97224638a34f8
and getting the password via crackstation.com : c99175974b6e192936d97224638a34f8 => mattp005numbertwo
with the creds gael:mattp005numbertwo we get the user.txt


