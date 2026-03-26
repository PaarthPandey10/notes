# VLAN II

## Why do we need trunk ports?

### Current Topology
![](images/Pasted%20image%2020260319143202.png)
![](images/Pasted%20image%2020260319143218.png)
![](images/Pasted%20image%2020260319143248.png)

### Trunk Port Topology
![](images/Pasted%20image%2020260319143323.png)
![](images/Pasted%20image%2020260319143424.png)

## Trunking (Tagging) Protocols
![](images/Pasted%20image%2020260319143450.png)

### 802.1Q Tag in Ethernet Frame
![](images/Pasted%20image%2020260319143542.png)

### 802.1Q Tag Format
![](images/Pasted%20image%2020260319143613.png)

#### TPID (Tag Protocol Identifier)
![](images/Pasted%20image%2020260319143745.png)

#### PCP (Priority Code Point)
![](images/Pasted%20image%2020260319143806.png)

#### DEI (Drop Eligible Indicator)
![](images/Pasted%20image%2020260319143831.png)


#### VID (VLAN ID)
![](images/Pasted%20image%2020260319143901.png)

#### TCI (Tag Control Information)
The TCI (Tag Control Information) is a 16-bit (2-byte) field within the 32-bit IEEE 802.1Q VLAN tag, placed inside Ethernet frames to identify VLAN membership and priority. It includes a 3-bit Priority (PCP), 1-bit Drop Eligible Indicator (DEI), and a 12-bit VLAN Identifier (VID), allowing for up to 4094 active VLANs.

## VLAN Ranges
![](images/Pasted%20image%2020260319144437.png)

## Working of Trunk Port
![](images/Pasted%20image%2020260319144516.png)

## Native VLAN
![](images/Pasted%20image%2020260319144544.png)

### Native VLAN Mismatch
![](images/Pasted%20image%2020260319144614.png)

## Trunk Configuration
![](images/Pasted%20image%2020260319144720.png)

### Auto encapsulation error in legacy switches

![](images/Pasted%20image%2020260319151548.png)

### Show trunk interfaces

![](images/Pasted%20image%2020260319151915.png)

In the first image, the first row shows the summary of listen port (only g0/0 in this case) - it also shows the native vlan. 
The second row shows the vlans allowed on listed trunk port.
The third row shows the vlans allowed on listed trunk port in current device / switch. 
Fourth row not covered till now. 

VLANs 1002, 1003, 1004, 1005 aren't shown because they're old technologies.

### Trunk Config Commands
![](images/Pasted%20image%2020260319152250.png)
#### WORD 
The WORD in this case is "10,30"
![](images/Pasted%20image%2020260319152225.png)
#### add
![](images/Pasted%20image%2020260319152325.png)
#### remove
![](images/Pasted%20image%2020260319153115.png)
#### all
![](images/Pasted%20image%2020260319153206.png)
#### except
![](images/Pasted%20image%2020260319153234.png)
#### none
![](images/Pasted%20image%2020260319153257.png)

### Setting the native VLAN
![](images/Pasted%20image%2020260319153415.png)
![](images/Pasted%20image%2020260319153431.png)


![](images/Pasted%20image%2020260319153505.png)


## Router On A Stick (ROAS)
Whenever there is just a single physical interface connecting the router and the switch.
![](images/Pasted%20image%2020260319153857.png)
### Sub-interfaces
![](images/Pasted%20image%2020260319153700.png)

#### Configuring Sub-Interfaces
![](images/Pasted%20image%2020260319153747.png)
![](images/Pasted%20image%2020260319153803.png)

![](images/Pasted%20image%2020260319153822.png)
![](images/Pasted%20image%2020260319153838.png)

### Final Topology
![](images/Pasted%20image%2020260319153925.png)