# Router_on_Stick

**_Router-on-a-Stick Configuration_**

**Overview**
This project demonstrates the configuration of a Router-on-a-Stick topology using Cisco Packet Tracer. The topology includes multiple VLANs, DHCP configuration, and inter-VLAN communication via subinterfaces.

**Steps to Configure**
_**Set Router Hostname and Banner:**_

Router(config)#hostname R1
R1(config)#banner motd #Cyber Quince#
Configure Access Ports and VLANs: On Switch S1:

Assign FastEthernet 0/1-8 to VLAN 200.
Assign FastEthernet 0/9-16 to VLAN 100.

S1(config)#interface range FastEthernet 0/1-8
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 200

S1(config)#interface range FastEthernet 0/9-16
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 100
**Set Trunk Ports: On Switch S3:**


S3(config)#interface range GigabitEthernet 0/1-2
S3(config-if-range)#switchport mode trunk
On Switch S2 (Link to Router R1):


S2(config)#interface FastEthernet 0/1
S2(config-if)#switchport mode trunk
**Create VLANs on Switches: On Switch S2:**


S2(config)#vlan 100
S2(config)#vlan 200
_Router-on-a-Stick Subinterfaces: _
**On Router R1:**


R1(config)#interface FastEthernet 0/1
R1(config-if)#no shutdown

R1(config)#interface FastEthernet 0/1.100
R1(config-subif)#encapsulation dot1Q 100
R1(config-subif)#ip address 192.168.1.1 255.255.255.0

R1(config)#interface FastEthernet 0/1.200
R1(config-subif)#encapsulation dot1Q 200
R1(config-subif)#ip address 192.168.2.1 255.255.255.0
Configure DHCP Server: Exclude reserved IPs:


R1(config)#ip dhcp excluded-address 192.168.1.0 192.168.1.10
R1(config)#ip dhcp excluded-address 192.168.2.0 192.168.2.10
_**Create DHCP pools:**_

**For VLAN 100:**

R1(config)#ip dhcp pool Vlan100
R1(dhcp-config)#network 192.168.1.0 255.255.255.0
R1(dhcp-config)#default-router 192.168.1.1


**For VLAN 200:**

R1(config)#ip dhcp pool Vlan200
R1(dhcp-config)#network 192.168.2.0 255.255.255.0
R1(dhcp-config)#default-router 192.168.2.1

**Testing**
Ensure devices in VLAN 100 and VLAN 200 receive IP addresses via DHCP.
Verify inter-VLAN communication using ICMP (ping).

**Notes**
Configure PCs with DHCP to dynamically obtain IP addresses.
Trunk links must be configured correctly to allow tagged traffic.
