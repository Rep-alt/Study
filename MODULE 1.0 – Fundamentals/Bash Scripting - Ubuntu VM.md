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
#!/bin/bash

## This is to choose what type of scan is needed.
echo "**Choose scan type**"

echo -e "\n1.)Host Discovery (Ping Sweep)"
echo -e "\n2.)Service & Version Detection"
echo -e "\n3.)OS Detection & Aggressive Scan"
echo -e "\n4.)Specific Port or Range Scan"
echo -e "\n5.)Stealth SYN Scan"
echo -e "\n6.)Using Nmap Scripting Engine"
echo -e "\nAcceptable Input # 1-6"
read Scan_type

if [ $Scan_type = 1 ];then
        echo "Scan type:Host Discovery"
elif [ $Scan_type = 2 ];then
        echo "Scan type:Service & Version Detection"
elif [ $Scan_type = 3 ];then
        echo "Scan type:OS Detection & Aggressive Scan"
elif [ $Scan_type = 4 ];then
        echo "Scan type:Specific Port or Range Scan"
elif [ $Scan_type = 5 ];then
        echo "Scan type:Stealth SYN Scan"
elif [ $Scan_type = 6 ];then
        echo "Scan type:Using Nmap Scripting Engine"
else
        echo "Try again"
        read Scan_type
fi
## This is to put an IP or IP range into a variable to call later in the script
echo "*Insert <Target> IP/IP range**"
read Target

## This is to put a port or list of ports into a variable to scan
echo -e "[[Insert Ports to scan separated by spaces]]\n'For all ports insert '1-65535'' "
read -a Port_array
Ports_joined=$(IFS=, ; echo "${Port_array[*]}")
echo $Ports_joined

##Scan being conducted
if [ $Scan_type = 1 ];then
        nmap "$Ports_joined" -sn "$Target"
elif [ $Scan_type = 2 ];then
        nmap "$Ports_joined" -sV "$Target"
elif [ $Scan_type = 3 ];then
        nmap "$Ports_joined" -A "$Target"
elif [ $Scan_type = 4 ];then
        nmap "$Ports_joined" -sU "$Target"
elif [ $Scan_type = 5 ];then
        nmap "$Ports_joined" -sS "$Target"
elif [ $Scan_type = 6 ];then
        nmap "$Ports_joined" -sC "$Target"
elif [ $Scan_type = 7 ];then
        nmap "$Ports_joined" -sn "$Target"

else
        echo "Try again"
        read Scan_type
fi

