# VLAN III
## Native VLAN on a router

### Configuring native VLAN on a router
![](images/Pasted%20image%2020260322231002.png)

### encapsulation command
![](images/Pasted%20image%2020260322231023.png)

#### Wireshark Capture (SW2 --> R1)
![](images/Pasted%20image%2020260322231528.png)
![697](images/Pasted%20image%2020260322231421.png)
#### Wireshark Capture (SW2 --> R1)
![](images/Pasted%20image%2020260322231453.png)
![](images/Pasted%20image%2020260322231637.png)
Not tagged with do1q because of native vlan usage.

![](images/Pasted%20image%2020260323001314.png)
The ICMP echo request will continue to its destination, untagged all the way, same for the ICMP echo reply. 

### Physical interface native VLAN configuration
![](images/Pasted%20image%2020260323001521.png)
![](images/Pasted%20image%2020260323001547.png)
![](images/Pasted%20image%2020260323002108.png)

## Layer 3 (Multilayer) Switches
![](images/Pasted%20image%2020260323002441.png)
