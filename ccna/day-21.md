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

*DETAILED EXPLANATION*:
#### Portfast: The Problem
![](images/Pasted%20image%2020260528150208.png)

![](images/Pasted%20image%2020260528150232.png)
STP Listening + Learning State going on
![](images/Pasted%20image%2020260528150311.png)
Interface ready to use after 30 seconds

#### Portfast: The Solution
![](images/Pasted%20image%2020260528150356.png)

![](images/Pasted%20image%2020260528150429.png)
Interface ready to use instantly

#### Portfast Configuration: Per Port/Interface
![](images/Pasted%20image%2020260528150559.png)

#### Portfast Configuration: Default
![](images/Pasted%20image%2020260528154737.png)

#### Portfast on Trunk Ports
![](images/Pasted%20image%2020260528154924.png)
#### Portfast Edge
![](images/Pasted%20image%2020260528155848.png)

### BPDU Guard
![](images/Pasted%20image%2020260528143759.png)
In cases like this, BPDU Guard comes in handy.
![](images/Pasted%20image%2020260528143823.png)
![](images/Pasted%20image%2020260528143832.png)
![](images/Pasted%20image%2020260528143844.png)
![](images/Pasted%20image%2020260528143852.png)
![](images/Pasted%20image%2020260528143908.png)

*DETAILED EXPLANATION*:
#### BPDUs vs Portfast
![](images/Pasted%20image%2020260528160408.png)
![](images/Pasted%20image%2020260528160431.png)
#### BPDU Guard: The Problem
![](images/Pasted%20image%2020260528160546.png)
![](images/Pasted%20image%2020260528160603.png)
![](images/Pasted%20image%2020260528160637.png)
![](images/Pasted%20image%2020260528161033.png)
#### BPDU Guard Configuration
![](images/Pasted%20image%2020260528161204.png)

When enabled by default: BPDU Guard is activated on all Portfast-enabled ports - THIS MEANS NOT NECESSARILY ALL ACCESS PORTS - ALL PORTFAST PORTS MEANING ANY PORT - EITHER ACCESS OR TRUNK - DEPENDS ON PORTFAST PORT. 
#### ErrDisable
![](images/Pasted%20image%2020260528162442.png)

##### ErrDisable on a real switch
![](images/Pasted%20image%2020260528162611.png)
Port 1 is ErrDisabled because of a BPDU Guard Violation.
![](images/Pasted%20image%2020260528162646.png)
Port 1 remains ErrDisabled even after removing the cable. Removing cable does not fix an ErrDisabled port. 

##### Fixing ErrDisable (Re-enabling)
![](images/Pasted%20image%2020260528162826.png)
![](images/Pasted%20image%2020260528230603.png)
### BPDU Filter 
#### BPDU Filter: The Problem
![](images/Pasted%20image%2020260528232300.png)
#### BPDU Filter: The Solution
![](images/Pasted%20image%2020260528232439.png)
![](images/Pasted%20image%2020260528232547.png)

### Root Guard
#### Root Bridge Placement
![](images/Pasted%20image%2020260530200503.png)
![](images/Pasted%20image%2020260530200552.png)
#### Root Guard: The Problem
![](images/Pasted%20image%2020260530202254.png)
![](images/Pasted%20image%2020260530202343.png)

#### Root Guard: The Solution
![](images/Pasted%20image%2020260530202513.png)
![](images/Pasted%20image%2020260530202542.png)

#### Root Guard: CLI Demonstration
![](images/Pasted%20image%2020260530202619.png)
![](images/Pasted%20image%2020260530202749.png)


### Loop Guard
#### Unidirectional Links
![](images/Pasted%20image%2020260530204414.png)
![](images/Pasted%20image%2020260530204435.png)
![](images/Pasted%20image%2020260530204443.png)
![](images/Pasted%20image%2020260530204503.png)
![](images/Pasted%20image%2020260530204513.png)
![](images/Pasted%20image%2020260530204525.png)
![](images/Pasted%20image%2020260530204707.png)

#### Loop Guard: The Problem
![](images/Pasted%20image%2020260530204747.png)
![](images/Pasted%20image%2020260530204758.png)

### Loop Guard: The Solution
![](images/Pasted%20image%2020260530204832.png)

#### Loop Guard: CLI Demonstration
![](images/Pasted%20image%2020260530204857.png)
![](images/Pasted%20image%2020260530204909.png)
![](images/Pasted%20image%2020260530204924.png)
![](images/Pasted%20image%2020260530204942.png)

#### Loop Guard vs Root Guard
![](images/Pasted%20image%2020260530205010.png)

## Configuring the Spanning Tree Mode
![](images/Pasted%20image%2020260528144008.png)

## Configuring the Root Bridge
### Primary Bridge
![](images/Pasted%20image%2020260528144053.png)
SW3 = Primary
SW2 = Secondary
![](images/Pasted%20image%2020260528144157.png)
Running Config's output:
![](images/Pasted%20image%2020260528144213.png)
### Secondary Bridge
![](images/Pasted%20image%2020260528144245.png)
![](images/Pasted%20image%2020260528144322.png)

## STP Load-Balancing
![](images/Pasted%20image%2020260528144430.png)
![](images/Pasted%20image%2020260528144438.png)
Blocking the same interfaces in both VLAN 1 and 2 = Interface Bandwidth wasting. 

Since the primary and secondary changes applied above were in VLAN 1, it shows a different topology. 

### Load Balancing Example
![](images/Pasted%20image%2020260528144655.png)
![](images/Pasted%20image%2020260528144710.png)
![](images/Pasted%20image%2020260528144727.png)
![](images/Pasted%20image%2020260528144747.png)
![](images/Pasted%20image%2020260528144802.png)

## Configuring STP Port Settings
![](images/Pasted%20image%2020260528144944.png)
