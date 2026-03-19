# VLAN I
Virtual Local Area Networks

## What is a LAN?
![](images/Pasted%20image%2020260319105601.png)
### Broadcast Domain
![](images/Pasted%20image%2020260319105718.png)

## The need for VLAN
### Current Network Config
![](images/Pasted%20image%2020260319110300.png)
![](images/Pasted%20image%2020260319110311.png)
A broadcast frame in the current config will flood the entire network causing network performance issues, and security issues since anyone in any department can access anything. 
### Better Network Config
![](images/Pasted%20image%2020260319110345.png)
![](images/Pasted%20image%2020260319110355.png)
![](images/Pasted%20image%2020260319110629.png)
In this config, we are forcing the traffic to pass through routers in case of KUFs (known unicast frames), this helps with improved security as we can configure security policies and other layer 3 level rules in a router. But it still has the issue of poor network performance due to broadcast floods. Although we separated the three departments into three subnets (Layer 3), they are still in the same broadcast domain (Layer 2). 

### Best Network Config (VLANs)
![](images/Pasted%20image%2020260319110822.png)
![](images/Pasted%20image%2020260319110832.png)
You can configure VLANs on a switch, more specifically on the switch interface. 
A switch will NOT forward traffic between VLANs, including broadcast/unknown unicast traffics. 
Traffic that arrives on a VLAN 10 interface, will be forwarded out of a VLAN 10 interface. 
Switch does perform inter-VLAN routing (router does), it must send the traffic through the router. Even if two devices are in the same subnet, but different VLANs - the switch will NOT route traffic between these devices. 

## What is a VLAN?
![](images/Pasted%20image%2020260319111143.png)

## VLAN Configuration
![](images/Pasted%20image%2020260319111211.png)
![](images/Pasted%20image%2020260319111252.png)

By default, all interfaces (ports) are in VLAN 1.
### Access Ports & Trunk Ports
![](images/Pasted%20image%2020260319111414.png)

Because VLAN 10 didnt exist, it was created automatically (same for VLAN 20 and 30). 

![](images/Pasted%20image%2020260319111642.png)
![](images/Pasted%20image%2020260319111651.png)
![](images/Pasted%20image%2020260319111735.png)
![](images/Pasted%20image%2020260319111758.png)

