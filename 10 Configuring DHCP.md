# Configuring DHCP

1. Restore the Router-on-a-Stick configuration on the switch and router ; the router should own the first ip of each vlan.
2. configure the clients for DHCP. After 60 seconds of enabling DHCP , see what ip address they have. What is this IP address?
3. Configure the router as a DHCP server for each subnet, distributing host address between 20 and 99 in each subnet. Aso distribute 4.2.2.2 and 8.8.8.8 as primary and secondary DNS servers. Verify hosts receive the addresses.
4. Disable DHCP on the Router and configure DHCP relay for the DHCP server 10.1.51.200 on VLAN 51. If you can , configure a DHCP server at this address for both subnets and ensure DHCP relay functions properly.

```
no ip dhcp use vrf connected
ip dhcp excluded-address 10.1.51.1 10.1.51.19
ip dhcp excluded-address 10.1.51.100 10.1.51.255
ip dhcp excluded-address 10.1.52.1 10.1.52.19
ip dhcp excluded-address 10.1.52.100 10.1.52.255
!
ip dhcp pool ENG
   network 10.1.51.0 255.255.255.0
   default-router 10.1.51.1 
   dns-server 4.2.2.2 8.8.8.8 
!
ip dhcp pool MGMT
   network 10.1.52.0 255.255.255.0
   default-router 10.1.52.1 
   dns-server 4.2.2.2 8.8.8.8 
```

```
R1#show ip dhcp binding 
Bindings from all pools not associated with VRF:
IP address          Client-ID/	 	    Lease expiration        Type
		    Hardware address/
		    User name
10.1.51.20          0100.5079.6668.01       Mar 02 2002 01:03 AM    Automatic
10.1.52.20          0100.5079.6668.02       Mar 02 2002 01:02 AM    Automatic
     
R1#show ip dhcp conflict 
IP address        Detection method   Detection time          VRF
R1#

```

## DHCP relay

```
R1(config)#int f0/0.52

R1(config-subif)#ip helper-address 10.1.51.200
```