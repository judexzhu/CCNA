# Routing between Vlans

<img src=images\2017-09-23_10-56-17.png>

1. Connect devices as shown 
2. Create VLAN 51(ENG) and VLAN 52(MGMT) on the switch and assign ports as shown
3. Configure a router-on-a-stick design. The router should be the first IP address on each subnet
NOTE: The trunk port between the router an switch should ONLY allow VLAN 51 and 52(VLAN 1 native)
4. The computer in VLAN 51 should be able to ping the computer in VLAN52 (and vice versa)