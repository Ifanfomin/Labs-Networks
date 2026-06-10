### Доп команды
```
sh ip bgp summary
sh ip bgp
sh ip route
sh run | section bgp

traceroute -d yandex.ru
curl ipinfo.io/2a02:6b8::2:242
```

### C-iBGP-102
```
ena
conf t

hostname IFomin-iBGP-102

int e1/0
description "IFomin-C-iBGP-28"
ip address 13.10.3.102 255.255.255.0
no shutdown

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
 bgp router-id 5.5.5.6
 bgp log-neighbor-changes

 network 13.10.1.0 mask 255.255.255.0
 network 13.10.2.0 mask 255.255.255.0
 network 13.10.3.0 mask 255.255.255.0
 network 16.11.1.0 mask 255.255.255.0
 network 15.11.1.0 mask 255.255.255.0

 redistribute connected

 neighbor 13.10.1.15 remote-as 400
 neighbor 13.10.1.15 next-hop-self
 neighbor 13.10.1.15 route-map NHS_15 out

 neighbor 13.10.2.101 remote-as 400
 neighbor 13.10.2.101 next-hop-self
 neighbor 13.10.2.101 route-map NHS_101 out

 neighbor 13.10.3.2 remote-as 400
 neighbor 13.10.3.2 next-hop-self
 neighbor 13.10.3.2 route-map NHS_28 out

 neighbor 15.11.1.2 remote-as 300
 neighbor 16.11.1.1 remote-as 100
exit

route-map NHS_15 permit 10
 set ip next-hop 13.10.1.102
exit

route-map NHS_101 permit 10
 set ip next-hop 13.10.2.102
exit

route-map NHS_28 permit 10
 set ip next-hop 13.10.3.102
exit

end
write memory
```

### C-iBGP-15
```
ena
conf t

hostname IFomin-iBGP-15

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
 bgp router-id 5.5.5.9
 bgp log-neighbor-changes

 network 13.10.1.0 mask 255.255.255.0
 network 13.10.4.0 mask 255.255.255.0
 network 13.10.5.0 mask 255.255.255.0
 network 13.32.12.0 mask 255.255.255.0

 neighbor 13.10.1.102 remote-as 400
 neighbor 13.10.1.102 route-reflector-client
 neighbor 13.10.1.102 next-hop-self
 neighbor 13.10.1.102 route-map NHS_102 out

 neighbor 13.10.4.23 remote-as 400
 neighbor 13.10.4.23 route-reflector-client
 neighbor 13.10.4.23 next-hop-self
 neighbor 13.10.4.23 route-map NHS_23 out

 neighbor 13.10.5.101 remote-as 400
 neighbor 13.10.5.101 route-reflector-client
 neighbor 13.10.5.101 next-hop-self
 neighbor 13.10.5.101 route-map NHS_101 out

 neighbor 13.32.12.85 remote-as 400
 neighbor 13.32.12.85 route-reflector-client
 neighbor 13.32.12.85 next-hop-self
 neighbor 13.32.12.85 route-map NHS_85 out
exit

route-map NHS_102 permit 10
 set ip next-hop 13.10.1.15
exit

route-map NHS_23 permit 10
 set ip next-hop 13.10.4.15
exit

route-map NHS_101 permit 10
 set ip next-hop 13.10.5.15
exit

route-map NHS_85 permit 10
 set ip next-hop 13.32.12.15
exit

end
write memory
```

