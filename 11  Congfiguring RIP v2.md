## RIP v2

<img src=images\2017-09-24_17-35-50.png>

1. Assign IP addresses and connect devices as show. Ensure there are no static routes on R1 and R2.
2. Test pings between the devices on the two networks. What results do you see ?
3. Configure RIPv2 to populate the routing table on both routers. Verify this is successful.
4. Ping between the two devices again. What results do you see ?
5. Bonus: Add a secondary interface/network between R1 and R2. Add it to the RIP routing process; how does it affect the routing table?

```
interface FastEthernet0/0
 ip address 10.5.1.1 255.255.255.0
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.5.2.1 255.255.255.0
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address 192.168.0.1 255.255.255.0
 duplex auto
 speed auto
!
router rip
 version 2
 network 10.0.0.0
 network 192.168.0.0
 no auto-summary
!
```

```
interface FastEthernet0/0
 ip address 10.5.3.1 255.255.255.0
 duplex auto
 speed auto
!
interface FastEthernet0/1
 ip address 10.5.2.2 255.255.255.0
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address 192.168.0.2 255.255.255.0
 duplex auto
 speed auto
!
router rip
 version 2
 network 10.0.0.0
 network 192.168.0.0
 no auto-summary
```

```
S1#show ip route 
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/24 is subnetted, 3 subnets
R       10.5.3.0 [120/1] via 192.168.0.2, 00:00:23, FastEthernet1/0
                 [120/1] via 10.5.2.2, 00:00:11, FastEthernet0/1
C       10.5.2.0 is directly connected, FastEthernet0/1
C       10.5.1.0 is directly connected, FastEthernet0/0
C    192.168.0.0/24 is directly connected, FastEthernet1/0

```

```
S2#show ip route 
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/24 is subnetted, 3 subnets
C       10.5.3.0 is directly connected, FastEthernet0/0
C       10.5.2.0 is directly connected, FastEthernet0/1
R       10.5.1.0 [120/1] via 192.168.0.1, 00:00:23, FastEthernet1/0
                 [120/1] via 10.5.2.1, 00:00:26, FastEthernet0/1
C    192.168.0.0/24 is directly connected, FastEthernet1/0
```