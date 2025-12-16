# Cisco Networking

## Explain Ethernet frame format 

<img width="1910" height="538" alt="image" src="https://github.com/user-attachments/assets/bcebe991-d289-4dcd-92be-54461efc4d85" />

### 1. Preamble (7 bytes)
Pattern: 10101010 repeated

Purpose: Synchronizes the sender and receiver clocks

Not counted as part of the actual frame data

### 2. Start Frame Delimiter (SFD) – 1 byte
Value: 10101011

Marks the start of the actual frame

### 3. Destination MAC Address (6 bytes)

Identifies the intended recipient

Can be:

Unicast (one device)

Multicast

Broadcast (FF:FF:FF:FF:FF:FF)

### 4. Source MAC Address (6 bytes)

MAC address of the sender

Always a unicast address

### 5. Type / Length Field (2 bytes)

This depends on the Ethernet standard used:

Ethernet II

Value ≥ 0x0600

Indicates the upper-layer protocol

0x0800 → IPv4

0x0806 → ARP

0x86DD → IPv6

IEEE 802.3

Value ≤ 1500

Indicates the length of the payload

### 6. Data / Payload (46–1500 bytes)

Contains the actual data (e.g., IP packet)

Minimum is 46 bytes

If data is smaller, padding is added

Maximum: 1500 bytes (MTU)

### 7. Frame Check Sequence (FCS) – 4 bytes

Uses CRC (Cyclic Redundancy Check)

Detects transmission errors

Frames with errors are discarded

Frame Size Summary

Minimum frame size: 64 bytes

Maximum frame size: 1518 bytes
(1522 bytes if VLAN tagging is used)


Quick Visual Example
~~~md
| 7 | 1 |   6   |   6   |   2   | 46–1500 | 4 |
| P | S | Dest  | Src   | Type  |  Data   | F |
~~~


If you want, I can also:

Compare Ethernet II vs IEEE 802.3

Explain VLAN-tagged (802.1Q) frames

Show how Ethernet fits into the OSI model

Just say the word 🙂

## easy english

Got it 🙂 I’ll keep it very simple.

Ethernet Frame (Easy English)

An Ethernet frame is a small package of data sent on a network.

It has different parts, like a letter in an envelope.

Ethernet Frame Parts

Preamble

Helps devices get ready to receive data

Start Frame Delimiter (SFD)

Says: “The frame starts now”

Destination MAC Address

Address of the device that should receive the data

Source MAC Address

Address of the device that sends the data

Type / Length

Tells what kind of data is inside (like Internet data, ARP, etc.)

Data

The real message being sent

FCS (Error Check)

Checks if the data was damaged

If there is an error, the frame is rejected

Frame Size

Minimum size: 64 bytes

Maximum size: 1518 bytes

Simple Example

Think of it like this:

To address → Destination MAC

From address → Source MAC

Message → Data

Error check → FCS

# Explain RIP routing protocol with configuration. 

# RIP (Routing Information Protocol)

## Introduction
RIP (Routing Information Protocol) is a **distance-vector routing protocol** used in computer networks to find the best path between networks. It uses **hop count** as its routing metric.

- **Maximum hop count:** 15  
- **Hop count 16:** Network is unreachable  
- **Routing updates:** Every 30 seconds  

---

## Working of RIP
- Each router sends its routing table to neighboring routers every 30 seconds.
- The best route is selected based on the **lowest hop count**.
- RIP uses the **Bellman-Ford algorithm** to calculate routes.

---

## Loop Prevention Techniques
RIP prevents routing loops using:
1. **Split Horizon** – Routes are not advertised back to the interface from which they were learned.
2. **Poison Reverse** – A failed route is advertised with hop count 16.
3. **Hold-down Timer** – Prevents routers from accepting incorrect updates for a certain time.

---

## Versions of RIP
### RIP v1
- Classful routing protocol
- Does not support VLSM

### RIP v2
- Classless routing protocol
- Supports **VLSM and CIDR**
- Uses multicast address **224.0.0.9**

---

## Example Network

~~~
PC1 --- R1 --- R2 --- R3 --- PC2
~~~


- Network at R1: 192.168.1.0  
- Network at R2: 10.0.0.0  
- Network at R3: 172.16.0.0  

**Path chosen by RIP:**  
PC1 → R1 → R2 → R3 → PC2  
**Hop count = 2**, so this path is selected.

---

## RIP Configuration Example (Cisco)

### Router R1
```plaintext
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network 192.168.1.0
```
```plaintext
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network 10.0.0.0
```
```plaintext
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network 172.16.0.0
```
Explanation of Commands

router rip – Enables RIP routing

version 2 – Enables RIP version 2 (classless)

network – Advertises connected networks

Advantages of RIP

Simple and easy to configure

Widely supported

Suitable for small networks

Limitations of RIP

Maximum hop count of 15

Slow convergence

Not suitable for large networks


