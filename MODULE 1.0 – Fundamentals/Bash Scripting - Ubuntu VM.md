Projects:
1. Create a script that prints out the most commonly needed nmap commands based off of user input for IP(s) and port(s)
List of commonly needed nmap commands:
	-  Host Discovery (Ping Sweep)
		- nmap -sn 192.168.1.0/24
	-  Service & Version Detection
		- nmap -sV 192.168.1.10
	- OS Detection & Aggressive Scan
		- nmap -A 192.168.1.10
		- nmap -O 192.168.1.10
	- Specific Port or Range Scan
		- nmap -p 22,80,443 192.168.1.10
		- nmap -p 1-1000 192.168.1.10
	- Stealth SYN Scan
		- nmap -sS 192.168.1.10
	- Using Nmap Scripting Engine
		- nmap -sC 192.168.1.10
		- nmap --script=http-title 192.168.1.10
	- Output options
		- nmap -oN output.txt 192.168.1.10 # Normal text
		- nmap -oX output.xml 192.168.1.10 # XML format

```
!/bin/bash

echo "Choose scan type"

echo -e "\n1.)Host Discovery (Ping Sweep)"
echo -e "\n2.)Service & Version Detection"
echo -e "\n3.)OS Detection & Aggressive Scan"
echo -e "\n4.)Specific Port or Range Scan"
echo -e "\n5.)Stealth SYN Scan"
echo -e "\n6.)Using Nmap Scripting Engine"
read Scan_type

echo $Scan_type

if [$Scan_type == 1];then
        echo "I did it"
fi
```