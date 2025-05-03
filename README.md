
 # Cisco Switch & Router Hardening, VLAN Segmentation & DHCP Inter-VLAN Routing Lab

---

## Project 

**Switch & Router Security, VLAN Management and DHCP Inter-VLAN Routing (Packet Tracer)**

---

## Objective

Designed and simulated a secure enterprise network in Cisco Packet Tracer, implementing VLAN segmentation, router-on-a-stick inter-VLAN routing, and centralized DHCP. Strengthened security with SSH, port security, and access controls—practicing real-world network administration in a virtual lab.

---

## Skills Learned

- Cisco IOS CLI configuration
- Device hardening techniques
- VLAN creation and interface assignment
- Inter-VLAN routing via subinterfaces
- DHCP setup and relay forwarding
- Security banners, password encryption, and Telnet access

---

## Tools Used

| Tool                | Purpose                                |
|---------------------|----------------------------------------|
| Cisco Packet Tracer | Network simulation                     |
| Cisco Switch        | VLAN creation, trunking, hardening     |
| Cisco Router        | Inter-VLAN routing, hardening          |
| DHCP Server         | Dynamic IP address management          |
| End Devices (PCs)   | Test DHCP and routing                  |

---
## Topology Overview
**Ref:**
Topology
![LanConf](https://github.com/user-attachments/assets/537311d0-6389-41bb-85ef-b10e7aa00cbe)


## Lab Sections

1. **Switch Hardening**
2. **Router Hardening**
3. **VLAN Setup**
4. **Router-on-a-Stick Inter-VLAN Routing**
5. **DHCP Server Configuration**
6. **DHCP Relay Configuration**
7. **Testing & Verification**

---

## 1. Switch Hardening (SWITCH01)

#### Step 1: Enter Privileged Mode
```bash
Switch> enable
```

#### Step 2: Configure Hostname

```bash
Switch# configure terminal
Switch(config)# hostname SWITCH01
```

#### Step 3: Configure MOTD Banner

```bash
SWITCH01(config)# banner motd #**
==========================================
Warning!! Authorized Users Only**
==========================================#
```
#### Step 4: Set Console Password
```bash
SWITCH01(config)# line con 0
SWITCH01(config-line)# password Paramaribo
SWITCH01(config-line)# login
```

#### Step 5: Set VTY Line Password (Telnet)
```bash
SWITCH01(config)# line vty 0 15
SWITCH01(config-line)# password Wanica
SWITCH01(config-line)# login
```
#### Step 6: encrypt passwords
```bash
SWITCH01(config)# service password-encryption
```

#### Step 7: Secure Enable Mode
```bash
SWITCH01(config)# enable secret Suriname
```

#### Step 8: Configure Management Interface
```bash
SWITCH01(config)# interface vlan 1
SWITCH01(config-if)# ip address 192.168.10.10 255.255.255.0
SWITCH01(config-if)# no shutdown
```
---

## 2. Router Hardening (ROUTER1)
 
#### Step1: Set host name
```
Router> enable
Router# configure terminal
Router(config)# hostname ROUTER1
```

#### Step 2: Set Enable Secret
```bash
ROUTER1(config)# enable secret Paramaribo
```

#### Step 3: Encrypt Passwords
```bash
ROUTER1(config)# service password-encryption
```

#### Step 4: Configure MOTD Banner
```bash
ROUTER1(config)# banner motd #
============================
Unauthorized Access is Prohibited!
Verboden Toegang!
============================
```

#### Step 5: Secure Console Access
```bash
ROUTER1(config)# line con 0
ROUTER1(config-line)# password Paramaribo
ROUTER1(config-line)# login
```

#### Step 6: Secure VTY Access (Telnet)
```bash
ROUTER1(config)# line vty 0 4
ROUTER1(config-line)# password Wanica
ROUTER1(config-line)# login
```
---

## 3. VLAN Configuration (SWITCH01)
### VLAN Creation & Assignment

#### Create VLANs
```bash
SWITCH01(config)# vlan 10
SWITCH01(config-vlan)# name Sales
SWITCH01(config)# vlan 20
SWITCH01(config-vlan)# name IT
SWITCH01(config)# vlan 30
SWITCH01(config-vlan)# name HR
```

#### Assign Ports
```bash
SWITCH01(config)# interface range fa0/2 - 5
SWITCH01(config-if-range)# switchport mode access
SWITCH01(config-if-range)# switchport access vlan 10

SWITCH01(config)# interface range fa0/6 - 10
SWITCH01(config-if-range)# switchport mode access
SWITCH01(config-if-range)# switchport access vlan 20

SWITCH01(config)# interface range fa0/11 - 15
SWITCH01(config-if-range)# switchport mode access
SWITCH01(config-if-range)# switchport access vlan 30
```
**Ref:**
![vlanass](https://github.com/user-attachments/assets/a0e76579-8348-4c53-99b6-417ee53183b9)

---

#### Configure Trunk Port (To ROUTER1)
```bash
SWITCH01(config)# interface fa0/20
SWITCH01(config-if)# switchport mode trunk
```
---

## 4. Router-on-a-Stick: Inter-VLAN Routing
#### **Configuration Subinterfaces on ROUTER1**

#### VLAN 10
```bash
ROUTER1(config)# interface g0/0/0.10
ROUTER1(config-subif)# encapsulation dot1Q 10
ROUTER1(config-subif)# ip address 192.168.10.1 255.255.255.0
```
#### VLAN 20
```bash
ROUTER1(config)# interface g0/0/0.20
ROUTER1(config-subif)# encapsulation dot1Q 20
ROUTER1(config-subif)# ip address 192.168.20.1 255.255.255.0
```

#### VLAN 30
```bash
ROUTER1(config)# interface g0/0/0.30
ROUTER1(config-subif)# encapsulation dot1Q 30
ROUTER1(config-subif)# ip address 192.168.30.1 255.255.255.0
```
---
## 5. DHCP Server Configuration
**Ref:**
![dhcp ser](https://github.com/user-attachments/assets/e7355172-a316-48f0-9579-481ad6ac824e)

---
## 6. DHCP Relay (IP Helper)
### Forward DHCP Requests in ROUTER1

#### VLAN 20
```bash
ROUTER1(config)# interface g0/0/0.20
ROUTER1(config-subif)# ip helper-address 192.168.10.5
```
#### VLAN 30
```bash
ROUTER1(config)# interface g0/0/0.30
ROUTER1(config-subif)# ip helper-address 192.168.10.5
```
### Tests
**ref: PC1 VLAN 10**
![pcvla10](https://github.com/user-attachments/assets/6a41bb67-4e3a-4244-96b6-4ac7da3c9888)

