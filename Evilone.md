For getting the services availabel:
```
Nmap -sV x.x.x.x
```
I got port 22 and port 80 open
When I go to port 80 on web I got the Apache index webpage.
Later I used gobuster with seclist for web discovery list.
```
Gobuster dir -r -u http://ip/ -w list -x HTML,php,txt
```
Gobuster on directory with -r to follow redirect 
(Feroxbuster might do the job$
I found ip/secret/ so I use gobuster again on it to find ip/secret/evil.php 

With ffuf I set a payload with a list to find any vulnerabilities for the /erc/passwd file
```
Ffuf -r -u http://ip/secret/evil.php?FUZZ=/etc/passwd -w list -fs
```
The machine had a ssh service, and a user called mowree 

We check for an id_rsa file on the user /home/mowree/.ssh/id_rsa 

We get the faile in our machine and set chmod 600 file

We use rockyou.txt password list and john to crack the password of the id_rsa 

We ssh into the server as mowree 
```
ssh mowree@ip -i id_rsa(the file in our machine)
```
With winpeas we realized that mowree user have writing privileges to the passwd file, so we set a password on the passwd type with openssl passwd -1

That password we use it to replace the x on the root user password.

With that we have access to root with su root
