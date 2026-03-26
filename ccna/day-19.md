# DTP/VTP

## Dynamic Trunking Protocol (DTP)
![](images/Pasted%20image%2020260323224423.png)
![](images/Pasted%20image%2020260323225247.png)
 
### Dynamic Desirable
![](images/Pasted%20image%2020260323225335.png)
![](images/Pasted%20image%2020260323225416.png)
![](images/Pasted%20image%2020260323225434.png)
![](images/Pasted%20image%2020260323230450.png)
![](images/Pasted%20image%2020260323230531.png)
![](images/Pasted%20image%2020260323230541.png)

### Dynamic Auto
![](images/Pasted%20image%2020260323232304.png)
![](images/Pasted%20image%2020260323232321.png)
![](images/Pasted%20image%2020260323232556.png)
![](images/Pasted%20image%2020260323232616.png)

### DTP Port Match Chart
![](images/Pasted%20image%2020260323232657.png)

DTP will not form a trunk with a router, PC, etc. The switchport will be in access mode. 
![](images/Pasted%20image%2020260323232858.png)
![](images/Pasted%20image%2020260326112849.png)
![](images/Pasted%20image%2020260326112933.png)
Negotiation of Trunking: On --> when DTP is on i.e. dynamic desirable, dynamic auto or trunk mode
Negotiation of Trunking: Off --> when in access mode or switchport no negotiate command used

## VLAN Trunking Protocol (VTP)
![](images/Pasted%20image%2020260326113213.png)
### VTP Servers/Clients
![](images/Pasted%20image%2020260326113336.png)

### Working of VTP
![](images/Pasted%20image%2020260326115520.png)![](images/Pasted%20image%2020260326115529.png)
Configuration Revision: If you add, modify or delete a VLAN - this number increases.
![](images/Pasted%20image%2020260326115719.png)
![](images/Pasted%20image%2020260326115804.png)![](images/Pasted%20image%2020260326121554.png)
![](images/Pasted%20image%2020260326121610.png)
![](images/Pasted%20image%2020260326121757.png)
![](images/Pasted%20image%2020260326121812.png)
![](images/Pasted%20image%2020260326121835.png)

### VTP Transparent
![](images/Pasted%20image%2020260326121936.png)
![](images/Pasted%20image%2020260326121955.png)
![](images/Pasted%20image%2020260326122417.png)
![](images/Pasted%20image%2020260326122447.png)
![](images/Pasted%20image%2020260326122500.png)
![](images/Pasted%20image%2020260326123046.png)
SW3 doesnt forward advertisement to SW4, to forward - change its domain name to match everything.
![](images/Pasted%20image%2020260326131338.png)

### VTP Version 
![](images/Pasted%20image%2020260326131436.png)
![](images/Pasted%20image%2020260326131447.png)



VTP also has a password feature which allows devices with matching passwords to sync vlan databases. 
