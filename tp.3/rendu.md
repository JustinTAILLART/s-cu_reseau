## R1

```

!
! Last configuration change at 19:39:35 UTC Mon Mar 24 2025 by admin
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
!
hostname R1
!
boot-start-marker
boot-end-marker
!
!
!
aaa new-model
!
!
aaa authentication login default local
aaa authorization exec default local 
!
!
!
!
!
aaa session-id common
!
!
!
!
!
!
ip domain name tp4.local
ip cef
no ipv6 cef
!
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
username admin privilege 15 secret 5 $1$tE9A$2.FO6ZcwjRf1FxRm1hymL/
!
!
!
!
!
ip ssh version 2
ip ssh pubkey-chain
  username admin
   key-hash ssh-rsa BF9C3D7B2460B16EE299D6A75960EEB9
! 
!
!
!
!
!
!
!
!
interface FastEthernet0/0
 no ip address
 duplex full
!
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.3.10.252 255.255.255.0
 ip helper-address 10.3.30.251
 ip nat inside
 standby 1 ip 10.3.10.254
 standby 1 preempt
!
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.3.20.252 255.255.255.0
 ip helper-address 10.3.30.251
 ip nat inside
 standby 1 ip 10.3.20.254
 standby 1 preempt
!
interface FastEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.3.30.252 255.255.255.0
 ip nat inside
 standby 1 ip 10.3.30.254
 standby 1 priority 50
!
interface FastEthernet1/0
 ip address 10.99.99.30 255.255.255.0
 ip nat outside
 duplex full
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex full
!
ip nat inside source list 1 interface FastEthernet1/0 overload
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 10.99.99.1
!
access-list 1 permit any
!
!
!
!
control-plane
!
!
line con 0
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
!
!
end
```
## R2

``` 
Current configuration : 1563 bytes                                                                                                                                                                               !                                                                                                                                                                                                                version 15.2                                                                                                                                                                                                     service timestamps debug datetime msec                                                                                                                                                                           service timestamps log datetime msec                                                                                                                                                                             !                                                                                                                                                                                                                hostname R2                                                                                                                                                                                                      !                                                                                                                                                                                                                boot-start-marker                                                                                                                                                                                                boot-end-marker                                                                                                                                                                                                  !                                                                                                                                                                                                                !                                                                                                                                                                                                                !
aaa new-model
!
!
aaa authentication login default local
aaa authorization exec default local
!
!
!
!
!
aaa session-id common
!
!
!
!
!
!
ip domain name tp4.local
ip cef
no ipv6 cef
!
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
username admin privilege 15 secret 5 $1$ofFJ$xODIrdrXylqeJQiIsFe9f.
!
!
!
!
!
ip ssh version 2
ip ssh pubkey-chain
  username admin
   key-hash ssh-rsa BF9C3D7B2460B16EE299D6A75960EEB9
!
!
!
!
!
!
!
!
!
interface FastEthernet0/0
 no ip address
 duplex full
!
interface FastEthernet0/0.10                                                                                                                                                                                      encapsulation dot1Q 10                                                                                                                                                                                           ip address 10.3.10.253 255.255.255.0                                                                                                                                                                             ip nat inside                                                                                                                                                                                                    standby 1 ip 10.3.10.254                                                                                                                                                                                         standby 1 priority 50                                                                                                                                                                                           !                                                                                                                                                                                                                interface FastEthernet0/0.20                                                                                                                                                                                      encapsulation dot1Q 20                                                                                                                                                                                           ip address 10.3.20.253 255.255.255.0                                                                                                                                                                             ip nat inside                                                                                                                                                                                                    standby 1 ip 10.3.20.254                                                                                                                                                                                         standby 1 priority 50                                                                                                                                                                                           !                                                                                                                                                                                                                interface FastEthernet0/0.30                                                                                                                                                                                      encapsulation dot1Q 30
 ip address 10.3.30.253 255.255.255.0
 ip nat inside
 standby 1 ip 10.3.30.254
 standby 1 preempt
!
interface FastEthernet1/0
 ip address 10.88.88.30 255.255.255.0
 ip nat outside
 duplex full
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex full
!
ip nat inside source list 1 interface FastEthernet1/0 overload
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 10.88.88.1
!
access-list 1 permit any
!
control-plane
!
!
line con 0
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
!
!
end
```

## Core1
```
!
! Last configuration change at 11:33:25 UTC Fri Mar 28 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW-CORE-1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac ip 
!
!
!
ip dhcp snooping vlan 10,20,30
ip dhcp snooping
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Port-channel1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 channel-group 1 mode active
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 channel-group 1 mode active
 ip dhcp snooping trust
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/3
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/1
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/2
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/3
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
ip forward-protocol nd
!
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
!
!
end


```
## Core2
```
!
! Last configuration change at 11:33:23 UTC Fri Mar 28 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW-CORE-2
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac ip 
!
!
!
ip dhcp snooping vlan 10,20,30
ip dhcp snooping
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Port-channel1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 channel-group 1 mode active
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 channel-group 1 mode active
 ip dhcp snooping trust
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/3
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/1
 no shutdown
 switchport access vlan 30
 switchport mode access
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/2
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/3
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
ip forward-protocol nd
!
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
!
!
end

```
## Dist1
```
!
! Last configuration change at 11:33:22 UTC Fri Mar 28 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW-DIST-1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac ip 
!
!
!
ip dhcp snooping vlan 10,20,30
ip dhcp snooping
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 20
 switchport mode access
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/1
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/2
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/3
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
ip forward-protocol nd
!
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
!
!
end
```
## Dist2
```
!
! Last configuration change at 11:33:20 UTC Fri Mar 28 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW-DIST-2
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac ip 
!
!
!
ip dhcp snooping vlan 10,20,30
ip dhcp snooping
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 20
 switchport mode access
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/1
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/2
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/3
 no shutdown
 ip arp inspection trust
 ip dhcp snooping trust
!
ip forward-protocol nd
!
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
!
!
end
```
## Access1
```
!
! Last configuration change at 11:33:19 UTC Fri Mar 28 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW-ACC-1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac ip 
!
!
!
ip dhcp snooping vlan 10,20,30
ip dhcp snooping
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 10
 switchport mode access
 spanning-tree bpduguard enable
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 20
 switchport mode access
 spanning-tree bpduguard enable
!
interface Ethernet1/0
 no shutdown
!
interface Ethernet1/1
 no shutdown
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
ip forward-protocol nd
!
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
!
!
end

```

