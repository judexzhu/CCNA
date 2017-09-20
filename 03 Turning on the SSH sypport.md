## Enable SSH

```
S1#conf t 
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#ip domain-name ?
  WORD  Default domain name

S1(config)#ip domain-name buyabs.corp

S1(config)#crypto key generate rsa 
The name for the keys will be: S1.buyabs.corp
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

S1(config)#
*Mar  1 01:22:56.843: %SSH-5-ENABLED: SSH 1.99 has been enabled
S1(config)#line vty 0 4
S1(config-line)#transport input ssh telnet
S1(config-line)#password cisco
S1(config-line)#login local

S1(config-line)#exit
S1(config)#
     
S1(config)#username Jude secret cisco
S1(config)#
```

## Show Users

```
S1#show users
    Line       User       Host(s)              Idle       Location
*  0 con 0                idle                 00:00:00   

  Interface    User               Mode         Idle     Peer Address

S1#
```

## Kick User

```
S1#clear line vty 0
[confirm]
 [OK]
S1#
```