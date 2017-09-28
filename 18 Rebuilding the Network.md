##  ICND1 Review – Rebuilding the Network

## Scenario:

<img src=images\2017-09-26_19-16-35.png>

Your organization has just upgraded from using non-standardized devices to a complete Cisco solution.
You’ve been offered a bonus of 3,281 Bitcoins to implement the new network infrastructure meeting the standards outlined below.

## Objectives:

- Configure all routers and switches with a base configuration. This base configuration should include the following:

    * Correct Hostname
    * Telnet and consol password of     **Cisco **
    * Synchronous console logging
    * Prevent the console port form having an idle timer 
    * Encrypted enable password **cisco**
    * Prevent mistyped commands in privileged mode from hanging fro 30 seconds
    * A logon banner threatening unauthorized users of dire consequences

```
hostname R2

line vty 0 4
 password Cisco
 login

line con 0
 exec-timeout 0 0
 password Cisco
 logging synchronous
 login
 exit

enable secret cisco
no ip domain lookup

banner motd $
###############################################################
#                 Authorized access only!                     # 
# Disconnect IMMEDIATELY if you are not an authorized user!!! #
#         All actions Will be monitored and recorded          #
###############################################################
$

end
wr
```

- Configure IP addressing appropriately on all router and switches based on the diagram shown.

- Hardcore the speed and duplex between all routers and switches in the corporate office.

```
speed 100
duplex full
```

- Configure basic RIPv2 routing between all routers. YOu should be able to ping/ telnet to any device shown in the network diagram from any switch or router.

    * disable RIPv3 automatic summarization on all routers
    * Ensure you do not run RIPv2 on the R1 interface connected to the ISP
    * Ensure RIP advertisement are never sent from R3 to any device on the Branch Office Lan.
    * Switches should use R1 as their default gateway.

- Configure a static default route on R1 pointing to the ISP. Configure a static default route on R2 pointing to R1 and a static default route on R3 point to R2. Bonus Bitcoins if you can find a way to do this using RIP for R2 and R3 rather than the static default routes.

```
R1(config)#router rip
R1(config-router)#default-information originate 
```

- Manually configure trunk ports between S1, S2 and S3.

- Add the following VLANs to S1, S2 and S2. Ensure these VLANs (and the default VLAN) are the only VLANs allowed to pass between all switches. You can optionally use VTP to save some configuration time.

    * VLAN2 - IT(10.24.2.0/24)
    * VLAN5 - Accounting(10.24.5.0/24)

>Note:     
The EtherSwitch module used for switch emulation in GNS3 use the vlan database mode for VLAN configuration .

- configure R1 to perform routing for VLAN 2 and 5 (router on a stick). For each sub-interface, R1 should use the first valid IP address from each VLAN.

- Configure R1 as a DHCP server for VLAN2 and 5. The DHCP server should distribute client IP addresses between .100 and .150 for each subnet, a DNS server of 4.2.2.2 (With a secondary 8.8.8.8), and the appropriate default gateway.

- Assign PC A to the IT Vlan and PC B to the accounting vlan. Verify (Using show ip interface brief) that they receive an IP address via DHCP. Verify routing by pinging from PC A to PC B.

- Configure NAT on R1 in such a way that all users of the corporate office can access the Internet by sharing the public IP address assigned to R1. 

>NOTE:  Only valid ip address from the corporate office should be permitted to use NAT; R1 should NAT only IP subnets shown in the network diagram.

Verify NAT is working correctly by pinging 4.2.2.2 or 8.8.8.8 from PC A or PC B 

>NOTE: If building this lab on your won, you will need to configure these IP addressed as loopback interfaces on the ISP. Do NOT configure any other routes on the ISP; if NAT is working correctly, the ISP should be able to respond to devices behind R1.

- Prevent users outside the 10.0.0.0/8 network from managing (via telnet or SSH) any device inside your corporate network. Test your configuration from R1 using a source interface of S0/0.

```
ip access-list standard LIMIT_TELNET
 permit 10.0.0.0 0.255.255.255 
 deny any
line vty 0 4
 access-class LIMIT_TELNET in
```

## Configurations

### R1

