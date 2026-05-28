### Main-Router:
```
ena
conf t
hostname IFomin-Main-Router

ip dhcp excluded-address 10.10.10.1 10.10.10.9
ip dhcp excluded-address 10.10.10.100 10.10.10.254
ip dhcp excluded-address 10.10.20.1 10.10.20.9
ip dhcp excluded-address 10.10.20.100 10.10.20.254
ip dhcp excluded-address 10.10.30.1 10.10.30.9
ip dhcp excluded-address 10.10.30.100 10.10.30.254
ip dhcp excluded-address 10.10.40.1 10.10.40.9
ip dhcp excluded-address 10.10.40.100 10.10.40.254
ip dhcp excluded-address 10.10.50.1 10.10.50.9
ip dhcp excluded-address 10.10.50.100 10.10.50.254
ip dhcp excluded-address 10.10.60.1 10.10.60.9
ip dhcp excluded-address 10.10.60.100 10.10.60.254
ip dhcp excluded-address 10.10.135.1 10.10.135.9
ip dhcp excluded-address 10.10.135.100 10.10.135.254

ip dhcp pool IFomin-VLAN10POOL
network 10.10.10.0 255.255.255.0
default-router 10.10.10.254 
domain-name IFomin-VLAN-10.IFomin-Server
dns-server 8.8.8.8 

ip dhcp pool IFomin-VLAN20POOL
network 10.10.20.0 255.255.255.0
default-router 10.10.20.254 
domain-name IFomin-VLAN-20.IFomin-Server
dns-server 8.8.8.8 

ip dhcp pool IFomin-VLAN30POOL
network 10.10.30.0 255.255.255.0
default-router 10.10.30.254 
domain-name IFomin-VLAN-30.IFomin-Server
dns-server 8.8.8.8 

ip dhcp pool IFomin-VLAN40POOL
network 10.10.40.0 255.255.255.0
default-router 10.10.40.254 
domain-name IFomin-VLAN-40.IFomin-Server
dns-server 8.8.8.8 

ip dhcp pool IFomin-VLAN50POOL
network 10.10.50.0 255.255.255.0
default-router 10.10.50.254 
domain-name IFomin-VLAN-50.IFomin-Server
dns-server 8.8.8.8 

ip dhcp pool IFomin-VLAN60POOL
network 10.10.60.0 255.255.255.0
default-router 10.10.60.254 
domain-name IFomin-VLAN-60.IFomin-Server
dns-server 8.8.8.8 

ip dhcp pool IFomin-VLAN135POOL
network 10.10.135.0 255.255.255.0
default-router 10.10.135.254 
domain-name IFomin-VLAN-135.IFomin-Server
dns-server 8.8.8.8 

int e0/0.10
description IFomin-VLAN-10
encapsulation dot1Q 10
ip address 10.10.10.254 255.255.255.0

int e0/0.20
description IFomin-VLAN-20       
encapsulation dot1Q 20
ip address 10.10.20.254 255.255.255.0

int e0/0.30
description IFomin-VLAN-30       
encapsulation dot1Q 30
ip address 10.10.30.254 255.255.255.0
         
int e0/0.40
description IFomin-VLAN-40       
encapsulation dot1Q 40
ip address 10.10.40.254 255.255.255.0

int e0/0.50
description IFomin-VLAN-50       
encapsulation dot1Q 50
ip address 10.10.50.254 255.255.255.0

int e0/0.60
description IFomin-VLAN-60       
encapsulation dot1Q 60
ip address 10.10.60.254 255.255.255.0

int e0/0.70
encapsulation dot1Q 70
ip address 10.10.70.254 255.255.255.0

int e0/0
duplex full
no shutdown

int vlan 70
Description "IFomin-Management"
ip address 10.10.70.1 255.255.255.0
no shutdown 
```

