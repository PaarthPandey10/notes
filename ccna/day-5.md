# Ethernet LAN Switching

## Ethernet Frame
![](images/Pasted%20image%2020260310163335.png)

### Preamble
![](images/Pasted%20image%2020260310163104.png)

### SFD
![](images/Pasted%20image%2020260310163117.png)

### Destination & Source
![](images/Pasted%20image%2020260310163146.png)

### Type / Length
![](images/Pasted%20image%2020260310163225.png)

### FCS
![](images/Pasted%20image%2020260310163424.png)

## MAC Address
![](images/Pasted%20image%2020260310163524.png)

### MAC Address Example 1
![](images/Pasted%20image%2020260310163655.png)
Two Cases (Unicast Frame, i.e. aimed at a single target)
1. Unknown: Unknown because switch doesnt have an entry in its MAC Address Table (MAT). FLOOD.
2. Known: Switch has entry in its MAC Address Table. FORWARD.

#### Dynamic MAC Address (Dynamically Learned)
When SW receives packet from PC1, at F0/1, it stores this info. in its MAT for upto 5 minutes.
When SW is forwarding an UUF, it FLOODS its other interfaces, i.e. the ones except F0/1 (cuz thats alr full - thats the source's interface)
When SW is forwarding a KUF, it FORWARDS to the interface from its MAT.

