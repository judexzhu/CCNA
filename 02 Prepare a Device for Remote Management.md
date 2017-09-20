## clear configuration 

```
ESW1#write erase 
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
ESW1#
```

```
ESW1#erase startup-config 
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
ESW1#
*Mar  1 00:03:40.743: %SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram
ESW1#delete flash:vlan.dat
Delete filename [vlan.dat]? 
Delete flash:/vlan.dat? [confirm]
ESW1#reload
Proceed with reload? [confirm]
```

## Basic Switch config

```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#enable secret cisco
Switch(config)#line vty 0 4 
Switch(config-line)#password cisco
Switch(config-line)#login
Switch(config-line)#exit
Switch(config)#hostname S1     
S1(config)#line con 0
S1(config-line)#logg synchronous 
S1(config-line)#no exec-timeout 
S1(config-line)#^Z
*Mar  1 00:04:18.903: %SYS-5-CONFIG_I: Configured from console by console

S1#show ip interface br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  administratively down down    
FastEthernet0/1            unassigned      YES NVRAM  administratively down down    
FastEthernet1/0            unassigned      YES unset  up                    down    
FastEthernet1/1            unassigned      YES unset  up                    down    
FastEthernet1/2            unassigned      YES unset  up                    down    
FastEthernet1/3            unassigned      YES unset  up                    down    
FastEthernet1/4            unassigned      YES unset  up                    down    
FastEthernet1/5            unassigned      YES unset  up                    down    
FastEthernet1/6            unassigned      YES unset  up                    down    
FastEthernet1/7            unassigned      YES unset  up                    down    
FastEthernet1/8            unassigned      YES unset  up                    down    
FastEthernet1/9            unassigned      YES unset  up                    down    
FastEthernet1/10           unassigned      YES unset  up                    down    
FastEthernet1/11           unassigned      YES unset  up                    down    
FastEthernet1/12           unassigned      YES unset  up                    down    
FastEthernet1/13           unassigned      YES unset  up                    down    
FastEthernet1/14           unassigned      YES unset  up                    down    
FastEthernet1/15           unassigned      YES unset  up                    down    
Serial2/0                  unassigned      YES unset  administratively down down    
Serial2/1                  unassigned      YES unset  administratively down down    
Serial2/2                  unassigned      YES unset  administratively down down    
Serial2/3                  unassigned      YES unset  administratively down down    
Vlan1                      unassigned      YES NVRAM  administratively down down 

```

```
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#int vlan 1
S1(config-if)#ip add 10.1.1.2 255.255.255.0
S1(config-if)#no shut
S1(config-if)#
*Mar  1 00:06:12.575: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
S1(config-if)#^Z
S1#
*Mar  1 00:06:19.159: %SYS-5-CONFIG_I: Configured from console by console
S1#show ip interface br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  administratively down down    
FastEthernet0/1            unassigned      YES NVRAM  administratively down down    
FastEthernet1/0            unassigned      YES unset  up                    down    
FastEthernet1/1            unassigned      YES unset  up                    down    
FastEthernet1/2            unassigned      YES unset  up                    down    
FastEthernet1/3            unassigned      YES unset  up                    down    
FastEthernet1/4            unassigned      YES unset  up                    down    
FastEthernet1/5            unassigned      YES unset  up                    down    
FastEthernet1/6            unassigned      YES unset  up                    down    
FastEthernet1/7            unassigned      YES unset  up                    down    
FastEthernet1/8            unassigned      YES unset  up                    down    
FastEthernet1/9            unassigned      YES unset  up                    down    
FastEthernet1/10           unassigned      YES unset  up                    down    
FastEthernet1/11           unassigned      YES unset  up                    down    
FastEthernet1/12           unassigned      YES unset  up                    down    
FastEthernet1/13           unassigned      YES unset  up                    down    
FastEthernet1/14           unassigned      YES unset  up                    down    
FastEthernet1/15           unassigned      YES unset  up                    down    
Serial2/0                  unassigned      YES unset  administratively down down    
Serial2/1                  unassigned      YES unset  administratively down down    
Serial2/2                  unassigned      YES unset  administratively down down    
Serial2/3                  unassigned      YES unset  administratively down down    
Vlan1                      10.1.1.2        YES manual up                    down 
```


