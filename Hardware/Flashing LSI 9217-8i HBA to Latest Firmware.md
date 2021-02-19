## [1] Flashing LSI 9217-8i HBA to Latest Firmware

1、 在 Broadcom 官网上下载最新的 Firmware 和 BIOS

[9217-8i_Package_P20_IR_IT_Firmware_BIOS_for_MSDOS_Windows](https://www.broadcom.com/support/download-search/?pg=Storage+Adapters,+Controllers,+and+ICs&pf=SAS/SATA/NVMe+Host+Bus+Adapters&pn=SAS+9217-8i+Host+Bus+Adapter&pa=Firmware&po=&dk=)

9217-8i_Package_P20_IR_IT_Firmware_BIOS_for_MSDOS_Windows 

2、下载Installer_P20_for_UEFI

__为了获得更好的兼任性，墙裂建议使用带有UEFI的服务器主板刷固件。__ 

3、 将sas2flash.efi  9207-8.bin  mptsas2.rom 拷入U盘

4、  如果在DOS下刷新需制作DOS启动盘

5、  启动BIOS（按住del键），选择UEFI  SHELL为第一启动顺序

6、  挂载U盘(mount fs0: 进入U盘系统fs0:)

7、  查看hba卡的<num>
```Bash
sas2flash.efi -listall
```

8、  sas2flash.efi -o –c <num> -e 6（必需，删除现有的firmware）
```Bash
sas2flash.efi -o –c 0 -e 6
```

9、 sas2flash.efi -o –c <num> -f 9207-8.bin -b mptsas2.rom
```Bash
sas2flash.efi -o –c 0 -f 9207-8.bin -b mptsas2.rom
```

-----------

## [2] HP H220 升级P20固件

先升级到 HP 最新的P15固件，再用 LSI 9207-8i P14升级程序升级P20固件

[HP H2xx SAS/SATA Host Bus Adapter Driver for Microsoft Windows Server 2008 R2 Editions](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=5263567&swItemId=MTX_37c7deddabb444758854c9e6e9&swEnvOid=4184)

[* RECOMMENDED * Online ROM Flash Component for Windows (x64) - HPE Host Bus Adapters H220, H221, H222, H210i and H220i](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=5263567&swItemId=MTX_fef291f3a22e41f6be409d99dc&swEnvOid=4184#tab3)

```Bash
# Verify sas2flash version
sas2flash
# Output will indicate
# LSI Corporation SAS2 Flash Utility
# Version 14.00.00.00 (2012.07.05)
#List HBA
sas2flsh -listall

#List SAS Addres
sas2flsh -o -c 0 -listsasadd

# Erase ROM
sas2flsh -o -c 0 -e 6

# Apply new FW
sas2flsh -o –c 0 -f 9207-8.bin -b mptsas2.rom

# Register SAS address
sas2flsh -o -c 0 -sasadd XXXXXXXXXXX
sas2flsh -o -c 0 -sasadd 50062b000000000 
```

### Link:<br>
[LSI RAID Controller and HBA Complete Listing Plus OEM Models](https://forums.servethehome.com/index.php?threads/lsi-raid-controller-and-hba-complete-listing-plus-oem-models.599/)<br>
[LSI MegaCLi (preboot CLi),StoreCLi, MegaSCU, MegaREC, SAS2flash and MegaOEM commands](https://forums.servethehome.com/index.php?threads/lsi-megacli-preboot-cli-storecli-megascu-megarec-sas2flash-and-megaoem-commands.457/)
