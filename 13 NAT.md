## Network Address Translation

<img src=images\2017-09-25_11-20-16.png>

1. Assign IP addresses and connect devices as shown. The computers can be configured via DHCP or statically.
2. Configure a default route to the internet- The ISP router is (10.1.51.100). Test connectivity to the internet from the router using ping.
3. Test connectivity from the computers to the Internet. Are they working? Why or why not?
4. Configure NAT overload to the F0/1 interface IP address. Verify NAT is functioning using relevant show commands 
5. Bouns: Convert the NAT configuration to an overloaded NAT pool using the range (10.1.51.100-10.1.51.109)

```
ip dhcp excluded-address 192.168.1.1 192.168.1.49
!
ip dhcp pool NAT_LAB
   network 192.168.1.0 255.255.255.0
   domain-name buyabs.corp
   dns-server 4.2.2.2 8.8.8.8 
   default-router 192.168.1.1 
!
```

```
R1#show ip route 
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route

Gateway of last resort is 10.1.51.1 to network 0.0.0.0

     10.0.0.0/25 is subnetted, 1 subnets
C       10.1.51.0 is directly connected, FastEthernet0/1
C    192.168.1.0/24 is directly connected, FastEthernet0/0
S*   0.0.0.0/0 [1/0] via 10.1.51.1

```

```
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.1.51.100 255.255.255.128
 ip nat outside
 ip virtual-reassembly
 duplex auto
 speed auto
!
ip forward-protocol nd
ip route 0.0.0.0 0.0.0.0 10.1.51.1
!
!
ip http server
no ip http secure-server
ip nat inside source list NAT_LAB interface FastEthernet0/1 overload
!
ip access-list standard NAT_LAB
 permit 192.168.1.0 0.0.0.255
!

```

## Bonus:

```
ip nat pool NAT_LAB 10.1.51.100 10.1.51.109 prefix-length 25
ip nat inside source list NAT_LAB pool NAT_LAB overload
!
ip access-list standard NAT_LAB
 permit 192.168.1.0 0.0.0.255
!
```

## Staic

1. Assign IP addresses and connect devices as shown. The computers can be configured via DHCP or statically.
2. Configure static NAT to translate all traffic from 192.168.1.50 to the IP address 10.1.51.100. Verify this is working correctly.
3. Configure static NAT to translate all web traffic (HTTP/HTTPs) to the new wbe server ate 192.168.1.51. This traffic will come in on the IP address 200.1.1.11. Only translate these specific services.
4. Verify all NAT configurations are implemented correctly. 

```
R1(config)#ip nat inside source static 192.168.1.50 10.1.51.100
R1(config)#ip nat inside source static 192.168.1.51 10.1.51.101
```

```
R1#show ip nat translations 
Pro Inside global      Inside local       Outside local      Outside global
--- 10.1.51.100        192.168.1.50       ---                --- 
--- 10.1.51.101        192.168.1.51       ---                ---
```

```
R1(config)#ip nat  inside source static tcp 192.168.1.51 80 10.1.51.101 80 
R1(config)#ip nat  inside source static tcp 192.168.1.51 443 10.1.51.101 443 

R1#show ip nat translations 
Pro Inside global      Inside local       Outside local      Outside global
--- 10.1.51.100        192.168.1.50       ---                ---
tcp 10.1.51.101:80     192.168.1.51:80    ---                ---
tcp 10.1.51.101:443    192.168.1.51:443   ---                ---
R1#
```