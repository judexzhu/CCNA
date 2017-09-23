## Speed Duplex

Gigabit port : Auto Auto

```
R3(config)#int gigabitEthernet 1
R3(config-if)#speed auto
R3(config-if)#duplex auto
```

FastEthernet0 : hardcore setting 100/Full

```
Jude(config)#int fastEthernet 0/0
Jude(config-if)#speed 100
Jude(config-if)#duplex full
*Mar  1 14:40:26.984: %LINK-3-UPDOWN: Interface FastEthernet0/0, changed state to up
Jude(config-if)#
```

## Port Security

Lab
1. Connect two devices to your Switch 1 (S1), port 1 and 2
2. Assign the device ip addresses on the same network and begin a continual ping between the devices.
3. View the Mac address table - verify the devices are appearing as expected
4. Enable port security on the ports to only allow a single MAC address

```
S1(config)#interface range fastEthernet 0/1-2
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport port-security
S1(config-if-range)#switchport port-security maximum 1
```

```
S1#show port-security interface fa0/1
```

5. On one of the devices. use a tool like Ostinato and generate traffic from multiple source addresses (alternate: another hub/switch); verify port security works.

`err-disable state`

6. Re-enable the port; ensure pings continue to work

```
S1(config)#interface range fastEthernet 0/1-2
S1(config-if-range)#shutdown
S1(config-if-range)#no shutdown
```

7. On both ports, enable the sticky MAC address feature

```
S1(config)#interface range fastEthernet 0/1-2
S1(config-if-range)#switchport port-security
S1(config-if-range)#switchport port-security mac-address sticky
```

the mac address will be learned and saved in the running-config

8. Move the device on port 1 and port 2 and vice versa. Verify port security is functioning as expected.
9. Re-enable the ports and remove port security

```
S1(config)#interface range fastEthernet 0/1-2
S1(config-if-range)#no switchport port-security
```


