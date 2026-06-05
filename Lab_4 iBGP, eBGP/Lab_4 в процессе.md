### Доп команды
```
sh ip bgp summary
sh ip bgp
sh ip route
sh run | section bgp

trace -d yandex.ru
curl ipinfo.io/77.88.44.55
```

### C-iBGP-102
```
ena 
conf t
hostname IFomin-C-iBGP-102

int e0/0
description "IFomin-Main-Router"
ip address 16.11.1.2 255.255.255.252
no shutdown

int e0/1
description "IFomin-M-OSPF-10"
ip address 15.11.1.1 255.255.255.252
no shutdown

int e0/2
description "IFomin-C-iBGP-15"
ip address 13.10.1.102 255.255.255.0
no shutdown

int e0/3
description "IFomin-C-iBGP-101"
ip address 13.10.2.102 255.255.255.0
no shutdown

int e1/0
description "IFomin-C-iBGP-28"
ip address 13.10.3.102 255.255.255.0
no shutdown

router bgp 400
network 13.10.1.0 mask 255.255.255.0
network 13.10.2.0 mask 255.255.255.0
network 13.10.3.0 mask 255.255.255.0
network 16.11.1.0 mask 255.255.255.0
network 15.11.1.0 mask 255.255.255.0

neighbor 16.11.1.1 remote-as 100
neighbor 15.11.1.2 remote-as 300

neighbor 13.10.1.15 remote-as 400
neighbor 13.10.2.101 remote-as 400
neighbor 13.10.3.28 remote-as 400

neighbor 13.10.1.15 route-map NH_15 out
neighbor 13.10.2.101 route-map NH_101 out
neighbor 13.10.3.28 route-map NH_28 out

neighbor 13.10.1.15 route-reflector-client
neighbor 13.10.2.101 route-reflector-client
neighbor 13.10.3.28 route-reflector-client

route-map NH_15 permit 10 
set ip next-hop 13.10.1.102

route-map NH_101 permit 10 
set ip next-hop 13.10.2.102

route-map NH_28 permit 10 
set ip next-hop 13.10.3.102


do wr
```

### C-iBGP-15
```
ena 
conf t
hostname IFomin-C-iBGP-15

int e0/0
description "IFomin-C-iBGP-102"
ip address 13.10.1.15 255.255.255.0
no shutdown

int e0/1
description "IFomin-M-iBGP-85"
ip address 13.32.12.15 255.255.255.0
no shutdown

int e0/2
description "IFomin-C-iBGP-23"
ip address 13.10.4.15 255.255.255.0
no shutdown

int e0/3
description "IFomin-M-iBGP-101"
ip address 13.10.5.15 255.255.255.0
no shutdown

router bgp 400
network 13.10.1.0 mask 255.255.255.0
network 13.10.4.0 mask 255.255.255.0
network 13.10.5.0 mask 255.255.255.0
network 13.32.12.0 mask 255.255.255.0

neighbor 13.10.1.102 remote-as 400
neighbor 13.10.4.23 remote-as 400
neighbor 13.10.5.101 remote-as 400
neighbor 13.32.12.85 remote-as 400

neighbor 13.10.1.102 route-map NH_102 out
neighbor 13.10.4.23 route-map NH_23 out
neighbor 13.10.5.101 route-map NH_101 out
neighbor 13.32.12.85 route-map NH_85 out

neighbor 13.10.1.102 route-reflector-client
neighbor 13.10.4.23 route-reflector-client
neighbor 13.10.5.101 route-reflector-client
neighbor 13.32.12.85 route-reflector-client

route-map NH_102 permit 10 
set ip next-hop 13.10.1.15

route-map NH_23 permit 10 
set ip next-hop 13.10.4.15

route-map NH_101 permit 10 
set ip next-hop 13.10.5.15

route-map NH_85 permit 10 
set ip next-hop 13.32.12.15

do wr
```