```
R1#show run 
Building configuration...

Current configuration : 2628 bytes
!
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$s0CM$ssdGQBqn2BrDUrVzDe5iv/
!
no aaa new-model
memory-size iomem 5
ip cef
!
!
no ip dhcp use vrf connected
ip dhcp excluded-address 10.24.2.1 10.24.2.99
ip dhcp excluded-address 10.24.2.151 10.24.2.255
ip dhcp excluded-address 10.24.5.1 10.24.5.99
ip dhcp excluded-address 10.24.5.151 10.24.5.255
!
ip dhcp pool IT
   network 10.24.2.0 255.255.255.0
   dns-server 4.2.2.2 8.8.8.8 
   default-router 10.24.2.1 
   domain-name ccna.it
!
ip dhcp pool ACCOUNTING
   network 10.24.5.0 255.255.255.0
   dns-server 4.2.2.2 8.8.8.8 
   default-router 10.24.5.1 
   domain-name ccna.accounting
!
!
no ip domain lookup
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
archive
 log config
  hidekeys
! 
!
!
!         
!
!
!
!
interface FastEthernet0/0
 ip address 10.24.0.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly
 speed 100
 full-duplex
!
interface FastEthernet0/0.2
 encapsulation dot1Q 2
 ip address 10.24.2.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly
!
interface FastEthernet0/0.5
 encapsulation dot1Q 5
 ip address 10.24.5.1 255.255.255.0
 ip nat inside
 ip virtual-reassembly
!         
interface FastEthernet0/1
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address 10.1.51.100 255.255.255.128
 ip nat outside
 ip virtual-reassembly
 duplex auto
 speed auto
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
router rip
 version 2
 passive-interface FastEthernet1/0
 network 10.0.0.0
 default-information originate
 no auto-summary
!
ip forward-protocol nd
ip route 0.0.0.0 0.0.0.0 10.1.51.1
!
!
ip http server
no ip http secure-server
ip nat inside source list NAT interface FastEthernet1/0 overload
!
ip access-list standard LIMIT_TELNET
 permit 10.0.0.0 0.255.255.255
 deny   any
ip access-list standard NAT
 permit 10.24.0.0 0.0.0.255
 permit 10.24.2.0 0.0.0.255
 permit 10.24.5.0 0.0.0.255
!
!
!
!
!         
!
!
control-plane
!
!
!
!
!
!
!
!
!
banner motd ^C
###############################################################
#                 Authorized access only!                     # 
# Disconnect IMMEDIATELY if you are not an authorized user!!! #
#         All actions Will be monitored and recorded          #
###############################################################
^C
!
line con 0
 exec-timeout 0 0
 password Cisco
 logging synchronous
 login
line aux 0
line vty 0 4
 access-class LIMIT_TELNET in
 password Cisco
 login
!
!
end
```

### R2

```
R2#show run 
Building configuration...

Current configuration : 1510 bytes
!
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R2
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$fwxm$.Am1HyPD0WT3.AVITEYkt.
!
no aaa new-model
memory-size iomem 5
ip cef
!
!
!
!
no ip domain lookup
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
archive   
 log config
  hidekeys
! 
!
!
!
!
!
!
!
interface FastEthernet0/0
 ip address 10.24.0.2 255.255.255.0
 speed 100
 full-duplex
!
interface FastEthernet0/1
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface FastEthernet1/0
 ip address 10.1.5.13 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
router rip
 version 2
 network 10.0.0.0
 no auto-summary
!
ip forward-protocol nd
!
!
ip http server
no ip http secure-server
!
ip access-list standard LIMIT_TELNET
 permit 10.0.0.0 0.255.255.255
 deny   any
!
!
!
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
!
banner motd ^C
###############################################################
#                 Authorized access only!                     # 
# Disconnect IMMEDIATELY if you are not an authorized user!!! #
#         All actions Will be monitored and recorded          #
###############################################################
^C
!
line con 0
 exec-timeout 0 0
 password Cisco
 logging synchronous
 login
line aux 0
line vty 0 4
 access-class LIMIT_TELNET in
 password Cisco
 login
!
!
end
```
### R3

```
R3#show run 
Building configuration...

Current configuration : 1546 bytes
!
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R3
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$Y1yq$EwFarvO0vvufJnzqPbGoO1
!
no aaa new-model
memory-size iomem 5
ip cef
!
!
!
!
no ip domain lookup
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
archive   
 log config
  hidekeys
! 
!
!
!
!
!
!
!
interface FastEthernet0/0
 ip address 10.23.1.1 255.255.255.0
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
 ip address 10.1.5.14 255.255.255.252
 duplex auto
 speed auto
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
router rip
 version 2
 passive-interface FastEthernet0/0
 network 10.0.0.0
 no auto-summary
!
ip forward-protocol nd
!
!
ip http server
no ip http secure-server
!
ip access-list standard LIMIT_TELNET
 permit 10.0.0.0 0.255.255.255
 deny   any
!
!
!
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
!
banner motd ^C
###############################################################
#                 Authorized access only!                     # 
# Disconnect IMMEDIATELY if you are not an authorized user!!! #
#         All actions Will be monitored and recorded          #
###############################################################
^C
!
line con 0
 exec-timeout 0 0
 password Cisco
 logging synchronous
 login
line aux 0
line vty 0 4
 access-class LIMIT_TELNET in
 password Cisco
 login
!
!
end
```

