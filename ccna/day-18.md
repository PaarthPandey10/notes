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
![](images/Pasted%20image%2020260323085130.png)
### Switch Virtual Interfaces (SVI)
![](images/Pasted%20image%2020260323085203.png)
![](images/Pasted%20image%2020260323085257.png)

For traffic destined outside the LAN, configure a default route from SW2 to R1 (SW2 is basically a router atp).
![](images/Pasted%20image%2020260323085812.png)

### Configurations
#### R1 config: set interface to default
![](images/Pasted%20image%2020260323085908.png)
![](images/Pasted%20image%2020260323085918.png)
#### SW2 routed port config
![](images/Pasted%20image%2020260323085928.png)
*ip routing: enables layer 3 routing on the switch.*
*no switchport: configures the interface as a router port i.e. layer 3 port, not a layer 2 (switch) port*
#### SW2 default route config
![](images/Pasted%20image%2020260323090131.png)
![](images/Pasted%20image%2020260323090146.png)
#### SW2 SVIs config
![](images/Pasted%20image%2020260323090228.png)
### SVI pre-requisites
![](images/Pasted%20image%2020260323090458.png)

------------------

![](images/Pasted%20image%2020260323090533.png)