### M-iBGP-101
```
/system identity set name=IFomin-M-OSPF-101

/interface ethernet
set ether1 comment="To IFomin-C-iBGP-102"
set ether2 comment="To IFomin-C-iBGP-28"
set ether3 comment="To IFomin-C-iBGP-15"
set ether4 comment="To IFomin-C-iBGP-23"
set ether5 comment="To IFomin-C-iBGP-33"

/interface bridge
add name=loopback0 comment="Loopback"

/ip dhcp-client remove number=0
/ip address
add interface=ether1 address=13.10.2.101/24 comment="To IFomin-C-iBGP-102"
add interface=ether2 address=13.10.6.101/24 comment="To IFomin-C-iBGP-28"
add interface=ether3 address=13.10.5.101/24 comment="To IFomin-C-iBGP-15"
add interface=ether4 address=13.10.8.101/24 comment="To IFomin-C-iBGP-23"
add interface=ether5 address=13.10.9.101/24 comment="To IFomin-C-iBGP-33"
add interface=loopback0 address=9.9.9.9/24 comment="Loopback"

/routing bgp
instance add as=400 name=BGP400 redistribute-connected=yes router-id=9.9.9.9
peer add remote-address=13.10.2.102 remote-as=400 instance=BGP400 route-reflect=yes nexthop-choice=force-self
peer add remote-address=13.10.6.28 remote-as=400 instance=BGP400 route-reflect=yes nexthop-choice=force-self
peer add remote-address=13.10.5.15 remote-as=400 instance=BGP400 route-reflect=yes nexthop-choice=force-self
peer add remote-address=13.10.8.23 remote-as=400 instance=BGP400 route-reflect=yes nexthop-choice=force-self
peer add remote-address=13.10.9.33 remote-as=400 instance=BGP400 route-reflect=yes nexthop-choice=force-self
```

### C-iBGP-28
```
ena 
conf t
hostname IFomin-C-iBGP-28

int e0/1
description "IFomin-C-iBGP-102"
ip address 13.10.3.28 255.255.255.0
no shutdown

int e0/2
description "IFomin-C-iBGP-33"
ip address 13.10.7.28 255.255.255.0
no shutdown

int e0/3
description "IFomin-M-iBGP-101"
ip address 13.10.6.28 255.255.255.0
no shutdown


router bgp 400
network 13.10.3.0 mask 255.255.255.0
network 13.10.7.0 mask 255.255.255.0
network 13.10.6.0 mask 255.255.255.0

neighbor 13.10.3.102 remote-as 400
neighbor 13.10.7.33 remote-as 400
neighbor 13.10.6.101 remote-as 400

neighbor 13.10.3.102 route-map NH_102 out
neighbor 13.10.7.33 route-map NH_33 out
neighbor 13.10.6.101 route-map NH_101 out

neighbor 13.10.3.102 route-reflector-client
neighbor 13.10.7.33 route-reflector-client
neighbor 13.10.6.101 route-reflector-client

route-map NH_102 permit 10 
set ip next-hop 13.10.3.28

route-map NH_33 permit 10 
set ip next-hop 13.10.7.28

route-map NH_101 permit 10 
set ip next-hop 13.10.6.28

do wr
```

