## R1

```
!
! Last configuration change at 09:48:29 UTC Mon Mar 17 2025
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
## R2

``` 
!
! Last configuration change at 01:17:02 UTC Tue Mar 18 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
!
hostname R2
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
interface FastEthernet0/0
 no ip address
 duplex full
!
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.3.10.253 255.255.255.0
 ip nat inside
 standby 1 ip 10.3.10.254
 standby 1 priority 50
!
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.3.20.253 255.255.255.0
 ip nat inside
 standby 1 ip 10.3.20.254
 standby 1 priority 50
!
interface FastEthernet0/0.30
 encapsulation dot1Q 30
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

## Core1
```

!
! Last configuration change at 15:19:40 UTC Thu Mar 6 2025
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
!
!
!
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
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/3
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
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
!
!
!
end

```
## Core2
```

!
! Last configuration change at 18:16:12 UTC Thu Mar 13 2025
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
!
!
!
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
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/3
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/0
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet1/1
 no shutdown
 switchport access vlan 30
 switchport mode access
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
## Dist1
```

!
! Last configuration change at 14:48:43 UTC Thu Mar 6 2025
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
!
!
!
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
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 20
 switchport mode access
!
interface Ethernet1/0
 no shutdown
 switchport access vlan 30
 switchport mode access
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
!
!
!
end

```
## Dist2
```

!
! Last configuration change at 14:48:44 UTC Thu Mar 6 2025
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
!
!
!
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
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 20
 switchport mode access
!
interface Ethernet1/0
 no shutdown
 switchport access vlan 30
 switchport mode access
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
!
!
!
end

```
## Access1
```

!
! Last configuration change at 15:05:35 UTC Thu Mar 6 2025
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
!
!
!
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
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 20
 switchport mode access
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
!
!
!
end

```
## Access2
```

!
! Last configuration change at 14:12:13 UTC Thu Mar 6 2025
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
!
!
!
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
!
interface Ethernet0/1
 no shutdown
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 20
 switchport mode access
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
!
!
!
end

```
## Access3
```

!
! Last configuration change at 14:12:11 UTC Thu Mar 6 2025
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
!
!
!
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
 switchport access vlan 30
 switchport mode access
!
interface Ethernet0/1
 no shutdown
 switchport access vlan 30
 switchport mode access
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 30
 switchport mode access
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
!
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