## Access2
```
!
! Last configuration change at 11:33:17 UTC Fri Mar 28 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW-ACC-2
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac ip 
!
!
!
ip dhcp snooping vlan 10,20,30
ip dhcp snooping
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 no shutdown
 switchport access vlan 20
 switchport mode access
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport access vlan 20
 switchport mode access
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 20
 switchport mode access
 spanning-tree bpduguard enable
!
interface Ethernet0/3
 no shutdown
!
interface Ethernet1/0
 no shutdown
!
interface Ethernet1/1
 no shutdown
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
ip forward-protocol nd
!
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
!
!
end
```
## Access3
```

!
! Last configuration change at 11:33:16 UTC Fri Mar 28 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW-ACC-3
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac ip 
!
!
!
ip dhcp snooping vlan 10,20,30
ip dhcp snooping
ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 30
 switchport mode access
 spanning-tree bpduguard enable
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 30
 switchport mode access
!
interface Ethernet1/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust
 ip dhcp snooping trust
!
interface Ethernet1/1
 no shutdown
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
ip forward-protocol nd
!
ip http server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
!
!
end
```
## DHCP

```
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
!
hostname DHCP
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
ip dhcp excluded-address 10.3.10.254
ip dhcp excluded-address 10.3.20.254
!
ip dhcp pool VLAN10
 network 10.3.10.0 255.255.255.0
 default-router 10.3.10.254 
 dns-server 8.8.8.8 1.1.1.1 
!
ip dhcp pool VLAN20
 network 10.3.20.0 255.255.255.0
 default-router 10.3.20.254 
 dns-server 8.8.8.8 1.1.1.1 
!
!
!
ip cef
no ipv6 cef
!
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
!
!
!
! 
!
!
!
!
!
!
!
!
interface FastEthernet0/0
 no ip address
 duplex full
!
interface FastEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.3.30.251 255.255.255.0
!
interface FastEthernet1/0
 no ip address
 shutdown
 duplex full
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex full
!
ip default-gateway 10.3.30.254
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 10.3.30.254
!
!
!
!
control-plane
!
!
line con 0
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 login
!
!
end
```

## VPC2
```
VPC-2> sh ip

NAME        : VPC-2[1]
IP/MASK     : 10.3.20.1/24
GATEWAY     : 10.3.20.254
DNS         : 8.8.8.8  1.1.1.1
DHCP SERVER : 10.3.20.252
DHCP LEASE  : 82501, 86400/43200/75600
MAC         : 00:50:79:66:68:0c
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500

VPC-2> ping 10.3.30.11

84 bytes from 10.3.30.11 icmp_seq=1 ttl=63 time=34.366 ms
84 bytes from 10.3.30.11 icmp_seq=2 ttl=63 time=35.888 ms
84 bytes from 10.3.30.11 icmp_seq=3 ttl=63 time=34.003 ms
84 bytes from 10.3.30.11 icmp_seq=4 ttl=63 time=35.230 ms
^C
VPC-2> ping ynov.com
ynov.com resolved to 104.26.10.233

84 bytes from 104.26.10.233 icmp_seq=1 ttl=126 time=50.219 ms
84 bytes from 104.26.10.233 icmp_seq=2 ttl=126 time=46.514 ms
84 bytes from 104.26.10.233 icmp_seq=3 ttl=126 time=47.507 ms
84 bytes from 104.26.10.233 icmp_seq=4 ttl=126 time=47.777 ms
```

## VPC4

```
VPC-4> sh ip

NAME        : VPC-4[1]
IP/MASK     : 10.3.10.1/24
GATEWAY     : 10.3.10.254
DNS         : 8.8.8.8  1.1.1.1
DHCP SERVER : 10.3.10.252
DHCP LEASE  : 0, 86400/43200/75600
MAC         : 00:50:79:66:68:0a
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500

VPC-4> ping 10.3.30.11

84 bytes from 10.3.30.11 icmp_seq=1 ttl=63 time=62.170 ms
84 bytes from 10.3.30.11 icmp_seq=2 ttl=63 time=32.636 ms
84 bytes from 10.3.30.11 icmp_seq=3 ttl=63 time=33.631 ms
^C
VPC-4> ping ynov.com
ynov.com resolved to 104.26.10.233

84 bytes from 104.26.10.233 icmp_seq=1 ttl=126 time=46.527 ms
84 bytes from 104.26.10.233 icmp_seq=2 ttl=126 time=48.616 ms
84 bytes from 104.26.10.233 icmp_seq=3 ttl=126 time=48.073 ms
84 bytes from 104.26.10.233 icmp_seq=4 ttl=126 time=46.787 ms
^C
```