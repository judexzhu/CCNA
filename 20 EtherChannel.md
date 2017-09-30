## EtherChannel: Configuring Bundles

<img src=images\2017-09-29_13-27-10.png>

1. Beginning with a base configuration, set all ports as access ports in VLan 1

2. Configure Etherchannel on the interfaces between on the interfaces between S2 and S3 using PAgP.

3. Configure Etherchannel on the interfaces between S1 and S2 using LACP

S1

```
S1(config)#int ran g0/0 - 1
S1(config-if-range)#shutdown
S1(config-if-range)#channel-group 1 mode active 
Creating a port-channel interface Port-channel 1

S1(config-if-range)#no shut
```

```
S1#show etherchannel summary 
Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      N - not in use, no aggregation
        f - failed to allocate aggregator

        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port

        A - formed by Auto LAG


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SD)         LACP      Gi0/0(s)    Gi0/1(s)    
```

S2

```
S2(config)# int ran g0/0 - 1
S2(config-if-range)#shutdown

S2(config-if-range)#channel-group 1 mode passive 
Creating a port-channel interface Port-channel 1

S2(config-if-range)#no shut 
```

```
S2#show etherchannel summary 
Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      N - not in use, no aggregation
        f - failed to allocate aggregator

        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port

        A - formed by Auto LAG


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SU)         LACP      Gi0/0(P)    Gi0/1(P) 
```

4. Configure hardcoreed Etherchannel on the interfaces between S1 and S3

5. Examine the configuration using show commands to verify Etherchannel works correctly

6. Misconfigure an interface in the PAgP/LACP bundle. What happens?

7. Fix the issue. What happens now ? 