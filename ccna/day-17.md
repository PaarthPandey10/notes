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
