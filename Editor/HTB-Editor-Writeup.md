# Editor (Easy) : 10.10.11.80
A simple linux machine that can be resumed in two points:
  1. xwiki 15.10.8 => [CVE-2025-24893](https://github.com/Artemir7/CVE-2025-24893-EXP) => USER.TXT
  2. netdata 1.45.2 => [CVE-2024-32019](https://github.com/AzureADTrent/CVE-2024-32019-POC) => ROOT.TXT

## USER.TXT
### Enum and the entry point:
As always we start with port scan **nmap** and dir/file/sub discovery **ffuf**

We will be interessted in **port 8080** or subdomaine **wiki**, theey are the two faces of the same coin: **xwiki**

with a quick look we found xwiki **version 15.10.8** that leat as to the following RCE exploit [CVE-2025-24893](https://github.com/Artemir7/CVE-2025-24893-EXP)


### Exploiting the RCE:
So by cloning the exploit, we can start using it.

**PS: it will be helpful if you add /xwiki/ in the link in the script.**

so we can start by testing commands like `id` or `whoami`, if its work we go further.

first thing to find is a user : `python testou.py -u http://wiki.editor.htb -c "ls /home/"` => oliver

and then a password that have been in xwiki config file, generraly if there a login option that mean there are stored password somewhere.

in our case was in **/etc/xwiki/hibernate.cfg.xml** by the command : `python testou.py -u http://wiki.editor.htb -c "cat /etc/xwiki/hibernate.cfg.xml"` => theEd1t0rTeam99

### Thank you oliver :

so by the creds oliver:theEd1t0rTeam99 we get the **USER.TXT**

## ROOT.TXT
###First Steps:

Lets start with **SUID** : `find / -perm -4000 -type f 2>/dev/null` , we can see that we have access to **Netdata**.

than we see the **accessible local ports** : `ss -tulnp` , we find **port 19999** , and its the one that Netdata use.

Next step **SSH port forwarding** : `ssh -L 7777:localhost:19999 oliver@editor.htb`.

###Netdata exploit

By accessing netdata web page we found an alert of outdated agent with **version 1.45.2**, and that lead us to this PrivEscal exploit [CVE-2024-32019](https://github.com/AzureADTrent/CVE-2024-32019-POC). 

by following the exploit, and use this command at the end `/opt/netdata/usr/libexec/netdata/plugins.d/ndsudo nvme-list`, we escal to root and get the **ROOT.TXT**