### C-iBGP-33
```
ena 
conf t
hostname IFomin-C-iBGP-33

int e0/0
description "IFomin-C-iBGP-28"
ip address 13.10.7.33 255.255.255.0
no shutdown

int e0/1
description "IFomin-C-iBGP-23"
ip address 13.10.10.33 255.255.255.0
no shutdown

int e0/2
description "IFomin-M-iBGP-101"
ip address 13.10.9.33 255.255.255.0
no shutdown

int e0/3
description "IFomin-M-iBGP-42"
ip address 13.32.15.33 255.255.255.0
no shutdown


router bgp 400
network 13.10.7.0 mask 255.255.255.0
network 13.10.10.0 mask 255.255.255.0
network 13.10.9.0 mask 255.255.255.0
network 13.32.15.0 mask 255.255.255.0

neighbor 13.10.7.28 remote-as 400
neighbor 13.10.10.23 remote-as 400
neighbor 13.10.9.101 remote-as 400
neighbor 13.32.15.42 remote-as 400

neighbor 13.10.7.28 route-map NH_28 out
neighbor 13.10.10.23 route-map NH_23 out
neighbor 13.10.9.101 route-map NH_101 out
neighbor 13.32.15.42 route-map NH_42 out

neighbor 13.10.7.28 route-reflector-client
neighbor 13.10.10.23 route-reflector-client
neighbor 13.10.9.101 route-reflector-client
neighbor 13.32.15.42 route-reflector-client

route-map NH_28 permit 10 
set ip next-hop 13.10.7.33

route-map NH_23 permit 10 
set ip next-hop 13.10.10.33

route-map NH_101 permit 10 
set ip next-hop 13.10.9.33

route-map NH_42 permit 10 
set ip next-hop 13.32.15.33

do wr
```

### C-iBGP-23
```
ena 
conf t
hostname IFomin-C-iBGP-23

int e0/0
description "IFomin-C-iBGP-15"
ip address 13.10.4.23 255.255.255.0
no shutdown

int e0/1
description "IFomin-C-iBGP-33"
ip address 13.10.10.23 255.255.255.0
no shutdown

int e0/2
description "IFomin-M-iBGP-101"
ip address 13.10.8.23 255.255.255.0
no shutdown

int e0/3
description "IFomin-M-iBGP-62"
ip address 13.32.13.23 255.255.255.0
no shutdown

int e1/0
description "IFomin-M-iBGP-85"
ip address 13.32.17.23 255.255.255.0
no shutdown

int e1/1
description "IFomin-M-iBGP-42"
ip address 13.32.16.23 255.255.255.0
no shutdown

int loopback 0
description "Loopback"
ip address 23.23.23.23 255.255.255.255
no shutdown

router bgp 400
bgp router-id 23.23.23.23
network 13.10.4.0 mask 255.255.255.0
network 13.10.10.0 mask 255.255.255.0
network 13.10.8.0 mask 255.255.255.0
network 13.32.13.0 mask 255.255.255.0
network 13.32.17.0 mask 255.255.255.0
network 13.32.16.0 mask 255.255.255.0

network 23.23.23.23 mask 255.255.255.255

neighbor 13.10.4.15 remote-as 400
neighbor 13.10.10.33 remote-as 400
neighbor 13.10.8.101 remote-as 400
neighbor 13.32.13.62 remote-as 400
neighbor 13.32.17.85 remote-as 400
neighbor 13.32.16.42 remote-as 400

neighbor 13.10.4.15 route-map NH_15 out
neighbor 13.10.10.33 route-map NH_33 out
neighbor 13.10.8.101 route-map NH_101 out
neighbor 13.32.13.62 route-map NH_62 out
neighbor 13.32.17.85 route-map NH_85 out
neighbor 13.32.16.42 route-map NH_42 out

neighbor 13.10.4.15 route-reflector-client
neighbor 13.10.10.33 route-reflector-client
neighbor 13.10.8.101 route-reflector-client
neighbor 13.32.13.62 route-reflector-client
neighbor 13.32.17.85 route-reflector-client
neighbor 13.32.16.42 route-reflector-client

route-map NH_15 permit 10 
set ip next-hop 13.10.4.23

route-map NH_33 permit 10 
set ip next-hop 13.10.10.23

route-map NH_101 permit 10 
set ip next-hop 13.10.8.23

route-map NH_62 permit 10 
set ip next-hop 13.32.13.23

route-map NH_85 permit 10 
set ip next-hop 13.32.17.23

route-map NH_42 permit 10 
set ip next-hop 13.32.16.23

do wr
```

