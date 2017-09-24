## L3 Switching 

<img src=images\2017-09-23_11-08-51.png>

1. Connect device as shown
2. Create VLAN 51(ENG) and VLAN 52 (MGMT) on the switch and assign ports as shown 
3. COnfigure a Layer 3 Switching desgin. The SVIs should have the first ip address on each subnet
4. The computer in VLAN 51 should be able to ping the computer in VLAN 52(and vice versa)

```
ip routeing

interface GigabitEthernet0/2
 switchport access vlan 51
 switchport mode access

!
interface GigabitEthernet0/3
 switchport access vlan 52
 switchport mode access

!
interface Vlan51
 ip address 10.1.51.1 255.255.255.0
!
interface Vlan52
 ip address 10.1.52.1 255.255.255.0

```