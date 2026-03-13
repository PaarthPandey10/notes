# Ethernet LAN Switching - II

## Ethernet Frame
![](images/Pasted%20image%2020260310164302.png)
![](images/Pasted%20image%2020260310164318.png)


![](images/Pasted%20image%2020260310164356.png)

## ARP
![](images/Pasted%20image%2020260310164423.png)

### ARP Request
![](images/Pasted%20image%2020260310192500.png)

### ARP Reply
![](images/Pasted%20image%2020260310192518.png)

#### Explanation
Don't know destination MAC address -> Send ARP request.
ARP request sent to all other hosts in the network.
PC3 responds to the ARP request since it's IP address matches the destination IP address in the ARP request -> ARP reply.
PC1 learn MAC address of PC3, adds it to the ARP table & uses that information to the destination of any ping (or frame) it wants to send.
Broadcast UUF have a destination MA of FF.FF.FF.FF.FF.FF.

### ARP Table
![](images/Pasted%20image%2020260310193011.png)

## Ping
![](images/Pasted%20image%2020260312214830.png)
![](images/Pasted%20image%2020260312214845.png)
![](images/Pasted%20image%2020260312214920.png)

## Clearing the entire MAC Address Table
![](images/Pasted%20image%2020260313225030.png)
## Clearing a single entry

### Using MAC Address
![](images/Pasted%20image%2020260313225050.png)

### Using interface number
![](images/Pasted%20image%2020260313225154.png)

## Ethernet Frame in wireshark

### ICMP
![](images/Pasted%20image%2020260313225243.png)

### ARP
![](images/Pasted%20image%2020260313225313.png)