### S1

```
S1#show run 
Building configuration...

Current configuration : 3804 bytes
!
! Last configuration change at 05:36:32 UTC Thu Sep 28 2017
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname S1
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$wG36$H5L9O9j580WMHJErfo5I31
!
no aaa new-model
!
!
!
!         
!
no ip routing
!
!
!
no ip domain-lookup
no ip cef
no ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
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
interface GigabitEthernet0/0
 switchport trunk allowed vlan 1,2,5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 media-type rj45
 speed 100
 duplex full
 no negotiation auto
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 1-5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 media-type rj45
 speed 100
 duplex full
 no negotiation auto
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 1-5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 media-type rj45
 speed 100
 duplex full
 no negotiation auto
!
interface GigabitEthernet0/3
 media-type rj45
 negotiation auto
!
interface Vlan1
 ip address 10.24.0.11 255.255.255.0
 no ip route-cache
!
ip default-gateway 10.24.0.1
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
ip access-list standard LIMIT_TELNET
 permit 10.0.0.0 0.255.255.255
 deny   any
!
!
!
!
!
control-plane
!
banner motd ^C
###############################################################
#                 Authorized access only!                     # 
# Disconnect IMMEDIATELY if you are not an authorized user!!! #
#         All actions Will be monitored and recorded          #
###############################################################
^C        
!
line con 0
 exec-timeout 0 0
 password Cisco
 logging synchronous
 login
line aux 0
line vty 0 4
 access-class LIMIT_TELNET in
 password Cisco
 login
!
!
end
```

### S2

```
S2#show run 
Building configuration...

Current configuration : 3633 bytes
!
! Last configuration change at 05:36:27 UTC Thu Sep 28 2017
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname S2
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$SeJh$AZWhkrHPKi.TvZ5UC.mu30
!
no aaa new-model
!
!
!
!         
!
no ip routing
!
!
!
no ip domain-lookup
no ip cef
no ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
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
interface GigabitEthernet0/0
 switchport trunk allowed vlan 1-5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 media-type rj45
 speed 100
 duplex full
 no negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 2
 switchport mode access
 media-type rj45
 speed 100
 duplex full
 no negotiation auto
!         
interface GigabitEthernet0/2
 media-type rj45
 negotiation auto
!
interface GigabitEthernet0/3
 media-type rj45
 negotiation auto
!
interface Vlan1
 ip address 10.24.0.12 255.255.255.0
 no ip route-cache
!
ip default-gateway 10.24.0.1
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
ip access-list standard LIMIT_TELNET
 permit 10.0.0.0 0.255.255.255
 deny   any
!         
!
!
!
!
control-plane
!
banner motd ^C
###############################################################
#                 Authorized access only!                     # 
# Disconnect IMMEDIATELY if you are not an authorized user!!! #
#         All actions Will be monitored and recorded          #
###############################################################
^C
!
line con 0
 exec-timeout 0 0
 password Cisco
 logging synchronous
 login    
line aux 0
line vty 0 4
 access-class LIMIT_TELNET in
 password Cisco
 login
!
!
end
```

### S3

```
S3#show run 
Building configuration...

Current configuration : 3660 bytes
!
! Last configuration change at 05:36:21 UTC Thu Sep 28 2017
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname S3
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$jF5H$e8ohzgV7xD8wF/Kegr3iE0
!
no aaa new-model
!
!
!
!         
!
no ip routing
!
!
!
no ip domain-lookup
no ip cef
no ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
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
interface GigabitEthernet0/0
 switchport trunk allowed vlan 1-5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 media-type rj45
 speed 100
 duplex full
 no negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 5
 switchport mode access
 media-type rj45
 speed 100
 duplex full
 no negotiation auto
!         
interface GigabitEthernet0/2
 media-type rj45
 speed 100
 duplex full
 no negotiation auto
!
interface GigabitEthernet0/3
 media-type rj45
 negotiation auto
!
interface Vlan1
 ip address 10.24.0.13 255.255.255.0
 no ip route-cache
!
ip default-gateway 10.24.0.1
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
ip access-list standard LIMIT_TELNET
 permit 10.0.0.0 0.255.255.255
 deny   any
!
!
!
!
!
control-plane
!
banner motd ^C
###############################################################
#                 Authorized access only!                     # 
# Disconnect IMMEDIATELY if you are not an authorized user!!! #
#         All actions Will be monitored and recorded          #
###############################################################
^C
!
line con 0
 exec-timeout 0 0
 password Cisco
 logging synchronous
 login
line aux 0
line vty 0 4
 access-class LIMIT_TELNET in
 password Cisco
 login
!
!
end
```