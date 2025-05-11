
# Cisco Switch & Router Core configuration, VLAN Segmentation & DHCP Inter-VLAN Routing Lab
---

## Project 
**Switch & Router Security, VLAN Management and DHCP Inter-VLAN Routing (Packet Tracer)**
## Objective

Designed and simulated a secure enterprise network in Cisco Packet Tracer, implementing VLAN segmentation, router-on-a-stick inter-VLAN routing, and centralized DHCP. Strengthened security, port security, and access controls practicing realworld network administration in a virtual lab.

---

## Skills Learned

- Cisco IOS CLI configuration
- Device hardening techniques
- VLAN creation and interface assignment
- Inter-VLAN routing via subinterfaces
- DHCP setup and relay forwarding
- Security banners, password encryption, and Telnet/SSH access

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
![topolo](https://github.com/user-attachments/assets/b2d88443-393d-4cce-bc64-2fad74d9c671)


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
Warning!! Authorized Users Only!!
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
SWITCH01(config-if)# ip address 192.168.50.100 255.255.255.0
SWITCH01(config-if)# no shutdown
```
**Ref: SWITCH01**
![sw hard](https://github.com/user-attachments/assets/b95f3383-77fc-4c5f-9b5c-c05a96083576)

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
============================#
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
![vln](https://github.com/user-attachments/assets/8edbc04c-715b-4069-b4d5-77df25b4f939)

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
ROUTER1(config)# interface g0/0.10
ROUTER1(config-subif)# encapsulation dot1Q 10
ROUTER1(config-subif)# ip address 192.168.10.1 255.255.255.0
```
#### VLAN 20
```bash
ROUTER1(config)# interface g0/0.20
ROUTER1(config-subif)# encapsulation dot1Q 20
ROUTER1(config-subif)# ip address 192.168.20.1 255.255.255.0
```

#### VLAN 30
```bash
ROUTER1(config)# interface g0/0.30
ROUTER1(config-subif)# encapsulation dot1Q 30
ROUTER1(config-subif)# ip address 192.168.30.1 255.255.255.0
```
---
## 5. DHCP Server Configuration
**Ref:**
![server sett](https://github.com/user-attachments/assets/79f95423-13f5-4d5c-8f99-98a9de49a4a8)

---
## 6. DHCP Relay (IP Helper)
### Forward DHCP Requests in ROUTER1

#### VLAN 20
```bash
ROUTER1(config)# interface g0/0.20
ROUTER1(config-subif)# ip helper-address 192.168.10.5
```
#### VLAN 30
```bash
ROUTER1(config)# interface g0/0.30
ROUTER1(config-subif)# ip helper-address 192.168.10.5
#
**DHCP server is in vlan 10, No DHCP-relay needed for vlan10**
```
### Tests DHCP
**Ref: HR Lola-PC VLAN 30**
![DHCPP](https://github.com/user-attachments/assets/a02847fa-c09f-4ac8-b0b2-0f2aef92765c)

---
**Ref: IT Victor-PC VLAN 20**
![testvictor](https://github.com/user-attachments/assets/3dcf2acd-f534-4aaf-a9aa-e080700c8d07)

---
