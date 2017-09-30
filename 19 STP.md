## STP

<img src=images\2017-09-28_18-46-16.png>

1. Ensure switches have a basic base configuration and connect as shown

2. User the proper show command to determine the version of STP running, port status, and identify the Root Bridge

3. Cause a Root port outage ; determine how long it takes to converge  

PSVT: LIS(15sec)[BPDU] >> LRN (15sec)[mac address] >> FWD/BLK 


4. Change all switches to use RSTP test the outage agian

```
S1(config)#spanning-tree mode ?
  mst         Multiple spanning tree mode
  pvst        Per-Vlan spanning tree mode
  rapid-pvst  Per-Vlan rapid spanning tree mode

S1(config)#spanning-tree mode rapid-pvst
```

RSTP: 2sec

5. Modify the bridge priority to elect S1 as the Root bridge ,S2 as the Backup Root. Diagram port results and verify your assumptions are connect to the switch.

```
S1(config)#spanning-tree vlan 1 priority 4096

S1(config)#do show spanning-tree 

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    4097
             Address     5000.0001.0000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4097   (priority 4096 sys-id-ext 1)
             Address     5000.0001.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    Shr 
Gi0/1               Desg FWD 4         128.2    Shr 
Gi0/2               Desg FWD 4         128.3    Shr 
Gi0/3               Desg FWD 4         128.4    Shr 
Gi1/0               Desg FWD 4         128.5    Shr 
Gi1/1               Desg FWD 4         128.6    Shr
```
<img src=images\2017-09-28_18-46-16-2.png>