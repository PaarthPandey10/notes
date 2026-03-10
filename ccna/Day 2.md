# Interfaces & Cables 

* Ethernet:
	* A Collection of network protocols/standards. 
	* Ethernet is not just one thing- for the purpose of this lesson we will focus on types of cabling as set by ethernet standards. 
* Protocols:
	* Agreed upon system and rules both physical & logical.
* Bits and Bytes:
	* Bit: 0 or 1
	* Byte: 8 bits
	* Speed is measure in bits per second, and data is sent bit by bit.
* Ethernet Standards are defined in IEEE 802.3 where IEEE stands for Institute of Electrical and Electronics Engineers.
* BASE-T Explained:
	* BASE: Stands for *Baseband Signaling*
	* T: Stands for *Twisted Pair Cabling*
	* Max length of 100m due to performance and technical reasons. 

## Ethernet Standards for Copper
![Image](ccna/images/Pasted%20image%20 20260308151822.png)

## UTP (Copper) Cables
Unshielded Twister Pair

* Twist helps protect against electromagnetic interference.
* 4 Types:
	1. 10BASE-T (2 pairs, 4 wires)
	2. 100BASE-T (2 pairs, 4 wires)
	3. 1000BASE-T (4 pairs, 8 wires)
	4. 10GBASE-T (4 pairs, 8 wires)
### Working of 10BASE-T/100BASE-T
![Image](ccna/images/Pasted%20image%20 20260308152639.png)

#### Straight Through Cable
Copper ethernet cable has two RJ-45 connectors on each end. 
#### Full Duplex Transmission
Both devices can send and receive data at the same time - NO COLLISSIONS cuz they use separate wires to transmit and receive data. 

PC & Router connect the same (straight through cable design) -  1 & 2 for TX & 3 & 6 for RX.
Switch works the opposite - 1 & 2 for RX & 3 & 6 for TX.

Because of this conflict, straight through cable cant connect router to router or pc to pc or switch to switch or pc to router or router to pc.

#### Crossover Cable
![Image](ccna/images/Pasted%20image%20 20260308153600.png)

For UTP Cables (10BASE-T, 100BASE-T)
![Image](ccna/images/Pasted%20image%20 20260308153654.png)

Modern day solution: Auto MDI-X
#### Auto MDI-X (Automatic Medium Dependent Interface Crossover)
Feature that auto detects which type of device is communicating using which pins and configures/adjusts ports accordingly - No straight through/crossover hassle. 

### Working of 1000BASE-T & 10GBASE-T
![Image](ccna/images/Pasted%20image%20 20260308154352.png)
Each pair is bidirectional allowing for faster speeds. 

## Fiber Optic Cables
* Connect using *SFP* Transceiver to switches & routers. 
* SFP: Small Form-Factor Pluggable.
* Fiber Optic Cables connect using two ends: one for TX, other for RX.
#### Inside a fiber optic cable:
![Image](ccna/images/Pasted%20image%20 20260308154616.png)

### Multimode fiber cables
![Image](ccna/images/Pasted%20image%20 20260308154737.png)
* Core diameter is wider than single mode
* Allows multiple angles of light waves to enter fiber-glass core
* Longer cables than UTP, shorter than single-mode
* Cheaper than single mode cuz they use LED based SFP Transceivers.

### Single mode fiber cables
![Image](ccna/images/Pasted%20image%20 20260308154847.png)
* Core diameter narrower than multimode fiber.
* Light enters at a single angle (mode) from a laser based SFP.
* Allows longer cables than both UTP & multimode fiber
* More expensive than multimode fiber (due to more expensive lased based SFP)

### Fiber optic cable standards
![Image](ccna/images/Pasted%20image%20 20260308155026.png)

### UTP v/s Fiber Optic Cabling
![Image](ccna/images/Pasted%20image%20 20260308155152.png)