### M-iBGP-101
```
/system identity
set name=IFomin-m-ibgp-101

/interface ethernet
set ether1 comment="To IFomin-C-iBGP-102"
set ether2 comment="To IFomin-C-iBGP-28"
set ether3 comment="To IFomin-C-iBGP-15"
set ether4 comment="To IFomin-C-iBGP-23"
set ether5 comment="To IFomin-C-iBGP-33"

/interface bridge
add name=loopback

/routing bgp instance
set default as=400 router-id=5.5.7.1 redistribute-connected=yes

/ip address
add interface=ether1 address=13.10.2.101/24 comment="To IFomin-C-iBGP-102" network=13.10.2.0
add interface=ether2 address=13.10.6.101/24 comment="To IFomin-C-iBGP-28" network=13.10.6.0
add interface=ether3 address=13.10.5.101/24 comment="To IFomin-C-iBGP-15" network=13.10.5.0
add interface=ether4 address=13.10.8.101/24 comment="To IFomin-C-iBGP-23" network=13.10.8.0
add interface=ether5 address=13.10.9.101/24 comment="To IFomin-C-iBGP-33" network=13.10.9.0
add address=5.5.7.1 interface=loopback network=5.5.7.1

/ip dhcp-client remove number=0

/routing bgp network
add network=13.10.2.0/24
add network=13.10.5.0/24
add network=13.10.6.0/24
add network=13.10.8.0/24
add network=13.10.9.0/24

/routing filter
add chain=NHS_102 action=accept set-out-nexthop=13.10.2.101
add chain=NHS_28 action=accept set-out-nexthop=13.10.6.101
add chain=NHS_15 action=accept set-out-nexthop=13.10.5.101
add chain=NHS_23 action=accept set-out-nexthop=13.10.8.101
add chain=NHS_33 action=accept set-out-nexthop=13.10.9.101

/routing bgp peer
add name=to-c-ibgp-102 \
    remote-address=13.10.2.102 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_102

add name=to-c-ibgp-28 \
    remote-address=13.10.6.28 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_28

add name=to-c-ibgp-15 \
    remote-address=13.10.5.15 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_15

add name=to-c-ibgp-23 \
    remote-address=13.10.8.23 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_23

add name=to-c-ibgp-33 \
    remote-address=13.10.9.33 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_33
```

### C-iBGP-28
```
ena
conf t

hostname IFomin-ibgp-28

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
 bgp router-id 5.5.5.7
 bgp log-neighbor-changes

 network 13.10.3.0 mask 255.255.255.0
 network 13.10.6.0 mask 255.255.255.0
 network 13.10.7.0 mask 255.255.255.0

 neighbor 13.10.3.102 remote-as 400
 neighbor 13.10.3.102 route-reflector-client
 neighbor 13.10.3.102 next-hop-self
 neighbor 13.10.3.102 route-map NHS_102 out

 neighbor 13.10.6.101 remote-as 400
 neighbor 13.10.6.101 route-reflector-client
 neighbor 13.10.6.101 next-hop-self
 neighbor 13.10.6.101 route-map NHS_101 out

 neighbor 13.10.7.33 remote-as 400
 neighbor 13.10.7.33 route-reflector-client
 neighbor 13.10.7.33 next-hop-self
 neighbor 13.10.7.33 route-map NHS_33 out
exit

route-map NHS_102 permit 10
 set ip next-hop 13.10.3.2
exit

route-map NHS_101 permit 10
 set ip next-hop 13.10.6.28
exit

route-map NHS_33 permit 10
 set ip next-hop 13.10.7.28
exit

end
write memory
```

### C-iBGP-33
```
ena
conf t

hostname IFomin-ibgp-33

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
 bgp log-neighbor-changes

 network 13.10.7.0 mask 255.255.255.0
 network 13.10.9.0 mask 255.255.255.0
 network 13.10.10.0 mask 255.255.255.0
 network 13.32.15.0 mask 255.255.255.0

 neighbor 13.10.7.28 remote-as 400
 neighbor 13.10.7.28 route-reflector-client
 neighbor 13.10.7.28 next-hop-self
 neighbor 13.10.7.28 route-map NHS_to_28 out

 neighbor 13.10.9.101 remote-as 400
 neighbor 13.10.9.101 route-reflector-client
 neighbor 13.10.9.101 next-hop-self
 neighbor 13.10.9.101 route-map NHS_to_101 out

 neighbor 13.10.10.23 remote-as 400
 neighbor 13.10.10.23 route-reflector-client
 neighbor 13.10.10.23 next-hop-self
 neighbor 13.10.10.23 route-map NHS_to_23 out

 neighbor 13.32.15.42 remote-as 400
 neighbor 13.32.15.42 route-reflector-client
 neighbor 13.32.15.42 next-hop-self
 neighbor 13.32.15.42 route-map NHS_to_42 out
exit

route-map NHS_to_28 permit 10
 set ip next-hop 13.10.7.33
exit

route-map NHS_to_101 permit 10
 set ip next-hop 13.10.9.33
exit

route-map NHS_to_23 permit 10
 set ip next-hop 13.10.10.33
exit

route-map NHS_to_42 permit 10
 set ip next-hop 13.32.15.33
exit

end
write memory
```

