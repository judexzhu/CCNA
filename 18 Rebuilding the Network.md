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

- Manually configure trunk ports between S1, S2 and S3.

- Add the following VLANs to S1, S2 and S2. Ensure these VLANs (and the default VLAN) are the only VLANs allowed to pass between all switches. You can optionally use VTP to save some configuration.