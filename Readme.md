## Evilone
Evilone had a ssh and http service, on http there was a file (evil.php) that will look into files. With this we get the id_rsa file of the user with ssh. After we ssh, we realized that the /etc/passwd file was writable by the user, so we encripted a password into paasswd with openssl and put the encrypted password replacing the X on the root user. After that our password will work for the root user.
## Jangow01
this is a VM with http and FTP. Discovery was the key to get the site/.backup file which contains the credentials to an user. After that, we did revers shell and LinPeas and a CV for the kernel version.
