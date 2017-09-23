## Trunking and VTP lab

<img src=images\2017-09-22_22-53-02.png >

1. Use two switches - completely erase the configuration on both of them

```
Switch#write erase 
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete

Switch#reload
Switch#show vlan 
Switch#dir flash
Switch#delete flash:vlan.dat
Switch#reload
```

2. Access switch1 (Left S1) and configure VTP Mode: Server, Domain:CBTNuggets (Set a VTP password if you like)

```
S1(config)#vtp domain CBTNuggets
Changing VTP domain name from NULL to CBTNuggets
S1(config)#
*Sep 22 01:44:00.460: %SW_VLAN-6-VTP_DOMAIN_NAME_CHG: VTP domain name changed to CBTNuggets.
S1(config)#vtp ?
  domain     Set the name of the VTP administrative domain.
  file       Configure IFS filesystem file where VTP configuration is stored.
  interface  Configure interface as the preferred source for the VTP IP updater
             address.
  mode       Configure VTP device mode
  password   Set the password for the VTP administrative domain
  pruning    Set the administrative domain to permit pruning
  version    Set the administrative domain to VTP version

S1(config)#vtp mode server
Device mode already VTP Server for VLANS.
S1(config)#vtp password cbt
Setting device VTP password to cbt
S1(config)#
```

3. Add the VLANS show and assign ports on S1

```
S1(config)#vlan 2
S1(config-vlan)#vlan 3
S1(config-vlan)#exit
S1(config)#exit
S1#
*Sep 22 01:46:37.570: %SYS-5-CONFIG_I: Configured from console by console
S1#show vlan   

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1, Gi0/2, Gi0/3
                                                Gi1/0
2    VLAN0002                         active    
3    VLAN0003                         active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0   
2    enet  100002     1500  -      -      -        -    -        0      0   
3    enet  100003     1500  -      -      -        -    -        0      0   
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------
S1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1, Gi0/2, Gi0/3
                                                Gi1/0
2    VLAN0002                         active    
3    VLAN0003                         active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
```

```
S1(config)#int range gigabitEthernet 0/0-1 
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 2
S1(config-if-range)#no shut

S1(config)#int range gigabitEthernet 0/2-3 
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 3
S1(config-if-range)#no shut
```

```
S1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi1/0
2    VLAN0002                         active    Gi0/0, Gi0/1
3    VLAN0003                         active    Gi0/2, Gi0/3
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 

```

4. Config Port5 as a trunk link, Manual configuration (Don't disable DTP), 802.1q encapsulation. Check the config on S2

```
S1(config)#int g1/0
S1(config-if)#switchport trunk encapsulation dot1q 
S1(config-if)#switchport mode trunk 
S1(config-if)#no shut
```

```
S1#show interfaces trunk      

Port        Mode             Encapsulation  Status        Native vlan
Gi1/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Gi1/0       1-4094

Port        Vlans allowed and active in management domain
Gi1/0       1-3

Port        Vlans in spanning tree forwarding state and not pruned
Gi1/0       1-3
```


```
S2>show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1, Gi0/2, Gi0/3
                                                Gi1/0
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
S2>show int trunk
S2>
```

5. Plug the two switches together on Port 5 . What happens? What is the config of S2, port 5? What VLANs does it have?

```
S2#show int trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi1/0       auto             n-802.1q       trunking      1

Port        Vlans allowed on trunk
Gi1/0       1-4094

Port        Vlans allowed and active in management domain
Gi1/0       1

Port        Vlans in spanning tree forwarding state and not pruned
Gi1/0       1

```
6. Configure VTP on S2 and see if VLANs replicate

```
S2#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1, Gi0/2, Gi0/3
2    VLAN0002                         active    
3    VLAN0003                         active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 

```

7. Verify the Trunk link - what VLANs are permitted to cross?

```
S1>show int trunk 

Port        Mode             Encapsulation  Status        Native vlan
Gi1/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Gi1/0       1-4094

Port        Vlans allowed and active in management domain
Gi1/0       1-3

Port        Vlans in spanning tree forwarding state and not pruned
Gi1/0       1-3
S1>

```

```
S2#show int trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi1/0       auto             n-802.1q       trunking      1

Port        Vlans allowed on trunk
Gi1/0       1-4094

Port        Vlans allowed and active in management domain
Gi1/0       1-3

Port        Vlans in spanning tree forwarding state and not pruned
Gi1/0       1-3
S2#
```
8. Plug some PCs in to the switches and move them around similarly to the last lab and verify all is working as expected.