### C-iBGP-23
```
ena
conf t

hostname IFomin-ibgp-23

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

router bgp 400
 bgp router-id 5.5.6.1
 bgp log-neighbor-changes

 network 13.10.4.0 mask 255.255.255.0
 network 13.10.8.0 mask 255.255.255.0
 network 13.10.10.0 mask 255.255.255.0
 network 13.32.13.0 mask 255.255.255.0
 network 13.32.16.0 mask 255.255.255.0
 network 13.32.17.0 mask 255.255.255.0

 neighbor 13.10.4.15 remote-as 400
 neighbor 13.10.4.15 route-reflector-client
 neighbor 13.10.4.15 next-hop-self
 neighbor 13.10.4.15 route-map NHS_15 out

 neighbor 13.10.8.101 remote-as 400
 neighbor 13.10.8.101 route-reflector-client
 neighbor 13.10.8.101 next-hop-self
 neighbor 13.10.8.101 route-map NHS_101 out

 neighbor 13.10.10.33 remote-as 400
 neighbor 13.10.10.33 route-reflector-client
 neighbor 13.10.10.33 next-hop-self
 neighbor 13.10.10.33 route-map NHS_33 out

 neighbor 13.32.13.62 remote-as 400
 neighbor 13.32.13.62 route-reflector-client
 neighbor 13.32.13.62 next-hop-self
 neighbor 13.32.13.62 route-map NHS_62 out

 neighbor 13.32.16.42 remote-as 400
 neighbor 13.32.16.42 route-reflector-client
 neighbor 13.32.16.42 next-hop-self
 neighbor 13.32.16.42 route-map NHS_42 out

 neighbor 13.32.17.85 remote-as 400
 neighbor 13.32.17.85 route-reflector-client
 neighbor 13.32.17.85 next-hop-self
 neighbor 13.32.17.85 route-map NHS_85 out
exit

route-map NHS_15 permit 10
 set ip next-hop 13.10.4.23
exit

route-map NHS_101 permit 10
 set ip next-hop 13.10.8.23
exit

route-map NHS_33 permit 10
 set ip next-hop 13.10.10.23
exit

route-map NHS_62 permit 10
 set ip next-hop 13.32.13.23
exit

route-map NHS_42 permit 10
 set ip next-hop 13.32.16.23
exit

route-map NHS_85 permit 10
 set ip next-hop 13.32.17.23
exit

end
write memory
```

### M-iBGP-85
```
/system identity
set name=IFomin-m-ibgp-85


/interface ethernet
set ether1 comment="To IFomin-M-iBGP-62"
set ether2 comment="To IFomin-C-iBGP-15"
set ether3 comment="To IFomin-C-iBGP-23"

/interface bridge
add name=loopback

/routing bgp instance
set default as=400 router-id=5.5.8.0 redistribute-connected=yes

/ip address
add interface=ether1 address=13.32.11.85/24 comment="To IFomin-M-iBGP-62" network=13.32.11.0
add interface=ether2 address=13.32.12.85/24 comment="To IFomin-C-iBGP-15" network=13.32.12.0
add interface=ether3 address=13.32.17.85/24 comment="To IFomin-C-iBGP-23" network=13.32.17.0
add address=5.5.8.0/24 interface=loopback network=5.5.8.0

/ip dhcp-client remove number=0

/routing bgp network
add network=13.32.11.0/24
add network=13.32.12.0/24
add network=13.32.17.0/24

/routing filter
add chain=NHS_TO_15 action=accept set-out-nexthop=13.32.12.85
add chain=NHS_TO_23 action=accept set-out-nexthop=13.32.17.85
add chain=NHS_TO_62 action=accept set-out-nexthop=13.32.11.85

/routing bgp peer
add name=to-c-ibgp-15 \
    remote-address=13.32.12.15 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_TO_15

add name=to-c-ibgp-23 \
    remote-address=13.32.17.23 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_TO_23

add name=to-c-ibgp-62 \
    remote-address=13.32.11.62 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_TO_62
```