### Switch-1:
```
en
vtp primary
conf t
hostname IFomin-Switch-1

vtp domain IFomin-Server
vtp password 12345678
vtp version 3
vtp mode server

int e0/0
description "To IFomin-Switch-2"
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
description "IFomin-PC-13"
switchport access vlan 10
switchport mode access

int e0/2
description "IFomin-PC-12"
switchport access vlan 10
switchport mode access

int e1/0
description "IFomin-PC-34"
switchport access vlan 20
switchport mode access

int e1/1
description "IFomin-SSH-Client"
switchport access vlan 70
switchport mode access

int Vlan70
description "IFomin-Management"
ip address 10.10.70.1 255.255.255.0

ip route 0.0.0.0 0.0.0.0 10.10.70.254

exit
wr
```

### Switch-2:
```
en
conf t

hostname IFomin-Switch-2

vtp version 3
vtp mode client
vtp domain IFomin-Server
vtp password 1234567

int e0/0
description "To IFomin-Switch-1"
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
description "To IFomin-Switch-3"
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/2
switchport access vlan 20
switchport mode access

int e0/3
switchport access vlan 10
switchport mode access

int e1/0
switchport access vlan 30
switchport mode access

int e1/1
switchport access vlan 30
switchport mode access

int e1/2
switchport access vlan 30
switchport mode access

int Vlan70
description IFomin-Management
ip address 10.10.70.2 255.255.255.0

ip route 0.0.0.0 0.0.0.0 10.10.70.254

exit
wr
```

### Switch-3:
```
enable
conf t
hostname IFomin-Switch-3

vtp version 3
vtp mode client
vtp domain IFomin-Server
vtp password 1234567

int e0/0
switchport trunk encapsulation dot1q
switchport mode trunk
Description "To IFomin-Switch-2"

int e0/1
switchport trunk encapsulation dot1q
switchport mode trunk
Description "To IFomin-Switch-4"

int e0/2
switchport trunk encapsulation dot1q
switchport mode trunk
Description "To IFomin-Main-Router"
duplex full

int e1/0
switchport mode access
switchport access vlan 30

int e0/3
switchport mode access
switchport access vlan 30

int vlan 70
Description "IFomin-Management"
ip address 10.10.70.3 255.255.255.0
no shutdown 
ip route 0.0.0.0 0.0.0.0 10.10.70.254

exit
write
```

### Switch-4:
```
en
conf t

hostname IFomin-Switch-4

vtp version 3
vtp mode client
vtp domain IFomin-Server
vtp password 1234567

int e0/0
description "To IFomin-Switch-3"
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
description "To IFomin-Switch-5"
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/3
description "IFomin-PC-16"
switchport access vlan 50
switchport mode access

int e1/0
description "IFomin-PC-17"
switchport access vlan 50
switchport mode access

int e1/1
description "IFomin-PC-19"
switchport access vlan 60
switchport mode access

int e1/2
description "IFomin-PC-26"
switchport access vlan 40
switchport mode access

int Vlan70
description "IFomin-Management"
ip address 10.10.70.4 255.255.255.0
 
ip route 0.0.0.0 0.0.0.0 10.10.70.254

exit
wr
```

### Switch-5:
```
en
conf t

hostname IFomin-Switch-5

vtp version 3
vtp mode client
vtp domain IFomin-Server
vtp password 1234567

int e0/0
switchport trunk encapsulation dot1q
switchport mode trunk

int e0/1
description "IFomin-PC-27"
switchport access vlan 40
switchport mode access

int e0/2
description "IFomin-PC-20"
switchport access vlan 60
switchport mode access

int e0/3
description "IFomin-PC-21"
switchport access vlan 60
switchport mode access

int e1/0
description "IFomin-Linux-PC-3"
switchport access vlan 60
switchport mode access

int e1/1
description "IFomin-PC-18"
switchport access vlan 50
switchport mode access

int e1/2
description "To IFomin-Switch-6"
switchport trunk encapsulation dot1q
switchport mode trunk

int Vlan70
description "IFomin-Management"
ip address 10.10.70.5 255.255.255.0
 
ip route 0.0.0.0 0.0.0.0 10.10.70.254

exit
wr
```

### PC12:
```
ip dhcp
```