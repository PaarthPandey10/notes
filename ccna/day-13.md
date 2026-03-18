# Subnetting I 

# Fixed Length Subnet Masks (FLSM)
## IPv4 Address Classes
![](images/Pasted%20image%2020260317222633.png)

Class D & E are used for special purposes.

![](images/Pasted%20image%2020260317222721.png)

## IANA (Internet Assigned Numbers Authority)
![](images/Pasted%20image%2020260317222830.png)
### Wasted IP addresses
![](images/Pasted%20image%2020260317223148.png)
![](images/Pasted%20image%2020260317223209.png)
![](images/Pasted%20image%2020260317223244.png)

## CIDR (Classless Inter-Domain Routing)
![](images/Pasted%20image%2020260317223401.png)
![](images/Pasted%20image%2020260317223414.png)
![](images/Pasted%20image%2020260317223628.png)

### Number of Usable Addresses
2<sup>n</sup> - 2
where n = number of host bits ( = 32 - number network bits (network bits is equal to 'a' in /a, where /a is the CIDR notation of a network's subnet mask))

### CIDR Notation
![](images/Pasted%20image%2020260317224439.png)

#### /31 Subnet Mask
Upon calculating, you'll notice that only 2 addresses are left in this subnet mask and those 2 are reserved for the broadcast (all host bits 1) and network address (all host bits 0). This subnet mask can not be used for usual modern day networks, but it can however be used for a point-to-point network.
![](images/Pasted%20image%2020260318002301.png)

## Calculating the required subnet mask
Let 'n' be the number of hosts required to accommodate, therefore the subnet mask should have 'N' i.e. number of usable addresses, greater than or equal to n, i.e., N >= n. 
