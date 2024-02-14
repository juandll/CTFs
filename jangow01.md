I ran the following command for information on the services
```
nmap -sV IP
```
From there i got:
21/tcp open  ftp     vsftpd 3.0.3
80/tcp open  http    Apache httpd 2.4.18

from here we look up both services, first on ftp that version supports anonymous login, but in the server it looks unavailable. So now is time to use the web server.
The index contains the actual site, ip/site/ but we will run a discovery script from the ip/

gobuster dir -r -u http://ip/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt,conf

In the meantime of the gobuster analisys i go into the webpage and explore. I found out a look up button that after pressed takes me into this url: http://IP/site/busque.php?buscar=
After looking at it for a while, i came to find out that is a command line. 

wordpress config file (wordpress/config.php)
when looking into the /etc/passwd file and others I can go into, I thought about looking into the files owned by the user in control of the consol. after this I looked into a lot of files, but I found a hidden file, /var/www/html/.backup 
looking into this file, i got:

$servername = "localhost"; $database = "jangow01"; $username = "jangow01"; $password = "abygurl69"; // Create connection $conn = mysqli_connect($servername, $username, $password, $database); // Check connection if (!$conn) { die("Connection failed: " . mysqli_connect_error()); } echo "Connected successfully"; mysqli_close($conn); 

after this I can comprimse the FTP server, by having the user and pass of an user. Now I am looking into the Linux version (4.4.0) to see if there is a vulnerability for this version.

  
