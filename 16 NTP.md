## NTP



1. Reconfigure your router so it has a connection to the internet

2. Determine the IP address of pool.ntp.org

3. Configure your device to obtain the time in your local time zone using NTP from pool.ntp.org.

4. Verify NTP is functioning correctly


```
[c:\~]$ ping pool.ntp.org

Pinging pool.ntp.org [198.58.105.63] with 32 bytes of data:
Reply from 198.58.105.63: bytes=32 time=35ms TTL=53
Reply from 198.58.105.63: bytes=32 time=35ms TTL=53
Reply from 198.58.105.63: bytes=32 time=34ms TTL=53
Reply from 198.58.105.63: bytes=32 time=33ms TTL=53

Ping statistics for 198.58.105.63:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 33ms, Maximum = 35ms, Average = 34ms

```


```
R1(config)#ntp server 198.58.105.63

R1(config)#clock timezone PDT -8
R1(config)#
Sep 26 06:25:32.719: %SYS-6-CLOCKUPDATE: System clock has been updated from 06:25:32 GMT Tue Sep 26 2017 to 22:25:32 PDT Mon Sep 25 2017, configured from console by console.

```

···
R1#show ntp associations 

      address         ref clock     st  when  poll reach  delay  offset    disp
*~198.58.105.63    142.66.101.13     2    54    64  367    20.1   -6.54     7.9
 * master (synced), # master (unsynced), + selected, - candidate, ~ configured
R1#show clock
22:26:52.489 PDT Mon Sep 25 2017
···
