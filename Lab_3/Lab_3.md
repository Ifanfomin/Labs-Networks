## RIP
### C-RIP-1
```
ena
conf t
hostname IFomin-C-RIP-1

int e0/0
Description "To IFomin-Main-Router"
ip address 17.11.1.2 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-RIP-3"
ip address 140.66.8.1 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-C-RIP-2"
ip address 140.66.9.1 255.255.255.252
no shutdown

int e0/3
description "To M-OSPF-10"
ip address 18.11.1.1 255.255.255.252
no shutdown

router rip
version 2

network 140.66.8.0
network 140.66.9.0
network 17.11.1.0
network 18.11.1.0
no auto-summary
redistribute bgp 200 metric 1

router bgp 200
neighbor 17.11.1.1 remote-as 100
neighbor 18.11.1.2 remote-as 300
redistribute connected
redistribute rip

exit
exit
wr
```

### C-RIP-2
```
ena
conf t
hostname IFomin-C-RIP-2

int e0/0
Description "To IFomin-C-RIP-3"
ip address 100.130.85.1 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-RIP-1"
ip address 140.66.9.2 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-C-RIP-4"
ip address 213.244.11.1 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-C-RIP-5"
ip address 93.56.249.1 255.255.255.252
no shutdown

router rip
version 2

no auto-summary
network 140.66.9.0
network 100.130.85.0
network 213.244.11.0
network 93.56.249.0

exit
exit
wr
```

### C-RIP-3
```
ena
conf t
hostname IFomin-C-RIP-3

int e0/0
Description "To IFomin-C-RIP-1"
ip address 140.66.8.2 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-RIP-2"
ip address 100.130.85.2 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-C-RIP-4"
ip address 213.244.10.1 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-C-RIP-6"
ip address 75.110.43.1 255.255.255.252
no shutdown

router rip
version 2
no auto-summary
network 213.244.10.0
network 100.130.85.0
network 140.66.8.0
network 75.110.43.0

exit
exit
wr
```

### C-RIP-4
```
ena
conf t
hostname IFomin-C-RIP-4

int e0/0
Description "To IFomin-C-RIP-2"
ip address 213.244.11.2 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-RIP-3"
ip address 213.244.10.2 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-C-RIP-6"
ip address 213.244.13.1 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-C-RIP-5"
ip address 213.244.12.1 255.255.255.252
no shutdown

router rip
version 2
no auto-summary
network 213.244.10.0
network 213.244.13.0
network 213.244.12.0
network 213.244.11.0

exit
exit

wr
```

### C-RIP-5
```
ena
conf t
hostname IFomin-C-RIP-5

int e0/0
Description "To IFomin-C-RIP-4"
ip address 213.244.12.2 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-RIP-7"
ip address 80.28.81.1 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-C-RIP-6"
ip address 100.130.86.1 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-M-RIP-2"
ip address 93.56.240.1 255.255.255.252
no shutdown

int e1/0
Description "To IFomin-C-RIP-2"
ip address 93.56.249.2 255.255.255.252
no shutdown

router rip
version 2
no auto-summary
network 213.244.12.0
network 80.28.81.0
network 100.130.86.0
network 93.56.240.0
network 93.56.249.0

exit
exit

wr
```

### C-RIP-6
```
ena
conf t
hostname IFomin-C-RIP-6

int e0/0
Description "To IFomin-C-RIP-4"
ip address 213.244.13.2 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-RIP-7"
ip address 80.28.80.1 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-C-RIP-5"
ip address 100.130.86.2 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-C-RIP-3"
ip address 75.110.43.2 255.255.255.252
no shutdown

int e1/0
Description "To IFomin-M-RIP-1"
ip address 75.110.44.1 255.255.255.252
no shutdown

int e1/1
Description "To IFomin-L-RIP-11"
ip address 75.110.46.1 255.255.255.252
no shutdown

router rip
version 2
no auto-summary
network 213.244.13.0
network 80.28.80.0
network 100.130.86.0
network 75.110.43.0
network 75.110.44.0
network 75.110.46.0

exit
exit

wr
```

