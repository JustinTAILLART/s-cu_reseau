## CONF SWITCH ACCESS 1 :

Last configuration change at 15:07:25 UTC Wed Mar 5 2025


version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config


hostname accesss1


boot-start-marker
boot-end-marker


no aaa new-model



ip arp inspection validate src-mac 
ip arp inspection filter access1 vlan  10,20


ip cef
no ipv6 cef


spanning-tree mode pvst
spanning-tree extend system-id


interface Ethernet0/0
 no shutdown
 switchport access vlan 10
 switchport mode access


interface Ethernet0/1
 no shutdown
 switchport access vlan 20
 switchport mode access


interface Ethernet0/2
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust


interface Ethernet0/3
 no shutdown


interface Ethernet1/0
 no shutdown


interface Ethernet1/1
 no shutdown


interface Ethernet1/2
 no shutdown


interface Ethernet1/3
 no shutdown


ip forward-protocol nd


ip http server


ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr


arp access-list access1
 permit ip host 10.2.10.1 mac host 0050.7966.6802 
 permit ip host 10.2.10.2 mac host 0050.7966.6806 
 permit ip host 10.2.20.1 mac host 0050.7966.6807 
 permit ip host 10.2.20.2 mac host 0050.7966.6808 
 permit ip host 10.2.30.1 mac host 0050.7966.6809 


control-plane


line con 0
 logging synchronous
line aux 0
line vty 0 4
 login


end

## CONF SWITCH ACCESS 2 :

Last configuration change at 10:29:41 UTC Thu Mar 6 2025


version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config


hostname access2


boot-start-marker
boot-end-marker


no aaa new-model


ip arp inspection vlan 10,20,30
ip arp inspection validate src-mac 
ip arp inspection filter access2 vlan  10,20,30


ip cef
no ipv6 cef


spanning-tree mode pvst
spanning-tree extend system-id


interface Ethernet0/0
 no shutdown
 switchport access vlan 10
 switchport mode access
 spanning-tree bpduguard enable


interface Ethernet0/1
 no shutdown
 switchport access vlan 20
 switchport mode access
 spanning-tree bpduguard enable


interface Ethernet0/2
 no shutdown
 switchport access vlan 30
 switchport mode access
 spanning-tree bpduguard enable


interface Ethernet0/3
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk
 ip arp inspection trust


interface Ethernet1/0
 no shutdown


interface Ethernet1/1
 no shutdown


interface Ethernet1/2
 no shutdown


interface Ethernet1/3
 no shutdown


ip forward-protocol nd


ip http server


ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr


arp access-list access2
 permit ip host 10.2.10.1 mac host 0050.7966.6802 
 permit ip host 10.2.10.2 mac host 0050.7966.6806 
 permit ip host 10.2.20.1 mac host 0050.7966.6807 
 permit ip host 10.2.20.2 mac host 0050.7966.6808 
 permit ip host 10.2.30.1 mac host 0050.7966.6809 


control-plane


line con 0
 logging synchronous
line aux 0
line vty 0 4
 login


end

## CONF SWITCH CORE 1 :

Last configuration change at 09:22:22 UTC Thu Mar 6 2025


version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config


hostname core1


boot-start-marker
boot-end-marker


no aaa new-model


ip cef
no ipv6 cef


spanning-tree mode pvst
spanning-tree extend system-id


interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 10,20
 switchport trunk encapsulation dot1q
 switchport mode trunk


interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk


interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport mode trunk


end

## CONF SWITCH R1 :

Last configuration change at 10:08:13 UTC Thu Mar 6 2025


version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec


hostname R1


boot-start-marker
boot-end-marker


no aaa new-model


ip cef
no ipv6 cef


multilink bundle-name authenticated


interface FastEthernet0/0
 no ip address
 ip nat inside
 duplex full


interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.2.10.254 255.255.255.0
 ip nat inside


interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.2.20.254 255.255.255.0
 ip nat inside


interface FastEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.2.30.254 255.255.255.0
 ip access-group 2 out
 ip nat inside


interface FastEthernet1/0
 ip address 10.99.99.10 255.255.255.0
 ip nat outside
 duplex full


ip nat inside source list 1 interface FastEthernet1/0 overload
ip forward-protocol nd


no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 10.99.99.1


access-list 1 permit any
access-list 2 deny   10.2.30.0 0.0.0.255


control-plane


line con 0
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 login


end
