
## Change Hostname 

```
R2#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#hostname Router
Router(config)#
```

## Config motd banner

```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#banner motd ?
  LINE  c banner-text c, where 'c' is a delimiting character

Router(config)#banner motd !
Enter TEXT message.  End with the character '!'.
 ================== ** W A R N I N G ** ==================



This is an NEWEGG Inc. system, restricted

to authorized individuals. This system is subject to

monitoring. Unauthorized users, access, and/or

modification will be prosecuted.



=========================================================
!
Router(config)#^Z
Router#
```

## Console Port

```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#line console 0
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#^Z
Router#
*Mar  1 00:01:44.411: %SYS-5-CONFIG_I: Configured from console by console
Router#show run | sec line con
line con 0
 exec-timeout 0 0
 privilege level 15
 password cisco
 logging synchronous
 login
```

```
Compiled Wed 18-Aug-10 07:55 by prod_rel_team
*Mar  1 00:00:07.383: %SNMP-5-COLDSTART: SNMP agent on host Router is undergoing a cold start
*Mar  1 00:00:07.603: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to down
*Mar  1 00:00:08.143: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down
*Mar  1 00:00:08.647: %LINK-5-CHANGED: Interface Serial0/0, changed state to administratively down
*Mar  1 00:00:08.715: %LINK-5-CHANGED: Interface Serial0/1, changed state to administratively down
 ================== ** W A R N I N G ** ==================



This is an NEWEGG Inc. system, restricted

to authorized individuals. This system is subject to

monitoring. Unauthorized users, access, and/or

modification will be prosecuted.



=========================================================


User Access Verification

Password: 
Router#
```

## logging synchronous

```
Router#conf t
Router(config)#line con 0
Router(config-line)#logging synchronous 
Router(config-line)#
```

## line vty

```
Router#conf t 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#line vty 0 4
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#^Z
Router#show 
*Mar  1 00:03:13.591: %SYS-5-CONFIG_I: Configured from console by console
Router#show run | sec line vty 
line vty 0 4
 password cisco
 login
Router#
```

## show ip interface brief 

```
Router#show ip interface brief 
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  administratively down down    
Serial0/0                  unassigned      YES NVRAM  administratively down down    
FastEthernet0/1            unassigned      YES NVRAM  administratively down down    
Serial0/1                  unassigned      YES NVRAM  administratively down down    
Router#
```

## set ip address

```
Router#conf t 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface fastEthernet 0/0
Router(config-if)#ip address 192.168.0.1 255.255.255.0
Router(config-if)#no shut
Router(config-if)#interface fast 0/1
Router(config-if)#ip address dhcp 
Router(config-if)#exit
Router(config)#exit

Router#show ip interface brief 
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            192.168.0.1     YES manual up                    up      
Serial0/0                  unassigned      YES NVRAM  administratively down down    
FastEthernet0/1            unassigned      YES manual administratively down down    
Serial0/1                  unassigned      YES NVRAM  administratively down down
```

## Disable DNS lookup

```
Router#conf t 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#no ip domain-lookup 
Router(config)#
```

## password-encryption

```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#service password-encryption 
Router(config)#
```

## enable secret

```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#enable secret cisco
Router(config)#
```

# Save config

```
Router#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
Router#
```