## Please add only OpenWrt and WRT610N related things to this page! Thanks.
'''Linksys WRT610N'''

'''Identification by S/N'''

Useful for identifying shrinkwrapped units. The '''S/N''' can be found on the box, left of the UPC and EAN barcodes.
||||<style="text-align: center;"> (!) '''Please contribute to this list.''' (!) ||
||||<style="text-align: center;">'''Model''' ||<style="text-align: center;"> '''S/N''' ||
||||<style="text-align: center;">WRT610N v1.0 ||<style="text-align: center;"> CTG01 ||


'''WRT610N v1.0'''

The WRT610N v1.0 is based on the Broadcom 4705 cpu running at 300MHz. It has 8 MB flash and 64 MB SDRAM (2x HY5DU561622FTP). The wireless NICs are a dual BCM4322 Chipset, one for 5GHz A and N and one for 2.4GHz B,G and N.  The switch is a BCM53115 chip. The WRT610N runs 802.11 A, B, G, and Draft N wireless protocols. It provides 4 gigabit LAN ports, 1 WAN port and a USB 2.0 'storage link' port.

Linksys source code is available at ftp://ftp.linksys.com/opensourcecode/wrt610n/

'''Original Firmware Boot Log'''

{{{
CFE version 1.0.37 for BCM947XX (32bit,SP,LE)
Build Date: Mon May 12 15:37:48 CST 2008 (ljh@team2-complier)
Copyright (C) 2000,2001,2002,2003 Broadcom Corporation.

Initializing Arena
Initializing PCI. [normal]
PCI bus 0 slot 0/0: vendor 0x14e4 product 0x0800 (flash memory, rev 0x02)
PCI bus 0 slot 1/0: vendor 0x14e4 product 0x471f (ethernet network, rev 0x02)
PCI bus 0 slot 2/0: vendor 0x14e4 product 0x471a (USB serial bus, interface 0x10, rev 0x02)
PCI bus 0 slot 2/1: vendor 0x14e4 product 0x471a (USB serial bus, interface 0x20, rev 0x02)
PCI bus 0 slot 3/0: vendor 0x14e4 product 0x471b (USB serial bus, rev 0x02)
PCI bus 0 slot 4/0: vendor 0x14e4 product 0x0804 (PCI bridge, rev 0x02)
PCI bus 0 slot 5/0: vendor 0x14e4 product 0x0816 (MIPS processor, rev 0x02)
PCI bus 0 slot 6/0: vendor 0x14e4 product 0x471d (IDE mass storage, rev 0x02)
PCI bus 0 slot 7/0: vendor 0x14e4 product 0x4718 (network/computing crypto, rev 0x02)
PCI bus 0 slot 8/0: vendor 0x14e4 product 0x080f (RAM memory, rev 0x02)
PCI bus 0 slot 9/0: vendor 0x14e4 product 0x471e (class 0xfe, subclass 0x00, rev 0x02)
Initializing Devices.

No DPN
This is a Parallel Flash
Boot partition size = 262144(0x40000)
Partition information:
boot    #00   00000000 -> 0003FFFF  (262144)
trx     #01   00040000 -> 0004001B  (28)
os      #02   0004001C -> 007F7FFF  (8093668)
nvram   #03   007F8000 -> 007FFFFF  (32768)
Partition information:
boot    #00   00000000 -> 0003FFFF  (262144)
trx     #01   00040000 -> 007F7FFF  (8093696)
nvram   #02   007F8000 -> 007FFFFF  (32768)
PCI bus 0 slot 1/0: pci_map_mem: attempt to map 64-bit region tag=0x800 @ addr=18010004
PCI bus 0 slot 1/0: pci_map_mem: addr=0x18010004 pa=0x18010000
ge0: BCM5750 Ethernet at 0x18010000
CPU type 0x2901A: 300MHz
Total memory: 65536 KBytes

Total memory used by CFE:  0x80700000 - 0x807A60B0 (680112)
Initialized Data:          0x8073A2B0 - 0x8073E540 (17040)
BSS Area:                  0x8073E540 - 0x807400B0 (7024)
Local Heap:                0x807400B0 - 0x807A40B0 (409600)
Stack Area:                0x807A40B0 - 0x807A60B0 (8192)
Text (code) segment:       0x80700000 - 0x8073A2B0 (238256)
Boot area (physical):      0x007A7000 - 0x007E7000
Relocation Factor:         I:00000000 - D:00000000

Boot version: v4.2
The boot is CFE

mac_init(): Find mac [00:21:29:C6:61:00] in location 0
Nothing...
country_init(): Find country code in location 0
The country is same
CMD: [ifconfig eth0 -addr=192.168.1.1 -mask=255.255.255.0]
eth0: Link speed: 1000BaseT FDX
Device eth0:  hwaddr 00-21-29-C6-61-00, ipaddr 192.168.1.1, mask 255.255.255.0
        gateway not set, nameserver not set
CMD: [go;]
Check CRC of image1
  Len:     0x717000     (7434240)       (0xBC040000)
  Offset0: 0x1C         (28)            (0xBC04001C)
  Offset1: 0x1016DC     (1054428)       (0xBC1416DC)
  Offset2: 0x0  (0)     (0xBC040000)
  Header CRC:    0x9E47184F
  Calculate CRC: 0x9E47184F
Image 1 is OK
Try to load image 1.
CMD: [load -raw -addr=0x807a60b0 -max=0x3a0000 :]
Loader:raw Filesys:tftp Dev:eth0 File:: Options:(null)
Loading: Failed.
Could not load :: Timeout occured
CMD: [boot -raw -z -addr=0x80001000 -max=0x3a0000 flash0.os:]
Loader:raw Filesys:raw Dev:flash0.os File: Options:(null)
Loading: .. 2416640 bytes read
Entry at 0x80001000
Closing network.
eth0: cannot clear 1400/00000002
Starting program at 0x80001000
CPU revision is: 0002901a
Primary instruction cache 32kb, linesize 16 bytes (4 ways)
Primary data cache 32kb, linesize 16 bytes (2 ways)
 2008
Setting the PFC to its default value
Determined physical RAM map:
 memory: 04000000 @ 00000000 (usable)
On node 0 totalpages: 16384
zone(0): 16384 pages.
zone(1): 0 pages.
zone(2): 0 pages.
Kernel command line: root=/dev/mtdblock2 noinitrd console=ttyS0,115200
CPU: BCM4785 rev 2 at 300 MHz
Calibrating delay loop... 299.82 BogoMIPS
Memory: 62208k/65536k available (2119k kernel code, 3328k reserved, 144k data, 76k init, 0k highmem)
Dentry cache hash table entries: 8192 (order: 4, 65536 bytes)
Inode cache hash table entries: 4096 (order: 3, 32768 bytes)
Mount-cache hash table entries: 1024 (order: 1, 8192 bytes)
Buffer-cache hash table entries: 4096 (order: 2, 16384 bytes)
Page-cache hash table entries: 16384 (order: 4, 65536 bytes)
Checking for 'wait' instruction...  unavailable.
POSIX conformance testing by UNIFIX
PCI: Initializing host
PCI: Fixing up bus 0
PCI: Fixing up bridge
PCI: Fixing up bus 1
Linux NET4.0 for Linux 2.4
Based upon Swansea University Computer Society NET3.039
Initializing RT netlink socket
Starting kswapd
devfs: v1.12c (20020818) Richard Gooch (rgooch@atnf.csiro.au)
devfs: boot_options: 0x1
NTFS driver v1.1.22 [Flags: R/O]
squashfs: version 3.0 (2006/03/15) Phillip Lougher
pty: 256 Unix98 ptys configured
Serial driver version 5.05c (2001-07-08) with MANY_PORTS SHARE_IRQ SERIAL_PCI enabled
ttyS00 at 0xb8000300 (irq = 8) is a 16550A
ttyS01 at 0xb8000400 (irq = 0) is a 16550A
HDLC line discipline: version $Revision$, maxframe=4096
N_HDLC line discipline registered.
PPP generic driver version 2.4.2
Broadcom Gigabit Ethernet Driver bcm5700 with Broadcom NIC Extension (NICE) ver. 8.3.14 (11/2/05)
PHY ID unknown, assume it is a copper PHY.
eth0: Broadcom BCM4785 10/100/1000 Integrated Controller found at mem 18010000, IRQ 5, node addr 002129c66100
eth0: Unknown transceiver found
eth0: Scatter-gather ON, 64-bit DMA ON, Tx Checksum ON, Rx Checksum ON, 802.1Q VLAN ON, NAPI ON
SCSI subsystem driver Revision: 1.00
 Amd/Fujitsu Extended Query Table v1.1 at 0x0040
number of CFI chips: 1
Flash device: 0x800000 at 0x1c000000
Physically mapped flash: squashfs filesystem found at block 1285
(Not Found Lang Block)off=0x1416dc off1=0x7e0000 size=0x800000
Creating 5 MTD partitions on "Physically mapped flash":
0x00000000-0x00040000 : "pmon"
0x00040000-0x007e0000 : "linux"
0x001416dc-0x007e0000 : "rootfs"
mtd: partition "rootfs" doesn't start on an erase block boundary -- force read-only
0x007e0000-0x007f0000 : "lang"
0x007f0000-0x00800000 : "nvram"
sflash: found no supported devices
usb.c: registered new driver usbdevfs
usb.c: registered new driver hub
ehci_hcd 00:02.1: PCI device 14e4:471a
ehci_hcd 00:02.1: irq 3, pci mem b8002800
usb.c: new USB bus registered, assigned bus number 1
ehci_hcd 00:02.1: illegal capability!
ECHI PCI device 471a14e4 found.
PCI: 00:02.1 PCI cache line size set incorrectly (0 bytes) by BIOS/FW, correcting to 32
ehci_hcd 00:02.1: USB 0.0 enabled, EHCI 1.00, driver 2003-Dec-29/2.4
hub.c: USB hub found
hub.c: 2 ports detected
host/usb-uhci.c: $Revision: 1.2 $ time 21:04:02 Jun  2 2008
host/usb-uhci.c: High bandwidth mode enabled
ECHI PCI device 471b14e4 found.
host/usb-uhci.c: v1.275:USB Universal Host Controller Interface driver
host/usb-ohci.c: USB OHCI at membase 0xb8002000, IRQ 3
host/usb-ohci.c: usb-00:02.0, PCI device 14e4:471a
usb.c: new USB bus registered, assigned bus number 2
hub.c: USB hub found
hub.c: 2 ports detected
usb.c: registered new driver usblp
printer.c: v0.13: USB Printer Device Class driver
Initializing USB Mass Storage driver...
usb.c: registered new driver usb-storage
USB Mass Storage support registered.
NET4: Linux TCP/IP 1.0 for NET4.0
IP Protocols: ICMP, UDP, TCP, IGMP
IP: routing cache hash table of 512 buckets, 4Kbytes
TCP: Hash tables configured (established 4096 bind 8192)
Linux IP multicast router 0.06 plus PIM-SM
ip_conntrack version 2.1 (512 buckets, 4096 max) - 368 bytes per conntrack
IPSEC isakmp Support Max Pass-through [50].
ip_conntrack_rtsp v0.01 loading
ip_nat_rtsp v0.01 loading
ip_tables: (C) 2000-2002 Netfilter core team
ipt_time loading
NET4: Unix domain sockets 1.0/SMP for Linux NET4.0.
IPv6 v0.8 for NET4.0
IPv6 over IPv4 tunneling driver
NET4: Ethernet Bridge 008 for NET4.0
802.1Q VLAN Support v1.7 Ben Greear <greearb@candelatech.com>
All bugs added by David S. Miller <davem@redhat.com>
cramfs: wrong magic
FAT: bogus logical sector size 19232
NTFS: Unable to set blocksize 512.
VFS: Mounted root (squashfs filesystem) readonly.
Mounted devfs on /dev
Freeing unused kernel memory: 76k freed
Algorithmics/MIPS FPU Emulator v1.5
cmd=[insmod ctmisc ]
Using /lib/modules/2.4.20/kernel/drivers/net/ctmisc/ctmisc.o
Register /dev/ctmisc device, major:250 minor:0
The boot is CFE
/dev/: cannot create
cmd=[misc -t get_wsc_pin -w 3 ]
type = [get_wsc_pin]
get_data(): cmd=ctmisc_ioctl: cmd=0x26, buffer size=196
0x26 count=8 lendata_init(): base = 0xbc03fcdc
=8
data_init(): location = [1], mydatas index = 1
ctmisc_ioctl: index=1
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get WSC count is [1]
get_data(): WSC 0: [86328432]
get_data(): done
cmd=[misc -t get_mac -w 3 ]
type = [get_mac]
get_data(): cmd=ctmisc_ioctl: cmd=0x11, buffer size=196
0x11 count=8 lendata_init(): base = 0xbc001e00
=18
data_init(): location = [1], mydatas index = 1
ctmisc_ioctl: index=1
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get MAC count is [1]
get_data(): MAC 0: [00:21:29:C6:61:00]
get_data(): done
cmd=[misc -t get_sn -w 3 ]
type = [get_sn]
get_data(): cmd=ctmisc_ioctl: cmd=0x15, buffer size=196
0x15 count=8 lendata_init(): base = 0xbc03fe30
=20
data_init(): location = [1], mydatas index = 1
ctmisc_ioctl: index=1
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get SN count is [1]
get_data(): SN 0: [CTG01H700321<FF><FF><FF><FF><FF><FF><FF><FF>]
get_data(): done
cmd=[misc -t get_country -w 3 ]
type = [get_country]
get_data(): cmd=ctmisc_ioctl: cmd=0x2c, buffer size=196
0x2c count=1 lendata_init(): base = 0xbc03fe2c
=4
data_init(): location = [1], mydatas index = 1
ctmisc_ioctl: index=1
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get COUNTRY count is [1]
get_data(): COUNTRY 0: [EU<FF><FF>]
get_data(): done
cmd=[misc -t get_flash_type -w 1 ]
type = [get_flash_type]
ctmisc_ioctl: cmd=0x17, buffer size=196
get_flash_type()Try 4: vendor id = 0x007F, device id = 0x22CB
: cmd=0x17 countEon EN29LV640B 4Mx16 BotB=0 len=0
Flash Type: Eon EN29LV640B 4Mx16 BotB
tallest:=====(ctmisc ioctl done...)=====
Get FLASH TYPE is [Eon EN29LV640B 4Mx16 BotB]
------ CT HW version is NULL ------
cmd=[misc -t get_pa2ga0idxval -w 3 ]
type = [get_pa2ga0idxval]
ctmisc_ioctl: cmd=0x28, buffer size=196
get_data(): cmd=data_init(): base = 0xbc03f6c0
0x28 count=8 lendata_init(): location = [1], mydatas index = 1
=24
ctmisc_ioctl: index=1
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get PA2GA0IDXVAL count is [1]
get_data(): PA2GA0IDXVAL 0: [4 0xfeca 0x14a4 0xfb21<FF><FF>]
get_data(): done
cmd=[misc -t get_pa2ga1idxval -w 3 ]
type = [get_pa2ga1idxval]
ctmisc_ioctl: cmd=0x2a, buffer size=196
get_data(): cmd=data_init(): base = 0xbc03f600
0x2a count=8 lendata_init(): location = [0], mydatas index = 0
=24
ctmisc_ioctl: index=0
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get PA2GA1IDXVAL count is [0]
get_data(): done
2G idx[4] w0a0[0xfeca] w1a0[0x14a4] w2a0[0xfb21<FF><FF>]
Update [0xfeca] to [0xfeca]
Update [0x14a4] to [0x14a4]
Update [0xfb21<FF><FF>] to [0xfb21]
Using default 2G PA1 value
cmd=[misc -t get_pa5gha0idxval -w 3 ]
type = [get_pa5gha0idxval]
ctmisc_ioctl: cmd=0x38, buffer size=196
get_data(): cmd=data_init(): base = 0xbc03f4c0
0x38 count=8 lendata_init(): location = [1], mydatas index = 1
=24
ctmisc_ioctl: index=1
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get PA5GHA0IDXVAL count is [1]
get_data(): PA5GHA0IDXVAL 0: [6 0xfea6 0x134c 0xfb34<FF><FF>]
get_data(): done
cmd=[misc -t get_pa5gha1idxval -w 3 ]
type = [get_pa5gha1idxval]
ctmisc_ioctl: cmd=0x3a, buffer size=196
get_data(): cmd=data_init(): base = 0xbc03f400
0x3a count=8 lendata_init(): location = [1], mydatas index = 1
=24
ctmisc_ioctl: index=1
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get PA5GHA1IDXVAL count is [1]
get_data(): PA5GHA1IDXVAL 0: [6 0xfeac 0x12da 0xfb54<FF><FF>]
get_data(): done
5G high idx[6] w0a0[0xfea6] w1a0[0x134c] w2a0[0xfb34<FF><FF>]
Update [0xfea6] to [0xfea6]
Update [0x134c] to [0x134c]
Update [0xfb34<FF><FF>] to [0xfb34]
5G high idx[6] w0a1[0xfeac] w1a1[0x12da] w2a1[0xfb54<FF><FF>]
Update [0xfeac] to [0xfeac]
Update [0x12da] to [0x12da]
Update [0xfb54<FF><FF>] to [0xfb54]
cmd=[misc -t get_pa5gla0idxval -w 3 ]
type = [get_pa5gla0idxval]
ctmisc_ioctl: cmd=0x48, buffer size=196
get_data(): cmd=data_init(): base = 0xbc03f2c0
0x48 count=8 lendata_init(): location = [0], mydatas index = 0
=24
ctmisc_ioctl: index=0
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get PA5GLA0IDXVAL count is [0]
get_data(): done
cmd=[misc -t get_pa5gla1idxval -w 3 ]
type = [get_pa5gla1idxval]
ctmisc_ioctl: cmd=0x4a, buffer size=196
get_data(): cmd=data_init(): base = 0xbc03f200
0x4a count=8 lendata_init(): location = [0], mydatas index = 0
=24
ctmisc_ioctl: index=0
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get PA5GLA1IDXVAL count is [0]
get_data(): done
Using default 5G low PA0 value
Using default 5G low PA1 value
cmd=[misc -t get_pa5ga0idxval -w 3 ]
type = [get_pa5ga0idxval]
ctmisc_ioctl: cmd=0x58, buffer size=196
get_data(): cmd=data_init(): base = 0xbc03f0c0
0x58 count=8 lendata_init(): location = [1], mydatas index = 1
=24
ctmisc_ioctl: index=1
tallest:=====(ctmisc ioctl done...)=====
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get PA5GA0IDXVAL count is [1]
get_data(): PA5GA0IDXVAL 0: [4 0xfead 0x134f 0xfb43<FF><FF>]
get_data(): done
cmd=[misc -t get_pa5ga1idxval -w 3 ]
type = [get_pa5ga1idxval]
ctmisc_ioctl: cmd=0x5a, buffer size=196
get_data(): cmd=data_init(): base = 0xbc03f000
0x5a count=8 lendata_init(): location = [0], mydatas index = 0
=24
ctmisc_ioctl: index=0
tallest:=====(ctmisc ioctl done...)=====
get_data(): Get PA5GA1IDXVAL count is [0]
get_data(): done
5G middle idx[4] w0a0[0xfead] w1a0[0x134f] w2a0[0xfb43<FF><FF>]
Update [0xfead] to [0xfead]
Update [0x134f] to [0x134f]
Update [0xfb43<FF><FF>] to [0xfb43]
Using default 5G middle PA1 value
Restoring Storage Nvram Defaults
enter main_loop()..
find "lang" in MSQUASHFS error: Can't find a SQUASHFS superblock on mtdblock(31,3)
TD 3 (/dev/mtdblock/3)
ret = -1
www -> /www
mount: No such file or directory
cmd=[touch /tmp/var/lib/nfs/xtab ]
cmd=[chmod 644 /tmp/var/lib/nfs/xtab ]
cmd=[touch /tmp/var/lib/nfs/etab ]
cmd=[chmod 644 /tmp/var/lib/nfs/etab ]
cmd=[touch /tmp/var/lib/nfs/rmtab ]
cmd=[chmod 644 /tmp/var/lib/nfs/rmtab ]
cmd=[touch /tmp/var/lib/nfs/state ]
cmd=[chmod go-rwx /tmp/var/lib/nfs/state ]
cmd=[touch /tmp/disk_updating_lock ]
cmd=[chmod 644 /tmp/disk_updating_lock ]
cmd=[touch /tmp/file_variable_updating_lock ]
cmd=[chmod 644 /tmp/file_variable_updating_lock ]
cmd=[insmod /lib/ufsd_cbtn.o ]
Using /lib/ufsd_cbtn.o
modules[0]=bcm57xx buf=[bcm57xx ]
modules[1]=wl buf=[bcm57xx wl ]
Needed modules: bcm57xx wl
cmd=[insmod bcm57xx ]
insmod: bcm57xx.o: no module by that name found
cmd=[insmod wl ]
Using /lib/modules/2.4.20/kernel/drivers/net/wl/wl.o
Error reading /proc/partitions: invalid header.
Failed trying to open `/proc/mdstat': No such file or directory
Failed trying to open `/proc/sestat': No such file or directory
Error reading /proc/partitions: invalid header.
Failed trying to open `/proc/mdstat': No such file or directory
Failed trying to open `/proc/sestat': No such file or directory
Hit enter to continue...The chipset is BCM4705 + BCM5397 for EWC

Make date==>Year:8,Month:6,Day:2
Firmware version =>v1.00.00
sum3=7435264 garbage=992 n=1024
MD5=[461cebecdf6b0d453ed225d2030de824]
cmd=[killall httpd ]
killall: httpd: no process killed
cmd=[killall httpd ]
killall: httpd: no process killed
cmd=[resetbutton ]
WARNING: console log level set to 1
cmd=[vconfig set_name_type VLAN_PLUS_VID_NO_PAD ]
cmd=[vconfig add eth0 1 ]
cmd=[vconfig set_ingress_map vlan1 0 0 ]
cmd=[vconfig set_ingress_map vlan1 1 1 ]
cmd=[vconfig set_ingress_map vlan1 2 2 ]
cmd=[vconfig set_ingress_map vlan1 3 3 ]
cmd=[vconfig set_ingress_map vlan1 4 4 ]
cmd=[vconfig set_ingress_map vlan1 5 5 ]
cmd=[vconfig set_ingress_map vlan1 6 6 ]
cmd=[vconfig set_ingress_map vlan1 7 7 ]
cmd=[vconfig add eth0 2 ]
cmd=[vconfig set_ingress_map vlan2 0 0 ]
cmd=[vconfig set_ingress_map vlan2 1 1 ]
cmd=[vconfig set_ingress_map vlan2 2 2 ]
cmd=[vconfig set_ingress_map vlan2 3 3 ]
cmd=[vconfig set_ingress_map vlan2 4 4 ]
cmd=[vconfig set_ingress_map vlan2 5 5 ]
cmd=[vconfig set_ingress_map vlan2 6 6 ]
cmd=[vconfig set_ingress_map vlan2 7 7 ]
cmd=[brctl addbr br0 ]
cmd=[brctl setfd br0 0 ]
cmd=[brctl stp br0 dis ]
name=[vlan1] lan_ifname=[br0]
cmd=[brctl addif br0 vlan1 ]
br0: No such file or directory
cmd=[wlconf vlan1 up ]
vlan1: Operation not supported
name=[eth1] lan_ifname=[br0]
Write wireless mac successfully
cmd=[brctl addif br0 eth1 ]
br0: No such file or directory
cmd=[wlconf eth1 up ]
eth1: Numerical result out of range
eth1: Operation not supported
eth1: Invalid argument
eth1: Invalid argument
eth1: Operation not supported
eth1: Operation not supported
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
eth1: Invalid argument
cmd=[brctl addif br0 eth1 ]
device eth1 is already a member of a bridge; can't enslave it to bridge br0.
name=[eth2] lan_ifname=[br0]
Write wireless mac successfully
cmd=[brctl addif br0 eth2 ]
br0: No such file or directory
cmd=[wlconf eth2 up ]
eth2: Numerical result out of range
eth2: Operation not supported
eth2: Operation not supported
eth2: Operation not supported
cmd=[brctl addif br0 eth2 ]
device eth2 is already a member of a bridge; can't enslave it to bridge br0.
name=[eth3] lan_ifname=[br0]
cmd=[brctl addif br0 eth3 ]
interface eth3 does not exist!
eth3: No such device
module:  wl                  c0068000  1172704   2
cmd=[wl gpiotimerval 0x640000 ]
wl: No such file or directory
cmd=[wl vlan_mode 0 ]
wl: No such file or directory
lo: File exists
Set 66560 to /proc/sys/net/core/rmem_max ...
cmd=[tftpd -s /tmp -c -l -P 610N ]
cmd=[cron ]
The boot is CFE
tftp server started
tftpd: standalone socket
cron: No such file or directory
cron: created
[HTTPD Starting on /www]
cmd=[httpd ]
cmd=[dnsmasq -h -i br0 -r /tmp/resolv.conf ]
br0 192.168.1.100  86400
cmd=[udhcpd /tmp/udhcpd.conf ]
info, udhcp server (v0.9.8) started
cmd=[/bin/wsccmd ]
cmd=[nas /tmp/nas.lan.conf /tmp/nas.lan.pid lan ]
zebra disabled.
No disk, do not start samba.
cmd=[killall twonkymedia ]
killall: twonkymedia: no process killed
cmd=[killall twonkymediaserv ]
killall: twonkymediaserv: no process killed
cmd=[killall twonkymediaserver ]
eth1: ignore i/f due to error(s)
eth2: ignore i/f due to error(s)
killall: twonkymediaserver: no process killed
cmd=[upnp -D -L br0 -W  -S 0 -I 60 -A 180 ]
*********************************************
Wi-Fi Simple Config Application - Intel Corp.
Version: Build 1.0.5, November 19 2006
*********************************************
calling upnp_main
lltd:echo WRT610N > /proc/sys/kernel/hostname
Initializing stack...button monitor start...!
 OK
Now starting stack
LLTD: wireless interface argument is eth1.
get mac = 00 21 29 C6 61 02
Using /bin/eghn_kernel_module.o
IP is 101a8c0
mask is ffffff
cmd=[tc qdisc del dev vlan1 root ]
RTNETLINK answers: No such file or directory
cmd=[tc qdisc del dev eth1 root ]
RTNETLINK answers: No such file or directory
Initializing UPnP Sdk with
         ipaddress = (null) port = 0
UPnP Initialized
         ipaddress= 192.168.1.1 port = 49152
Specifying the webserver root directory -- /var/EGHN_QPH
Registering the RootDevice
         with desc_doc_url: http://192.168.1.1:49152/qphdevicedesc.xml
RootDevice Registered
Initializing State Table
Found service: urn:schemas-upnp-org:service:QosPolicyHolder:2
serviceId: urn:upnp-org:serviceId:QosPolicyHolder-1
State Table Initialized
cmd=[tc qdisc del dev eth2 root ]
RTNETLINK answers: No such file or directory
cmd=[sh /tmp/cmdfile134 ]
Advertisements Sent
cmd=[dhcp6s -c /tmp/dhcp6s.conf br0 ]
Dispay orig configurations
cmd=[cat /tmp/cmdfile134 ]
tc qdisc add dev vlan1 root handle 1: prio bands 8 priomap 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7
tc qdisc add dev vlan1 parent 1:1 handle 10: pfifo limit 20
tc qdisc add dev vlan1 parent 1:2 handle 20: pfifo limit 20
tc qdisc add dev vlan1 parent 1:3 handle 30: pfifo limit 20
tc qdisc add dev vlan1 parent 1:4 handle 40: pfifo limit 20
tc qdisc add dev vlan1 parent 1:5 handle 50: pfifo limit 20
tc qdisc add dev vlan1 parent 1:6 handle 60: pfifo limit 20
tc qdisc add dev vlan1 parent 1:7 handle 70: pfifo limit 20
tc qdisc add dev vlan1 parent 1:8 handle 80: pfifo limit 20
tc filter add dev vlan1 parent 1:0 prio 5 protocol all tcindex mask 0xff shift 0 pass_on
tc filter add dev vlan1 parent 1:0 prio 5 handle 0 tcindex classid 1:6
tc filter add dev vlan1 parent 1:0 prio 5 handle 1 tcindex classid 1:8
tc filter add dev vlan1 parent 1:0 prio 5 handle 2 tcindex classid 1:7
tc filter add dev vlan1 parent 1:0 prio 5 handle 3 tcindex classid 1:5
tc filter add dev vlan1 parent 1:0 prio 5 handle 4 tcindex classid 1:4
tc filter add dev vlan1 parent 1:0 prio 5 handle 5 tcindex classid 1:3
tc filter add dev vlan1 parent 1:0 prio 5 handle 6 tcindex classid 1:2
tc filter add dev vlan1 parent 1:0 prio 5 handle 7 tcindex classid 1:1
cmd=[sh /tmp/cmdfile134 ]
cmd=[killall -1 radvd ]
DEVICE PIN: 86328432
UdpLib: Entered udp_open
UdpLib: Socket open successful, sd: 9
UdpLib: Entered udp_bind
UdpLib: Binding successful for socket [9]
UdpLib: Entered udp_read

******* MODE: Access Point *******

DEVICE PIN:86328432
WSC: In unconfiged AP mode, wait for start command....
tlvPtrChar* : func CMasterControl_InitiateRegistration  line 658 allocating memory 0x10003700 for 0x100036e8
UdpLib: Entered udp_open
UdpLib: Socket open successful, sd: 10
UdpLib: Entered udp_bind
UdpLib: Binding successful for socket [10]
UdpLib: Entered udp_read
UdpLib: Entered udp_read
killall: radvd: no process killed
Waiting for Registrar to connect...
tallest:=====( wan_or_lan=wan )=====
tallest:=====( wan_or_lan=wan is wan !!)=====
cmd=[udhcpc -i vlan2 -l br0 -p /var/run/wan_udhcpc.pid -s /tmp/udhcpc ]
info, udhcp client (v0.9.8) started
cmd=[nas /tmp/nas.wan.conf /tmp/nas.wan.pid wan ]
cmd=[dhcp6s -c /tmp/dhcp6s.conf br0 ]
Hit enter to continue...No interface specified. Quitting...
Hit enter to continue...Hit enter to continue...cmd=[sh /tmp/cmdfile134 ]
Hit enter to continue...
}}}
