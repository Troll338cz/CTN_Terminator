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
- Zapněte modem a přerušte autoboot
- Příkazem ```ATUR IP:V517ABQA3.1C0.bin``` nainstalujte nový firmware a počkejte přibližně minutu.
- Po dokončení zápisu se modem z restartuje, počkejte než se na konzoli objeví login Linuxu a pak modem obnovte do továrního nastavení.
- Po resetu bude webové rozhraní a SSH dostupné na 192.168.1.1, hesla pro supervisor a roota si musíte vygenerovat ze sériového čísla na štítku. ( https://github.com/boginw/zyxel-vmg8825-keygen )
- Pokud nepotřebujete odemknout bootloader tak je hotovo, DSL si to samo nastaví a do par minut půjde vytočit PPPoE session na VLAN 848

## Bootloader unlock
- K odemknutí zařízení to není potřeba, i pokročilý uživatel pravděpodobně potřebovat odemčený bootloader ani debug příkazy ZyCli potřebovat nebude!
- Podle verze CFE půjde klasický postup ATSE+ATEN jako u jiných Zyxelů, jen s novým algoritmem ( https://github.com/cjdelisle/ATENv3 )
- Na některých zařízení vám ATSE nedá klíč (error -1) tak je třeba přehodit nastavení přímo v nvram
- Stačí nainstalovat gcc-arm-linux-gnueabi a zbytek je v souboru Zyxel_CFE_nvram_EngDebugFlag.c

Vyzkoušeno na dvou zažízení oba fungují krásně u O2
