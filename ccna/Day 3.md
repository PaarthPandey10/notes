# TCP/IP Model

## Protocols & Standards
* A protocol is a set of rules defining how data should be communicated b/w devices over a network.
* A standard is an agreed upon specification that describes how a protocol or technology should work. 

## History
* US Department of Defense's ARPA (Advanced Research Projects Agency) funded ARPA in 1960s to connect mainframes at universities & labs. 
* Vint Cerf & Bob Kahn (@DARPA) began TCP in 1974 (Transmission Control Program), which was later divided into:
	* Transmission Control Protocol (TCP)
	* Internet Protocol (IP)
* ARPANET switched to TCP/IP on Jan 1, 1983 and later TCP/IP became dominant.

## Who defines these standards?
1. IEEE: Institute of Electrical & Electronics Engineers
	* Ethernet (802.3)
	* Wi-Fi (802.11)
2. IETF: Internet Engineering Task Force
	* Open community defines protocols used on the internet: TCP, IP, UDP, DNS, HTTPS etc. 
	* Publishes standards in documents called RFCs (Requests for Comments)

## Layered Models
* Each layer has a specific role & uses the services of layer below & provides services to the layer above. 
* Protocols together form a stack of protocols i.e. Network Stack.

## TCP/IP Model & its working 

![[Pasted image 20260308160512.png]]

Application: Protocols for communications between application processes; create & interpret data. 
Transport: Provides end to end communication b/w application processes using port numbers. 
Internet: Provides end to end communication b/w hosts across networks using IP addresses & routers. 
Local Network: Provides hop-to-hop delivery within a local network using MAC addresses & routers.
Physical: Sends bits as electrical, optical or radio signals over the physical medium.

### Layer 1: The physical layer
* Sends & receives bits as electrical, optical or radio signals over the medium 
* Defines things like cables, connectors, signal levels and link speeds
* Ex: UTP Cables, Fiber Optic Cables, Wi-Fi radios & antennas, Network Interface Cards (NIC)

### Layer 2: The local network layer
* Provides hop-to-hop deliver of messages on a local network
* A hop is one step along the path b/w two devices:
	* From one router or host, to the next router or host in the path. 
* Switches dont count because they just extend the local network allowing multiple devices to connect. 
* Uses MAC (Media Access Control) addresses to identify interfaces
	* PC1 sends msg -> MAC of R1's G1 interface (NIC)
	* R1 sends msg -> MAC of R2's G1 interface (NIC)
	* R2 sends msg -> MAC of SRV1's interface (NIC)
* Protocols at this layer include:
	* Ethernet (802.3)
	* Wi-Fi (IEEE 802.11)

### Layer 3: The internet layer
* The internet layer provides end to end delivery b/w hosts across multiple networks. (Internet - Internetwork - between networks)
* Uses IP addresses to identify hosts in the network (PC1 addresses message to SRV1's ip address)
* Routers operate mainly at this layer, using the message's destination IP address to fwd. the message toward its final destination host. 
* Protocols:
	* IP (IPv4, IPv6)
	* ICMP (Internet Control Message Protocol)

### Layer 4: The transport layer
* Transport layer provides end to end communication between application processes - AKA process to process or service to service. 
* Uses port number to identify the processes on each host - when web client on PC1 wants to send a request to the web server running on SRV1, it addresses the message to port 80.
* Runs mainly on communicating hosts (PC1, SRV1)
* Routers normally operate based on IP (layer 3), not on transport layer information.
* Protocols: 
	* UDP (User Datagram Protocol): Simple & Efficient
	* TCP (Transmission Control Protocol): More robust features beyond basic message addressing.

### Layer 5: The application layer
* This layer is where the network communications meet applications - called layer 7 in OSI model
* Defines how application processes format, send and interpret data
* Protocols at this layer define message formats and rules for specific tasks such as:
	* Browsing web pages (HTTP/HTTPS)
	* Transferring Files (FTP/TFTP)
	* Sending/Receiving email (SMTP, POP3, IMAP)
* Network infrastructure devices (routers, switches) dont care about application layer details
	* They just move messages across the network
	* Only the communicating hosts 'interpret application data'

### Encapsulation & Decapsulation Process
![[Pasted image 20260310152522.png]]
![[Pasted image 20260310152539.png]]

As data moves down, each layer encapsulates data with a header (trailer as well in layer 2) including information needed for that layer. 
Each layer removes their own header - L2 removes L2-header and L2-trailer, L3 removes L3-header etc.

### Protocol Data Units (PDU)
![[Pasted image 20260310152621.png]]

### Adjacent Layer Interaction
![[Pasted image 20260310152655.png]]

* L4 services L5 by delivering data to correct application using port number. 
* L3 services L4 by delivering segments/datagrams to correct destination host using IP addresses.
* L2 services L3 by delivering packet to the next hop using MAC addresses.
* L1 services L2 by sending & receiving frames as electrical, optical or radio signals. 
* Layers are modular

### OSI Model
![[Pasted image 20260310152718.png]]