### M-iBGP-85
```
/system identity set name=IFomin-M-OSPF-85

/interface ethernet
set ether1 comment="To IFomin-M-iBGP-62"
set ether2 comment="To IFomin-C-iBGP-15"
set ether3 comment="To IFomin-C-iBGP-23"

/ip dhcp-client remove number=0
/ip address
add interface=ether1 address=13.32.11.85/24 comment="To IFomin-M-iBGP-62"
add interface=ether2 address=13.32.12.85/24 comment="To IFomin-C-iBGP-15"
add interface=ether3 address=13.32.17.85/24 comment="To IFomin-C-iBGP-23"

/routing bgp
instance add as=400 name=BGP400 redistribute-connected=yes router-id=13.32.11.85
peer add remote-address=13.32.11.62 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
peer add remote-address=13.32.12.15 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
peer add remote-address=13.32.17.23 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
```

### M-iBGP-62
```
/system identity set name=IFomin-M-OSPF-62

/interface ethernet
set ether1 comment="To IFomin-M-iBGP-85"
set ether2 comment="To IFomin-M-iBGP-42"
set ether3 comment="To IFomin-C-iBGP-23"

/ip dhcp-client remove number=0
/ip address
add interface=ether1 address=13.32.11.62/24 comment="To IFomin-M-iBGP-85"
add interface=ether2 address=13.32.14.62/24 comment="To IFomin-M-iBGP-42"
add interface=ether3 address=13.32.13.62/24 comment="To IFomin-C-iBGP-23"

/routing bgp
instance add as=400 name=BGP400 redistribute-connected=yes router-id=13.32.11.62
peer add remote-address=13.32.11.85 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
peer add remote-address=13.32.14.42 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
peer add remote-address=13.32.13.23 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
```

### M-iBGP-42
```
/system identity set name=IFomin-M-OSPF-42

/interface ethernet
set ether1 comment="To IFomin-M-iBGP-62"
set ether2 comment="To IFomin-С-iBGP-33"
set ether3 comment="To IFomin-C-iBGP-23"

/ip dhcp-client remove number=0
/ip address
add interface=ether1 address=13.32.14.42/24 comment="To IFomin-M-iBGP-62"
add interface=ether2 address=13.32.15.42/24 comment="To IFomin-С-iBGP-33"
add interface=ether3 address=13.32.16.42/24 comment="To IFomin-C-iBGP-23"

/routing filter
add chain=NH_62 set-in-nexthop=13.32.14.42 action=accept
add chain=NH_33 set-in-nexthop=13.32.15.42 action=accept
add chain=NH_23 set-in-nexthop=13.32.16.42 action=accept

/routing bgp
instance add as=400 name=BGP400 router-id=13.32.14.42
peer add remote-address=13.32.14.62 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
peer add remote-address=13.32.15.33 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
peer add remote-address=13.32.16.23 remote-as=400 instance=BGP400 nexthop-choice=force-self route-reflect=yes
```



------------------------------------------------------
------------------------------------------------------
### M-iBGP-113
```
/system identity set name=IFomin-M-OSPF-113

/interface ethernet
set ether1 comment="To IFomin-M-OSPF-3"
set ether2 comment="To IFomin-M-OSPF-2"
set ether3 comment="To IFomin-M-OSPF-5"
set ether4 comment="To IFomin-C-OSPF-5"
set ether5 comment="To IFomin-L-OSPF-1"

/ip dhcp-client remove number=0
/ip address
add address=12.22.12.2/30 interface=ether1 disabled=no comment="To IFomin-M-OSPF-3"
add address=12.21.11.2/30 interface=ether2 disabled=no comment="To IFomin-M-OSPF-2"
add address=12.22.15.1/30 interface=ether3 disabled=no comment="To IFomin-M-OSPF-5"
add address=12.21.12.1/30 interface=ether4 disabled=no comment="To IFomin-C-OSPF-5"
add address=12.27.22.1/30 interface=ether5 disabled=no comment="To IFomin-L-OSPF-1"

/routing bgp
instance add as=400 name=BGP400 redistribute-connected=yes router-id=13.1.12.113
peer add remote-address=13.1.11.23 remote-as=400 instance=BGP400
peer add remote-address=13.1.11.33 remote-as=400 instance=BGP400
```