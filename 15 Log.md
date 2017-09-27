## Log
 
Router messages

0	Emergencies	System shutting down due to missing fan tray
1	Alerts	Temperature limit exceeded
2	Critical	Memory allocation failures
3	Errors	Interface Up/Down messages
4	Warnings	Configuration file written to server, via SNMP request
5	Notifications	Line protocol Up/Down
6	Information	Access-list violation logging
7	Debugging	Debug messages


### Buffered Logging:

```
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#logging buffered informational
Router(config)#end
```

### Terminal logging

```
Router#terminal monitor
```



### Syslog server

```
Router#conf t

Router(config#logging host x.x.x.x

Router(config)#logging traps (i.e 0 1 2 3 4 5 .. according to your requirement)
```

### Clear Log

```
Router#clear logging

Router# show logging
```