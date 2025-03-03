TP1 

PARTIE 1 : 

1. Setup IP

🌞 Définir une adresse IP statique sur chaque machine

[admin@node1 ~]$ cat /etc/sysconfig/network-scripts/ifcfg-ens160

DEVICE=ens160
NAME=lan

ONBOOT=yes

IPADDR=10.1.1.11
NETMASK=255.255.255.0
GATEWAY=10.1.1.2
DNS1=8.8.8.8

```----------------------------------------------------------```

[admin@node2 ~]$ cat /etc/sysconfig/network-scripts/ifcfg-ens160
DEVICE=ens160
NAME=lan

ONBOOT=yes

IPADDR=10.1.1.12
NETMASK=255.255.255.0
GATEWAY=10.1.1.2
DNS1=8.8.8.8

```----------------------------------------------------------```
[admin@dhcp ~]$ cat /etc/sysconfig/network-scripts/ifcfg-ens160
DEVICE=ens160
NAME=lan

ONBOOT=yes

IPADDR=10.1.1.253
NETMASK=255.255.255.0
GATEWAY=10.1.1.2
DNS1=8.8.8.8

🌞 Ping !

[admin@node1 ~]$ ping 10.1.1.12
PING 10.1.1.12 (10.1.1.12) 56(84) bytes of data.
64 bytes from 10.1.1.12: icmp_seq=1 ttl=64 time=0.726 ms
64 bytes from 10.1.1.12: icmp_seq=2 ttl=64 time=0.409 ms
64 bytes from 10.1.1.12: icmp_seq=3 ttl=64 time=0.681 ms
64 bytes from 10.1.1.12: icmp_seq=4 ttl=64 time=0.643 ms
^C
--- 10.1.1.12 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3093ms
rtt min/avg/max/mdev = 0.409/0.614/0.726/0.122 ms

🦈 Capture ICMP

[ping_1.pcap](ping_1.pcap)

🌞 Nommez les machines

[admin@node1 ~]$ sudo hostnamectl set-hostname node1.tp1.my

```----------------------------------------------------------```

[admin@node1 ~]$ sudo hostnamectl set-hostname node2.tp1.my

```----------------------------------------------------------```

[admin@node1 ~]$ sudo hostnamectl set-hostname dhcp.tp1.my

🌞 Fermer tous les ports inutilement ouverts dans le firewall

[admin@node1 ~]$ sudo firewall-cmd --permanent --remove-service dhcpv6-client
sudo firewall-cmd --permanent --remove-service cockpit
sudo firewall-cmd --reload

Success
Success
Success

(pareil sur les trois machines)


🌞 Remplir le fichier hosts

127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.1.1.12 node2.tp1.my node2
10.1.1.253 dhcp.tp1.my dhcp

```----------------------------------------------------------```

[admin@node1 ~]$  ping node2
PING node2.tp1.my (10.1.1.12) 56(84) bytes of data.
64 bytes from node2.tp1.my (10.1.1.12): icmp_seq=1 ttl=64 time=0.825 ms
64 bytes from node2.tp1.my (10.1.1.12): icmp_seq=2 ttl=64 time=1.19 ms

--- node2.tp1.m ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 3029ms
rtt min/avg/max/mdev = 0.506/0.787/1.192/0.259 ms

```----------------------------------------------------------```

[admin@node1 ~]$ ping node2.tp1.my
PING node2.tp1.my (10.1.1.12) 56(84) bytes of data.
64 bytes from node2.tp1.my (10.1.1.12): icmp_seq=1 ttl=64 time=0.496 ms
64 bytes from node2.tp1.my (10.1.1.12): icmp_seq=2 ttl=64 time=0.970 ms

--- node2.tp1.m ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 3029ms
rtt min/avg/max/mdev = 0.506/0.787/1.192/0.259 ms

2. ARP

🌞 Table ARP

[admin@node1 ~]$ ip neigh show
10.1.1.2 dev ens160 lladdr 00:50:56:e0:53:ce STALE
10.1.1.1 dev ens160 lladdr 00:50:56:c0:00:01 REACHABLE
10.1.1.12 dev ens160 lladdr 00:0c:29:78:02:a4 STALE
10.1.1.253 dev ens160 lladdr 00:0c:29:f4:b0:c7 STALE

```----------------------------------------------------------```

