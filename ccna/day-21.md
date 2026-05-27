# Spanning Tree Protocol II

![](images/Pasted%20image%2020260527131149.png)

## Blocking State
![](images/Pasted%20image%2020260527131353.png)

## Listening State 
![](images/Pasted%20image%2020260527131529.png)

## Learning State
![](images/Pasted%20image%2020260527131631.png)

## Forwarding State
![](images/Pasted%20image%2020260527131657.png)

## STP Port States Overview
![](images/Pasted%20image%2020260527131727.png)

## STP Timers
![](images/Pasted%20image%2020260527132108.png)

### STP Hello Timer
![](images/Pasted%20image%2020260527132144.png)
Every 2 seconds the root bridge sends BPDUs after the network has stabilized.
The other switches forward these BPDUs through their designated ports only. 
![](images/Pasted%20image%2020260527132245.png)

### Forward Delay Timer
The time the port takes to transition from blocking to forwarding state - it includes a 15 second listening time, and a 15 second learning time. 


### Max Age Timer
![](images/Pasted%20image%2020260527132520.png)
Focus on SW2 G0/1 interface
![](images/Pasted%20image%2020260527132543.png)
![](images/Pasted%20image%2020260527132557.png)

## Spanning Tree BPDU
![](images/Pasted%20image%2020260527132848.png)
![](images/Pasted%20image%2020260527133045.png)
If Root Identifier = Bridge Identifier => This is the root bridge. 
Message Age is the number of hops. 

## STP Features (STP Toolkit)

### Portfast
![](images/Pasted%20image%2020260527133330.png)
![](images/Pasted%20image%2020260527133344.png)
![](images/Pasted%20image%2020260527133958.png)

Portfast only works on access ports, not trunks because trunks are usually connected to switches. 