### C-RIP-7
```
ena
conf t
hostname IFomin-C-RIP-7

int e0/0
Description "To IFomin-C-RIP-6"
ip address 80.28.80.2 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-RIP-5"
ip address 80.28.81.2 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-M-RIP-2"
ip address 80.28.82.1 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-M-RIP-2"
ip address 80.28.84.1 255.255.255.252
no shutdown

int e1/0
Description "To IFomin-C-RIP-10"
ip address 80.28.83.1 255.255.255.252
no shutdown

router rip
version 2
no auto-summary
network 80.28.80.0
network 80.28.81.0
network 80.28.82.0
network 80.28.84.0
network 80.28.83.0

exit
exit

wr
```

### C-RIP-10
```
ena
conf t
hostname IFomin-C-RIP-10

int e0/0
Description "To IFomin-M-RIP-1"
ip address 186.23.48.1 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-M-RIP-2"
ip address 186.22.48.2 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-C-RIP-7"
ip address 80.28.83.2 255.255.255.252
no shutdown

router rip
version 2
no auto-summary
network 186.23.48.0
network 186.22.48.0
network 80.28.83.0

exit
exit

wr
```

### M-RIP-1
```
/system identity set name=IFomin-M-RIP-1

/interface ethernet
set ether1 comment="To IFomin-C-RIP-7"
set ether2 comment="To IFomin-C-RIP-6"
set ether3 comment="To IFomin-C-RIP-10"
set ether4 comment="To IFomin-Interent-RIP"

/ip dhcp-client remove number=0
/ip address
add address=80.28.84.2/30 interface=ether1 disabled=no comment="To IFomin-C-RIP-7"
add address=75.110.44.2/30 interface=ether2 disabled=no comment="To IFomin-C-RIP-6"
add address=186.23.48.2/30 interface=ether3 disabled=no comment="To IFomin-C-RIP-10"

/routing rip interface
add interface=ether1 send=v2 receive=v2
add interface=ether2 send=v2 receive=v2
add interface=ether3 send=v2 receive=v2

/routing rip network
add network=80.28.84.0/30
add network=75.110.44.0/30
add network=186.23.48.0/30
```

### M-RIP-2
```
/system identity set name=IFomin-M-RIP-2

/interface ethernet
set ether1 comment="To IFomin-C-RIP-7"
set ether2 comment="To IFomin-C-RIP-10"
set ether3 comment="To IFomin-C-RIP-5"
set ether4 comment="To IFomin-L-RIP-12"

/ip dhcp-client remove number=0
/ip address
add address=80.28.82.1/30 interface=ether1 disabled=no comment="To IFomin-C-RIP-7"
add address=186.22.48.1/30 interface=ether2 disabled=no comment="To IFomin-C-RIP-10"
add address=93.56.240.2/30 interface=ether3 disabled=no comment="To IFomin-C-RIP-5"
add address=93.46.46.2/30 interface=ether4 disabled=no comment="To IFomin-L-RIP-12"

/routing rip interface
add interface=ether1 send=v2 receive=v2
add interface=ether2 send=v2 receive=v2
add interface=ether3 send=v2 receive=v2
add interface=ether4 send=v2 receive=v2

/routing rip network
add network=80.28.82.0/30
add network=186.22.48.0/30
add network=93.56.240.0/30
add network=93.46.46.0/30
```

## OSPF
### C-OSPF-4
```
ena
conf t
hostname IFomin-C-OSPF-4

int e0/0
Description "To IFomin-C-OSPF-3"
ip address 12.23.10.14 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-С-OSPF-5"
ip address 12.23.10.18 255.255.255.252
no shutdown

router ospf 1
network 12.23.10.16 0.0.0.3 area 0
network 12.23.10.12 0.0.0.3 area 0

exit
exit
wr
```

### C-OSPF-5
```
ena
conf t
hostname IFomin-C-OSPF-5

int e0/0
Description "To IFomin-C-OSPF-4"
ip address 12.23.10.17 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-M-OSPF-5"
ip address 12.22.16.2 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-M-OSPF-4"
ip address 12.21.12.2 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-Extra-Net-2"

router ospf 1
network 12.21.12.0 0.0.0.3 area 0
network 12.23.10.16 0.0.0.3 area 0
network 12.22.16.0 0.0.0.3 area 0

exit
exit
wr
```

### C-OSPF-3
```
ena
conf t
hostname IFomin-C-OSPF-3

int e0/0
Description "To IFomin-C-OSPF-2"
ip address 12.23.10.10 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-OSPF-4"
ip address 12.23.10.13 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-M-OSPF-5"
ip address 12.22.17.2 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-L-OSPF-2"
ip address 12.23.20.1 255.255.255.252
no shutdown

router ospf 1
network 12.23.10.12 0.0.0.3 area 0
network 12.22.17.0 0.0.0.3 area 0
network 12.23.10.8 0.0.0.3 area 0

redistribute connected

exit
exit
wr
```