[admin@node2 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:78:02:a4 brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    inet 10.1.1.12/24 brd 10.1.1.255 scope global noprefixroute ens160
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe78:2a4/64 scope link
       valid_lft forever preferred_lft forever

```----------------------------------------------------------```

[admin@dhcp ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:f4:b0:c7 brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    inet 10.1.1.253/24 brd 10.1.1.255 scope global noprefixroute ens160
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fef4:b0c7/64 scope link


🌞 Manipuler la table ARP

[admin@node1 ~]$ ip neigh show
10.1.1.2 dev ens160 lladdr 00:50:56:e0:53:ce STALE
10.1.1.1 dev ens160 lladdr 00:50:56:c0:00:01 REACHABLE
10.1.1.12 dev ens160 lladdr 00:0c:29:78:02:a4 STALE
10.1.1.153 dev ens160 FAILED
10.1.1.253 dev ens160 lladdr 00:0c:29:f4:b0:c7 STALE
[admin@node1 ~]$ sudo ip neigh flush all
[admin@node1 ~]$ ip neigh show
10.1.1.2 dev ens160 lladdr 00:50:56:e0:53:ce STALE
10.1.1.1 dev ens160 lladdr 00:50:56:c0:00:01 REACHABLE
[admin@node1 ~]$ ping node2
PING node2.tp1.my (10.1.1.12) 56(84) bytes of data.
64 bytes from node2.tp1.my (10.1.1.12): icmp_seq=1 ttl=64 time=1.18 ms
64 bytes from node2.tp1.my (10.1.1.12): icmp_seq=2 ttl=64 time=0.938 ms
64 bytes from node2.tp1.my (10.1.1.12): icmp_seq=3 ttl=64 time=0.961 ms
^C
--- node2.tp1.my ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 0.938/1.026/1.179/0.108 ms
[admin@node1 ~]$ ip neigh show
10.1.1.2 dev ens160 lladdr 00:50:56:e0:53:ce REACHABLE
10.1.1.1 dev ens160 lladdr 00:50:56:c0:00:01 REACHABLE
10.1.1.12 dev ens160 lladdr 00:0c:29:78:02:a4 REACHABLE
[admin@node1 ~]$

🦈 Capture ARP

[arp_1.pcap](arp_1.pcap)

PARTIE 2 

1. Install and conf

🌞 Installer un service DHCP sur la machine dhcp.tp1.my

[admin@dhcp ~]$ sudo nano /etc/dhcp/dhcpd.conf

# specify domain name
option domain-name "tp1.my";
# specify DNS server's hostname or IP address
option domain-name-servers 1.1.1.1;
# default lease time
default-lease-time 86400;
# max lease time
max-lease-time 172800;
# specify network address and subnetmask
subnet 10.1.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.1.1.100 10.1.1.200;
    # specify broadcast address
    option broadcast-address 10.1.1.255;
}

[admin@dhcp ~]$ sudo firewall-cmd --add-service=dhcp
success
[admin@dhcp ~]$ sudo firewall-cmd --runtime-to-permanent
success
[admin@dhcp ~]$

2. Ask an address

🌞 Demander une adresse IP depuis node1.tp1.my

[admin@node1 ~]$ sudo cat /etc/sysconfig/network-scripts/ifcfg-ens160
DEVICE=ens160
NAME=lan

ONBOOT=yes

BOOTPROTO=dhcp

[admin@node1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:3d:1f:1b brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    inet 10.1.1.100/24 brd 10.1.1.255 scope global dynamic noprefixroute ens160
       valid_lft 1761sec preferred_lft 1761sec
    inet6 fe80::20c:29ff:fe3d:1f1b/64 scope link
       valid_lft forever preferred_lft forever

[admin@node1 ~]$ sudo cat /etc/resolv.conf
; generated by /usr/sbin/dhclient-script
search tp1.my
nameserver 1.1.1.1

🦈 Capture DHCP

[dhcp_1.pcap](dhcp_1.pcap)

🌞 Afficher le bail DHCP serveur

[admin@dhcp dhcpd]$ sudo grep -A 10 "10.1.1.100" /var/lib/dhcpd/dhcpd.leases
lease 10.1.1.100 {
  starts 4 2025/02/13 11:27:21;
  ends 5 2025/02/14 11:27:21;
  cltt 4 2025/02/13 11:27:21;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 00:0c:29:3d:1f:1b;
}
lease 10.1.1.101 {
  starts 4 2025/02/13 11:30:01;

PARTIE 3 

1. Network scan

🌞 Effectuer un scan ARP depuis la machine attaquante node1.tp1.my

[admin@node1 ~]$ sudo nmap -sn -PR 10.1.1.0/24
Starting Nmap 7.92 ( https://nmap.org ) at 2025-02-13 07:09 EST
Nmap scan report for 10.1.1.1
Host is up (0.00060s latency).
MAC Address: 00:50:56:C0:00:01 (VMware)
Nmap scan report for 10.1.1.2
Host is up (0.00017s latency).
MAC Address: 00:50:56:E0:53:CE (VMware)
Nmap scan report for 10.1.1.101
Host is up (0.00023s latency).
MAC Address: 00:0C:29:78:02:A4 (VMware)
Nmap scan report for dhcp.tp1.my (10.1.1.253)
Host is up (0.00038s latency).
MAC Address: 00:0C:29:F4:B0:C7 (VMware)
Nmap scan report for 10.1.1.100
Host is up.
Nmap done: 256 IP addresses (5 hosts up) scanned in 28.25 seconds


🦈 Capture ARP Scan

[dhcp_1.pcap](dhcp_1.pcap)

🌞 Effectuer un scan de service et d'OS depuis la machine attaquante node1.tp1.my

[admin@node1 ~]$ sudo nmap -sS -sV -O -p 22,67,68 dhcp.tp1.my
Starting Nmap 7.92 ( https://nmap.org ) at 2025-02-13 08:16 EST
Nmap scan report for dhcp.tp1.my (10.1.1.253)
Host is up (0.00089s latency).

PORT   STATE    SERVICE VERSION
22/tcp open     ssh     OpenSSH 8.7 (protocol 2.0)
67/tcp filtered dhcps
68/tcp filtered dhcpc
MAC Address: 00:0C:29:F4:B0:C7 (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (94%), Linux 3.10 - 4.11 (94%), Linux 3.2 - 4.9 (94%), Linux 3.4 - 3.10 (94%), Linux 4.15 - 5.6 (94%), Linux 5.4 (94%), Linux 2.6.32 - 3.10 (93%), Linux 2.6.32 - 3.13 (93%), Linux 3.10 (93%), Linux 5.1 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 5.80 seconds

🦈 Capture Service Scan

[nmap_2.pcap](nmap_2.pcap)


PARTIE 4 

1. Simple ARP spoof

🌞 Empoisonnez la table ARP de node2.tp1.my

[admin@node1 ~]$ sudo arpspoof -i ens160 -t 10.1.1.101 10.1.1.253
0:c:29:3d:1f:1b 0:c:29:78:2:a4 0806 42: arp reply 10.1.1.253 is-at 0:c:29:3d:1f:1b
0:c:29:3d:1f:1b 0:c:29:78:2:a4 0806 42: arp reply 10.1.1.253 is-at 0:c:29:3d:1f:1b
0:c:29:3d:1f:1b 0:c:29:78:2:a4 0806 42: arp reply 10.1.1.253 is-at 0:c:29:3d:1f:1b
^CCleaning up and re-arping targets...
0:c:29:3d:1f:1b 0:c:29:78:2:a4 0806 42: arp reply 10.1.1.253 is-at 0:c:29:f4:b0:c7
0:c:29:3d:1f:1b 0:c:29:78:2:a4 0806 42: arp reply 10.1.1.253 is-at 0:c:29:f4:b0:c7
0:c:29:3d:1f:1b 0:c:29:78:2:a4 0806 42: arp reply 10.1.1.253 is-at 0:c:29:f4:b0:c7
0:c:29:3d:1f:1b 0:c:29:78:2:a4 0806 42: arp reply 10.1.1.253 is-at 0:c:29:f4:b0:c7
0:c:29:3d:1f:1b 0:c:29:78:2:a4 0806 42: arp reply 10.1.1.253 is-at 0:c:29:f4:b0:c7https://github.com/JustinTAILLART/s-cu_reseau.git

[admin@node2 ~]$ ip n s
10.1.1.100 dev ens160 lladdr 00:0c:29:3d:1f:1b STALE
10.1.1.1 dev ens160 lladdr 00:50:56:c0:00:01 REACHABLE
10.1.1.253 dev ens160 lladdr 00:0c:29:3d:1f:1b REACHABLE

🦈 Capture ARP Spoof

[arp_spoof_1.pcap](arp_spoof_1.pcap)

2. Avec Scapy

🌞 Ecrire un script Scapy qui fait le travail de arpspoof

🦈 Capture ARP Spoof Scapy

🌞 Mettre en place un MITM ARP

🦈 Capture MITM ARP