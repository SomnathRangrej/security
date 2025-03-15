- What is difference between subnet & VLAN? explain with examples


### Difference Between **Subnet** and **VLAN**

| Feature        | Subnet (Subnetting) | VLAN (Virtual LAN) |
|---------------|------------------|----------------|
| **Definition** | Logical division of an IP network into smaller networks. | Logical segmentation of a physical network at the data link layer (Layer 2). |
| **Layer** | Works at **Layer 3** (Network Layer) of the OSI model. | Works at **Layer 2** (Data Link Layer) of the OSI model. |
| **Function** | Separates a large network into smaller IP networks to optimize routing and improve security. | Segments a single physical switch into multiple logical networks to control broadcast domains. |
| **Communication** | Devices in different subnets require a **router** to communicate. | Devices in different VLANs require a **Layer 3 switch or router** to communicate. |
| **Broadcast Domain** | Each subnet forms a different broadcast domain by default. | Each VLAN is a separate broadcast domain, even on the same switch. |
| **IP Addressing** | Uses **IP addresses** to identify devices within a subnet. | Uses **VLAN IDs** to logically separate devices on a switch. |
| **Usage** | Used for **networking efficiency** and **security** by breaking down large networks into manageable sub-networks. | Used to **isolate traffic**, enhance security, and optimize network performance in a LAN. |

---

## **Example of Subnetting**
Imagine a company with the IP range **192.168.1.0/24**. Instead of using one big network, the company splits it into **two subnets**:

1. **Subnet 1:** 192.168.1.0/25 (Hosts: 192.168.1.1 - 192.168.1.127) → Used for Employees  
2. **Subnet 2:** 192.168.1.128/25 (Hosts: 192.168.1.129 - 192.168.1.254) → Used for Guests  

Since subnets work at **Layer 3**, devices from different subnets need a **router** to communicate.

---

## **Example of VLAN**
A company has a single **Layer 2 switch** but wants to separate different departments without using multiple physical switches:

1. **VLAN 10 - HR Department**
   - Devices: HR Computers, Printers
   - VLAN ID: 10  

2. **VLAN 20 - IT Department**
   - Devices: IT Support, Servers
   - VLAN ID: 20  

Although both VLANs exist on the same **physical switch**, they cannot communicate unless a **Layer 3 switch or router** is used.

---

### **Key Takeaways**
- **Subnetting is for IP segmentation (Layer 3), VLAN is for logical segmentation (Layer 2).**
- **A VLAN can contain multiple subnets, and a subnet can span multiple VLANs.**
- **Subnet requires a router for communication, VLAN requires a Layer 3 device for inter-VLAN routing.**

Would you like a practical network configuration example? 🚀

### **Practical Network Configuration Example: Subnet & VLAN**  

Let’s assume a company has a **single Layer 3 switch** and wants to:  
1. **Separate HR and IT departments using VLANs**  
2. **Assign different subnets to each VLAN**  
3. **Enable communication between VLANs using a Layer 3 switch**  

---

## **Step 1: Network Design**
| Department | VLAN ID | Subnet |
|------------|---------|----------------|
| HR | 10 | 192.168.10.0/24 |
| IT | 20 | 192.168.20.0/24 |

---

## **Step 2: Configure VLANs on a Layer 3 Switch**
### **On Cisco Switch**
1. **Create VLANs**
   ```sh
   configure terminal
   vlan 10
   name HR
   exit
   vlan 20
   name IT
   exit
   ```

2. **Assign Ports to VLANs**
   - Suppose HR computers are on ports **FastEthernet0/1 - 0/10**  
   - IT computers are on ports **FastEthernet0/11 - 0/20**  
   
   ```sh
   interface range FastEthernet0/1 - 10
   switchport mode access
   switchport access vlan 10
   exit

   interface range FastEthernet0/11 - 20
   switchport mode access
   switchport access vlan 20
   exit
   ```

---

## **Step 3: Assign Subnets & Enable Inter-VLAN Routing**
Since a Layer 3 switch is used, we need to create **SVIs (Switched Virtual Interfaces)** to enable communication between VLANs.

```sh
interface Vlan10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface Vlan20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
```
- **Each VLAN now has a virtual gateway IP (192.168.10.1 & 192.168.20.1).**  
- **HR computers will use `192.168.10.1` as their gateway.**  
- **IT computers will use `192.168.20.1` as their gateway.**

---

## **Step 4: Enable Routing Between VLANs**
By default, VLANs **cannot communicate** with each other. Since we are using a **Layer 3 switch**, enable **IP routing**:

```sh
ip routing
```

Now, devices in **HR (192.168.10.0/24)** can talk to devices in **IT (192.168.20.0/24)**.

---

## **Verification**
1. **Check VLAN Configuration**  
   ```sh
   show vlan brief
   ```
2. **Check IP Routing**  
   ```sh
   show ip route
   ```
3. **Ping Between VLANs**  
   - From a computer in HR (192.168.10.100)  
     ```sh
     ping 192.168.20.100
     ```
   If successful, **Inter-VLAN routing is working!** ✅

---

### **Summary**
✔ **VLAN 10 & VLAN 20 separate departments logically**  
✔ **Each VLAN has a unique subnet**  
✔ **Layer 3 switch routes traffic between VLANs**  

Would you like an advanced setup with DHCP or security policies?