### M-OSPF-5
```
/system identity set name=IFomin-M-OSPF-5

/interface ethernet
set ether1 comment="To IFomin-M-OSPF-4"
set ether2 comment="To IFomin-C-OSPF-3"
set ether3 comment="To IFomin-C-OSPF-5"
set ether4 comment="To IFomin-C-OSPF-2"

/ip dhcp-client remove number=0
/ip address
add address=12.22.15.2/30 interface=ether1 disabled=no comment="To IFomin-M-OSPF-4"
add address=12.22.17.1/30 interface=ether2 disabled=no comment="To IFomin-C-OSPF-3"
add address=12.22.16.1/30 interface=ether3 disabled=no comment="To IFomin-C-OSPF-5"
add address=12.22.14.2/30 interface=ether4 disabled=no comment="To IFomin-C-OSPF-2"

/interface bridge add name=loopback
/ip address add address=10.255.255.5/32 interface=loopback
/routing ospf 
instance set 0 router-id=10.255.255.5
network add network=12.22.15.0/30 area=backbone
network add network=12.22.16.0/30 area=backbone
network add network=12.22.17.0/30 area=backbone
network add network=12.22.14.0/30 area=backbone
```

### M-OSPF-4
```
/system identity set name=IFomin-M-OSPF-4

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

/interface bridge add name=loopback
/ip address add interface=loopback address=10.255.255.4/32
/routing ospf
instance set 0 router-id=10.255.255.4
network add network=12.21.11.0/30 area=backbone
network add network=12.22.12.0/30 area=backbone
network add network=12.22.15.0/30 area=backbone
network add network=12.21.12.0/30 area=backbone
```

### C-OSPF-2
```
ena
conf t
hostname IFomin-C-OSPF-2

int e0/0
Description "To IFomin-C-OSPF-1"
ip address 12.23.10.6 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-M-OSPF-3"
ip address 12.22.13.2 255.255.255.252
no shutdown

int e0/2
Description "To IFomin-C-OSPF-3"
ip address 12.23.10.9 255.255.255.252
no shutdown

int e0/3
Description "To IFomin-M-OSPF-5"
ip address 12.22.14.1 255.255.255.252
no shutdown

router ospf 1
network 12.23.10.4 0.0.0.3 area 0
network 12.22.13.0 0.0.0.3 area 0
network 12.23.10.8 0.0.0.3 area 0
network 12.22.14.0 0.0.0.3 area 0

exit
exit
wr
```

### M-OSPF-3
```
/system identity set name=IFomin-M-OSPF-3

/interface ethernet
set ether1 comment="To IFomin-M-OSPF-10"
set ether2 comment="To IFomin-M-OSPF-2"
set ether3 comment="To IFomin-M-OSPF-4"
set ether4 comment="To IFomin-C-OSPF-2"

/ip dhcp-client remove number=0
/ip address
add address=12.22.10.2/30 interface=ether1 disabled=no comment="To IFomin-M-OSPF-10"
add address=12.22.11.2/30 interface=ether2 disabled=no comment="To IFomin-M-OSPF-2"
add address=12.22.12.1/30 interface=ether3 disabled=no comment="To IFomin-M-OSPF-4"
add address=12.21.13.1/30 interface=ether4 disabled=no comment="To IFomin-C-OSPF-2"

/interface bridge add name=loopback
/ip address add interface=loopback address=10.255.255.3/32
/routing ospf
instance set 0 router-id=10.255.255.3
network add network=12.22.10.0/30 area=backbone
network add network=12.22.11.0/30 area=backbone
network add network=12.22.12.0/30 area=backbone
network add network=12.22.13.0/30 area=backbone
```

