
# Zyxel VMG4005-B60A ( Cetin Terminátor ) Debranding quick guide
## [[Česky](README_cz.md)]

## Requirements
### Hardware
* Phillips head screwdriver (➕)
* 3.3V UART Adapter + Jumper cables
* Pry tool or old plastic card

### Software
* Any TFTP Server
* Terminal client
* Python3
* armv7 toolchain that can compile static binary (optional)

## Disassembly
- Carefully remove the 2 rubber feet located on the side of the connector near the sticker.
- Remove the 2 Phillips head screws.
- Using your favorite tool, pry off the bottom plastic cover, there are clips on each side and 2 clips opposite the connectors.
- Remove the circuit board from the top plastic case.

## Software flash
- Connect to UART (pic provided in repo) at 115200 Baud
- Set your PC to static IP in range 192.168.1.2-254
- Start TFTP server with clean firmware ( V517ABQA3.1C0.bin )
- Connect to modems ethernet port
- Turn on modem and interupt CFE autoboot
- Run command ```ATUR IP:V517ABQA3.1C0.bin```
- Wait for the flashing to complete and let the device boot into the new firmware. Once it boots up, perform a factory reset.
- Find a tool of your choice to generate supervisor and root passwords from serial number. "zyxel-vmg8825-keygen" tested and working ok
- Unlock done!

## Bootloader unlock v1 
- Try normal ATSE+ATEN commands, if they don't work then go for risky method.
- Install arm toolchain for cross compile for your linux distribution
- Follow instructions in Zyxel_CFE_nvram_EngDebugFlag.c
- Get unlocked bootloader or a brick, this tool is a PoC and i don't guarantee any success, CFE can recover from missing nvram if its not corrupted too badly and you backed up data it needs (if it boots back into OS you can flash back mtd dump backup)...

## Bootloader unlock v2
- Connect modem to a Linux PC
- Clone and build https://github.com/bmork/zyxel-hacks/ 
- Bring up ethernet interface ```busybox ifconfig eth0 up```
- Start the zyeng tool as root ```sudo ./zyeng eth0```
- Power up the device and wait for the following output:
```
Multiboot client version: 1.6

Hit any key to stop autoboot:001

Multiboot server is available for download firmware image!
Be patient, it should be finish in 15 minutes...
No file need to download, stop multiboot service!


Updated Engineer Debug Flag!
Magic number didn`t match!

```
- Power cycle the device and verify with ATSH output ```Boot Module Debug Flag : 01``` 

## TODO
- Test zyxel multicast ✅
- Find why ATSE does not accept any seed
- Maybe make mod fw that does not generate passwords with SN algo
- Look at VMG4005-B50A and DM4200 [Firmware for DIY unlock here](https://files.qqwee.net/Zyxel/)
