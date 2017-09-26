# ACL Configuration Scenario

<img src=images\2017-09-24_21-06-34.png>

1. Use a standard ACL to block the 10.1.1.1 from reaching 10.1.1.6 and 192.168.1.0/24
2. Use a standard ACL to block access to the 192.168.1.0/24 from 192.168.2.0/25
3. Use a standard ACL to block 192.168.2.50 from reaching the 10.1.1.1 wan IP address

*** Filter VTY access


### R1
```
interface FastEthernet0/0
 ip address 10.1.1.6 255.255.255.252
 ip access-group SCENARIO1 in
!

interface FastEthernet1/0
 ip address 192.168.1.1 255.255.255.0
 ip access-group SCENARIO2 out
!
router rip
 version 2
 network 10.0.0.0
 network 192.168.1.0
 no auto-summary
!

ip access-list standard SCENARIO1
 deny   10.1.1.1
 permit any
ip access-list standard SCENARIO2
 deny   192.168.2.0 0.0.0.127
 permit any
! 
```

### R3

```
!
interface FastEthernet0/0
 ip address 10.1.1.1 255.255.255.252
 ip access-group SCENARIO3 in
 duplex auto
 speed auto
!
interface FastEthernet0/1
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address 192.168.2.129 255.255.255.128
 duplex auto
 speed auto
!
router rip
 version 2
 network 10.0.0.0
 network 192.168.2.0
 no auto-summary
!
ip forward-protocol nd
!
!
ip http server
no ip http secure-server
!
ip access-list standard SCENARIO3
 deny   192.168.2.50
 permit any
!
```

### VTY

```
R3(config)#ip access-list standard FILTER_TELNET

R3(config-std-nacl)#permit 10.0.0.0 0.255.255.255  
R3(config-std-nacl)#exit
R3(config)#line vty 0 4

R3(config-line)#access-class ?
  <1-199>      IP access list
  <1300-2699>  IP expanded access list
  WORD         Access-list name

R3(config-line)#access-class FILTER_TELNET ?
  in   Filter incoming connections
  out  Filter outgoing connections

R3(config-line)#access-class FILTER_TELNET in
R3(config-line)#exit
R3(config)#
```