### M-OSPF-2
```
/system identity set name=IFomin-M-OSPF-2

/interface ethernet
set ether1 comment="To IFomin-M-OSPF-10"
set ether2 comment="To IFomin-M-OSPF-3"
set ether3 comment="To IFomin-M-OSPF-4"
set ether4 comment="To IFomin-Linux-PC-OSPF"

/ip dhcp-client remove number=0
/ip address
add address=12.21.10.2/30 interface=ether1 disabled=no comment="To IFomin-M-OSPF-10"
add address=12.22.11.1/30 interface=ether2 disabled=no comment="To IFomin-M-OSPF-3"
add address=12.21.11.1/30 interface=ether3 disabled=no comment="To IFomin-M-OSPF-4"
add address=12.23.21.2/30 interface=ether4 disabled=no comment="To IFomin-Linux-PC-OSPF"

/interface bridge add name=loopback
/ip address add interface=loopback address=10.255.255.2/32
/routing ospf
instance set 0 router-id=10.255.255.2
network add network=12.21.10.0/30 area=backbone
network add network=12.22.11.0/30 area=backbone
network add network=12.21.11.0/30 area=backbone
```

### C-OSPF-1
```
ena
conf t
hostname IFomin-C-OSPF-1

int e0/0
Description "To IFomin-M-OSPF-10"
ip address 12.23.10.2 255.255.255.252
no shutdown

int e0/1
Description "To IFomin-C-OSPF-2"
ip address 12.23.10.5 255.255.255.252
no shutdown

router ospf 1
network 12.23.10.0 0.0.0.3 area 0
network 12.23.10.4 0.0.0.3 area 0

exit
exit
wr
```

### M-OSPF-10
```
/system identity set name=IFomin-M-OSPF-10

/interface ethernet
set ether1 comment="To IFomin-Main-Router"
set ether2 comment="To IFomin-C-RIP-1"
set ether6 comment="To IFomin-C-iBGP"
set ether3 comment="To IFomin-M-OSPF-2"
set ether4 comment="To IFomin-M-OSPF-3"
set ether5 comment="To IFomin-C-OSPF-1"

/ip dhcp-client remove number=0
/ip address 
add interface=ether1 address=14.11.1.2/30 comment="To IFomin-Main-Router"
add interface=ether2 address=18.11.1.2/30 comment="To IFomin-C-RIP-1"
add interface=ether6 address=15.11.1.2/30 comment="To IFomin-C-iBGP-102"
add interface=ether3 address=12.21.10.1/30 comment="To IFomin-M-OSPF-2"
add interface=ether4 address=12.22.10.1/30 comment="To IFomin-M-OSPF-3"
add interface=ether5 address=12.23.10.1/30 comment="To IFomin-C-OSPF-1"

/interface bridge add name=loopback
/ip address add interface=loopback address=10.255.255.10/32
/routing ospf
instance set 0 router-id=10.255.255.10 redistribute-bgp=as-type-1
network add network=12.21.10.0/30 area=backbone
network add network=12.22.10.0/30 area=backbone
network add network=12.23.10.0/30 area=backbone

/routing bgp instance
add name=BGP300 as=300 redistribute-connected=yes redistribute-ospf=yes router-id=10.255.255.10

/routing bgp peer
add instance=BGP300 remote-address=18.11.1.1 remote-as=200
add instance=BGP300 remote-address=14.11.1.1 remote-as=100
add instance=BGP300 remote-address=15.11.1.1 remote-as=400
```

## eBGP
### Main-Router
```
ena
conf t

router bgp 100
neighbor 17.11.1.2 remote-as 200
neighbor 14.11.1.2 remote-as 300
neighbor 16.11.1.2 remote-as 400
redistribute connected
redistribute static

exit 
exit 
write
```

### C-R-1
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 10.27.0.2
do wr
```

### C-R-1-1
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 17.26.1.2
do wr
```

### C-R-1-2
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 17.26.2.2
do wr
```

### C-R-1-3
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 17.26.3.2
do wr
```

### C-R-2
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 10.29.0.2
do wr
```

### C-R-2-1
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 18.30.1.2
do wr
```

### C-R-2-2
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 18.30.2.2
do wr
```

### C-R-2-3
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 18.30.3.2
do wr
```

### C-R-3
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 10.26.0.2
do wr
```

### C-R-3-1
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 16.28.1.2
do wr
```

### C-R-3-2
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 16.28.2.2
do wr
```

### C-R-3-3
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 16.28.3.2
do wr
```

### C-R-4
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 10.30.0.2
do wr
```

### C-R-4-1
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 10.20.10.2
do wr
```

### C-R-4-2
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 30.40.2.2
do wr
```

### C-R-4-3
```
ena
conf t
ip route 0.0.0.0 0.0.0.0 30.40.3.2
do wr
```