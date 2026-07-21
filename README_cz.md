# Odemknutí Cetin Terminátor

## Co budete potřebovat
### Hardware
* Křížový šroubovák (➕)
* USB-TTL převodník 3.3V s propojkami
* Něco na odcvaknutí západek

### Software
* TFTP Server, například Tftpd64 nebo libovolná linuxová varianta
* Klient Putty, GNU screen nebo Minicom
* Python 3
* GCC arm toolchain na sestavení nvram editoru

## Rozebrání
- Šroubovákem vyloupněte gumové nožičky na straně s konektory
- Vyšroubujte 2 schované šroubky
- Kusem plastu uvolněte západky po stranách
- Z horní části krytu vyklopte PCB
- Gumové nožičky si schovejte tak aby se na lepidlo nedostal bordel

## Modifikace softwaru
- Podle fotky připojte TTL převodník s rychlostí 115200 baud
- Na PC si nastavte statickou IP v rozsahu 192.168.1.0/24 krom .1
- Otevřte TFTP server nastavený se souborem V517ABQA3.1C0.bin
- Připojte ethernetový kabel
- Zapněte modem a přerušte autoboot, nainstalujte nový software jednou z následujících možností:
- A. Příkazem ```ATUR IP:V517ABQA3.1C0.bin``` nainstalujte nový firmware a počkejte přibližně minutu.
- B. Otevřete http://192.168.1.1 v prohlížeči, vyberte nový .bin, možnost "Select Only if you want to write misc partition" nechte prázdný (bootloader vybere automaticky správný) a tlačítkem "Update software" nahrajte, router se sám restartuje
- Po dokončení zápisu se modem z restartuje, počkejte než se na konzoli objeví login Linuxu a pak modem obnovte do továrního nastavení.
- Po resetu bude webové rozhraní a SSH dostupné na 192.168.1.1, hesla pro supervisor a roota si musíte vygenerovat ze sériového čísla na štítku. ( https://github.com/boginw/zyxel-vmg8825-keygen ), výstup byde typu zcfgBeCommonGenKeyBySerialNumMethod3
- Pokud nepotřebujete odemknout bootloader tak je hotovo, DSL si to samo nastaví a do par minut půjde vytočit PPPoE session na VLAN 848

## Bootloader unlock
- K odemknutí zařízení to není potřeba, i pokročilý uživatel pravděpodobně potřebovat odemčený bootloader ani debug příkazy ZyCli potřebovat nebude!
- Podle verze CFE půjde klasický postup ATSE+ATEN jako u jiných Zyxelů, jen s novým algoritmem ( https://github.com/cjdelisle/ATENv3 )
- Na některých zařízení vám ATSE nedá klíč (error -1) tak je třeba přehodit nastavení přímo v nvram
- Stačí nainstalovat gcc-arm-linux-gnueabi a zbytek je v souboru Zyxel_CFE_nvram_EngDebugFlag.c

## Bootloader unlock v2
- Modem připojte do libovolého PC s Linuxem
- Postavte nástroj https://github.com/bmork/zyxel-hacks/
- Příkazem ```busybox ifconfig eth0 up``` aktivujte ethernet interface
- Jako root spustě nástroj ```sudo ./zyeng eth0```
- Zapněte modem a počkejte na výstup:
```
Multiboot client version: 1.6

Hit any key to stop autoboot:001

Multiboot server is available for download firmware image!
Be patient, it should be finish in 15 minutes...
No file need to download, stop multiboot service!


Updated Engineer Debug Flag!
Magic number didn`t match!

```
- Resetujte vytažením napájení a přikazem ATSH zkontrolujte výstup ```Boot Module Debug Flag : 01```

## TODO
- Vyzkoušet unlock DM4200 ✅- Stejný postup, potřebuje unlock c2 a po resetu hesla z ATCK příkazu.
- Vyzkoušet unlock VMG4005-B50A ❓
- [Firmware k stažení pro DIY odemčení](https://files.qqwee.net/Zyxel/)

Vyzkoušeno na dvou zažízení oba fungují krásně u O2
