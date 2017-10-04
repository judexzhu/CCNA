## OSPF Base Configuration / Exploration Lab


<img src=images\2017-09-30_15-24-36.png>

1. Configure all routers shown to operate in the backbone area. Hardcode Router IDs so they do not easily change.

```
Tie#show run | sec ospf
router ospf 1
 router-id 1.1.1.1
 log-adjacency-changes
 network 10.1.1.1 0.0.0.0 area 0

```

2. Determine which router became the DR; elect "Tie" as the DR moving forward.

```
Tie(config)#int f0/1
Tie(config-if)#ip ospf priority 10
```

```
Tie#clear ip ospf process
Reset ALL OSPF processes? [no]: y
```

3. Adjust the metric of OSPF to function well with Speed up to 10G links.

```
Tie(config)#router ospf 1
Tie(config-router)#auto-cost reference-bandwidth 10000
```

4. Ensure "Shoe" does not form OSPF neighbors on it's LAN network.

```
Shoe(config-router)#passive-interface all     
Shoe(config-router)#no passive-interface f0/0
```

```
Shoe#show run | sec osp
router ospf 1
 router-id 4.4.4.4
 log-adjacency-changes
 auto-cost reference-bandwidth 10000
 passive-interface default
 no passive-interface FastEthernet0/0
 network 10.1.2.2 0.0.0.0 area 0
 network 10.1.3.1 0.0.0.0 area 0
Shoe#
```

5. Adjust the Hello timer on the "Sock" WAN interface to send  Hello Message 1/sec.

```
Sock(config)#int f0/1
Sock(config-if)#ip ospf hello-interval 1
```

```
Sock#show ip ospf  interface 
FastEthernet0/1 is up, line protocol is up 
  Internet Address 10.1.2.1/24, Area 0 
  Process ID 1, Router ID 3.3.3.3, Network Type BROADCAST, Cost: 1000
  Transmit Delay is 1 sec, State BDR, Priority 1
  Designated Router (ID) 4.4.4.4, Interface address 10.1.2.2
  Backup Designated router (ID) 3.3.3.3, Interface address 10.1.2.1
  Flush timer for old DR LSA due in 00:02:50
  Timer intervals configured, Hello 1, Dead 4, ....
```

Bonus: Create Loopback interfaces in such a way that Router IDs are pingable from any router.