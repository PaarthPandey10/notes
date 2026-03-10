# Introduction to Computer Networking Devices
A computer network is a digital telecommunications network that allows nodes to share resources. 

* Client: 
	* Device that access service made available by a server.
* Server: 
	* Device that provides functions or services for a client. 
	* Same device can be a client & server in other situations. 
* Switches: 
	* Has many network interfaces/ports for end hosts to connect to (usually 24+). 
	* Provides connectivity to end hosts within same LAN.
	* Eg: Catalyst 9200, Catalyst 3650
	* Don't provide connectivity b/w LANs or over the internet.
* Routers:
	* Have fewer network interfaces than switches.
	* Used to provide connectivity b/w LANs, therefore used to send data over the internet.
	* Eg: ISR 1000 (has ports on backside), ISR 900 (has ports on frontside), ISR 4000 (has ports on backside).
* Firewalls:
	* Monitor and control network traffic based on configured rules.
	* Can be placed inside the network or outside the network.
	* "*Next Generation Firewalls*" when they include more modern and advanced filtering capabilities. 
	* *Network Firewalls*: hardware devices that filter traffic between networks.
	* *Host-based firewalls*: Software applications that filter traffic entering and exiting a host machine like a PC.
	* Eg: ASA 5500-X (Adaptive Security Appliance), Firepower 2100.
	* ASA is a more classic version, modern day ASA has IPS (Intrusion Prevention Systems).
	* Firepower is a modern day ASA, aka Next Generation Firewall.
