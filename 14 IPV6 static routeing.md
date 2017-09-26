## IPV6 

<img src=images\2017-09-26_10-33-38.png>

1. Assign IP addresses and connect devices as shown.
2. Test by pinging between the two devices
3. Configure a static route allowing the two LAN connections to reach each other.
4. Test using sourced from each LAN interface of the routers


R1

```
ipv6 unicast-routing

interface Loopback0
 ipv6 address 2001:55::1/64
!
interface FastEthernet0/0
 ipv6 address 2001:210:10:1::1/64
!
ipv6 route 2001:56::/64 2001:210:10:1::2
!
```

R2

```
ipv6 unicast-routing

interface Loopback0
 ipv6 address 2001:56::1/64
!
interface FastEthernet0/0
 ipv6 address 2001:210:10:1::2/64
!
ipv6 route 2001:55::/64 2001:210:10:1::1
!
```

```
R1#ping ipv6 2001:56::1 source 2001:55::1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:56::1, timeout is 2 seconds:
Packet sent with a source address of 2001:55::1
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/16/20 ms
```
```
R1#show ipv6 route    
IPv6 Routing Table - 6 entries
Codes: C - Connected, L - Local, S - Static, R - RIP, B - BGP
       U - Per-user Static route, M - MIPv6
       I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary
       O - OSPF intra, OI - OSPF inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2
       D - EIGRP, EX - EIGRP external
C   2001:55::/64 [0/0]
     via ::, Loopback0
L   2001:55::1/128 [0/0]
     via ::, Loopback0
S   2001:56::/64 [1/0]
     via 2001:210:10:1::2
C   2001:210:10:1::/64 [0/0]
     via ::, FastEthernet0/0
L   2001:210:10:1::1/128 [0/0]
     via ::, FastEthernet0/0
L   FF00::/8 [0/0]
     via ::, Null0
```

