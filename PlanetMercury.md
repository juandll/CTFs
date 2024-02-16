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
