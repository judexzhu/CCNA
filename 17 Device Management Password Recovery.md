## How a Cisco Device Boots

1. Check the configuration register
    - 2100: ROMMON
    - 2101: RXBOOT
    - 2102: BOOT NORMALLY
    - 2142: IGNORE NVRAM

2. Check for "Boot System" commands in the startup config

3. Look for the first IOS image in FLash

4. Broadcast for a TFTP server



Boot

Ctrl + Break

rommon x> confreg 0x2142

rommon x> reset

Router#copy startup-config running-config


Router(config)#enable secret newpasswordx

Router(config)#enable password newpasswor

Router(config)#username newuser password newpass

Router#wr

Router(config)#config-register 0x2102