### M-iBGP-62
```
/system identity
set name=IFomin-m-ibgp-62

/interface ethernet
set ether1 comment="To iBGP-85"
set ether2 comment="To M-iBGP-42"
set ether3 comment="To iBGP-23"

/interface bridge
add name=loopback

/routing bgp instance
set default as=400 router-id=5.5.6.5 redistribute-connected=yes

/ip address
add interface=ether1 address=13.32.11.62/24 comment="To IFomin-M-iBGP-85" network=13.32.11.0
add interface=ether2 address=13.32.14.62/24 comment="To IFomin-M-iBGP-42" network=13.32.14.0
add interface=ether3 address=13.32.13.62/24 comment="To IFomin-C-iBGP-23" network=13.32.13.0
add address=5.5.6.5 interface=loopback network=5.5.6.5

/ip dhcp-client remove number=0

/routing bgp network
add network=13.32.11.0/32
add network=13.32.13.0/32
add network=13.32.14.0/32

/routing filter
add chain=NHS_to_85 action=accept set-out-nexthop=13.32.11.62
add chain=NHS_to_42 action=accept set-out-nexthop=13.32.14.62
add chain=NHS_to_23 action=accept set-out-nexthop=13.32.13.62

/routing bgp peer
add name=to-m-ibgp-85 \
    remote-address=13.32.11.85 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_to_85

add name=to-m-ibgp-42 \
    remote-address=13.32.14.42 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_to_42

add name=to-m-ibgp-23 \
    remote-address=13.32.13.23 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_to_23
```

### M-iBGP-42
```
/system identity
set name=IFomin-m--ibgp-42

/interface ethernet
set ether1 comment="To IFomin-M-iBGP-62"
set ether2 comment="To IFomin-С-iBGP-33"
set ether3 comment="To IFomin-C-iBGP-23"

/interface bridge
add name=loopback

/routing bgp instance
set default as=400 router-id=5.5.6.9 redistribute-connected=yes

/ip address
add interface=ether1 address=13.32.14.42/24 comment="To IFomin-M-iBGP-62" network=13.32.14.0
add interface=ether2 address=13.32.15.42/24 comment="To IFomin-С-iBGP-33" network=13.32.15.0
add interface=ether3 address=13.32.16.42/24 comment="To IFomin-C-iBGP-23" network=13.32.16.0
add address=5.5.6.9 interface=loopback network=5.5.6.9

/ip dhcp-client remove number=0

/routing bgp network
add network=13.32.14.0/24
add network=13.32.15.0/24
add network=13.32.16.0/24

/routing filter
add chain=NHS_62 action=accept set-out-nexthop=13.32.14.42
add chain=NHS_23 action=accept set-out-nexthop=13.32.16.42
add chain=NHS_33 action=accept set-out-nexthop=13.32.15.42

/routing bgp peer
add name=to-m-ibgp-62 \
    remote-address=13.32.14.62 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_62

add name=to-m-ibgp-23 \
    remote-address=13.32.16.23 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_23

add name=to-m-ibgp-33 \
    remote-address=13.32.15.33 \
    remote-as=400 \
    route-reflect=yes \
    nexthop-choice=force-self \
    out-filter=NHS_33
```
