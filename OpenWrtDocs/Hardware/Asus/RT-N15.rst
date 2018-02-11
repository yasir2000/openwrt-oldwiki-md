= Asus RT-N15 =

{{{
CPU: Ralink RT2880
RAM: 32MB
Flash: 8MB
Wi-Fi: Ralink RT2860 (built-in)
Switch: Realtek RTL8366SR
}}}

== Serial console ==

Serial console is available on J1, pinout as follow :

{{{
RT2880 [x] GND
       [x] TX
       [x] RX
       [x] VCC
}}}

== Bootlog ==

{{{

U-Boot 1.1.3 (Jun 10 2008 - 13:49:10)

Board: RT2880 DRAM:  32 MB

 twe0 set to <NULL>

 toe0 set to <NULL>
flash_protect ON: from 0xBC400000 to 0xBC4290D7
protect on 0
protect on 1
protect on 2
flash_protect ON: from 0xBC430000 to 0xBC43FFFF
protect on 3
*** Warning - bad CRC, using default environment



============================================
ASIC -VerB/C (MAC to MAC Mode)
DRAM COMPONENT=128Mbits
DRAM BUS=32BIT
Total memory = 32Mbytes
Date:Jun 10 2008  Time:13:49:10
============================================
 D-CACHE set to 4 way
 I-CACHE set to 4 way

 ##### The CPU freq = 266 MHZ ####

 SDRAM bus set to 32 bit
 SDRAM size =32 Mbytes
operation>
 0
uboot menu

rt2880_eth_initialize

_start init rtl8366s
[V2]InitChip 8366s

# uboot: set mdio reg value as 3f014a45
InitChip 8366s done

 eth_register
Eth0 (10/100-M)

 eth_current->name = Eth0 (10/100-M)

## Booting image at bc450000 ...
   Image Name:
   Created:      2008-09-15  11:31:08 UTC
System Control Status = 0x02910084
   Image Type:   MIPS Linux Kernel Image (lzma compressed)
   Data Size:    2566980 Bytes =  2.4 MB
   Load Address: 8a000000
   Entry Point:  8a19e040
   Verifying Checksum ... OK

3: System Boot system code via Flash.
## Booting image at bc450000 ...
   Image Name:
   Created:      2008-09-15  11:31:08 UTC
System Control Status = 0x02910084
   Image Type:   MIPS Linux Kernel Image (lzma compressed)
   Data Size:    2566980 Bytes =  2.4 MB
   Load Address: 8a000000
   Entry Point:  8a19e040
   Verifying Checksum ... OK
   Uncompressing Kernel Image ... OK
No initrd
## Transferring control to Linux (at address 8a19e040) ...
## Giving linux memsize in MB, 32

Starting kernel ...


THIS IS ASIC - VERSION B
ramsize = 32 MBytes
rambase not set, set to default (0x08000000)
MEMORY DESCRIPTOR dump:
[0,8a3f8090]: base<0a000000> size<02000000> type<Free RAM memory>

 The CPU feqenuce set to 266 MHz
CPU revision is: 0001906c
icache: sets:256, ways:4, linesz:16 ,total:16384, waybit:12, flags:0x0
dcache: sets:256, ways:4, linesz:16 ,total:16384, waybit:12, flags:0x0
i waysize = 4096, d waysize = 4096, i sets= 256, d sets=256
Primary instruction cache 16kB, physically tagged, 4-way, linesize 16 bytes.
Primary data cache 16kB, 4-way, linesize 16 bytes.
[setup_scache]:1043
Linux version 2.4.30 (root@localhost.localdomain) (gcc version 3.3.6) #1698 äž 9æ 15 19:31:00 CST 2008
Determined physical RAM map:
 memory: 02000000 @ 0a000000 (usable)
Initial ramdisk at: 0x8a1d7000 (1949696 bytes)
On node 0 totalpages: 49152
zone(0): 49152 pages.
zone(1): 0 pages.
zone(2): 0 pages.
Kernel command line: console=ttyS1,57600n8 root=/dev/ram0
cause = 50c08100, status = 1000fc00
calculating r4koff... 0028b0aa(2666666)
CPU frequency 266.67 MHz
Using 133.333 MHz high precision timer.
Calibrating delay loop... 266.24 BogoMIPS
Memory: 26568k/32768k available (1641k kernel code, 6200k reserved, 2024k data, 100k init, 0k highmem)
Dentry cache hash table entries: 32768 (order: 6, 262144 bytes)
Inode cache hash table entries: 16384 (order: 5, 131072 bytes)
Mount cache hash table entries: 512 (order: 0, 4096 bytes)
Buffer cache hash table entries: 16384 (order: 4, 65536 bytes)
Page-cache hash table entries: 65536 (order: 6, 262144 bytes)
Checking for 'wait' instruction...  unavailable.
POSIX conformance testing by UNIFIX
pci.c pcibios_init():817
BAR0 at slot 0 = 8
pci.c pcibios_fixup_resources():638
dev= 0x8a61cc00
Linux NET4.0 for Linux 2.4
Based upon Swansea University Computer Society NET3.039
Initializing RT netlink socket
Starting kswapd
devfs: v1.12c (20020818) Richard Gooch (rgooch@atnf.csiro.au)
devfs: boot_options: 0x1
squashfs: version 3.1 (2006/08/19) Phillip Lougher
pty: 256 Unix98 ptys configured
Ralink RT2880 gpio driver initialized
spidrv_major = 217
i2cdrv_major = 218

CONFIG devfs

@@ start init rtl8366s asic with qos

InitChip 8366s

@@@@ MDIO_CFG reg valeu is 3f014a45

rtl8366s initialized
Serial driver version 5.05c (2001-07-08) with MANY_PORTS SHARE_IRQ SERIAL_PCI enabled
ttyS00 at 0x300500 (irq = 9) is a 16550A
ttyS01 at 0x300c00 (irq = 8) is a 16550A
HDLC line discipline: version $Revision: 1.1.1.1 $, maxframe=4096
N_HDLC line discipline registered.
RAMDISK driver initialized: 16 RAM disks of 8192K size 1024 blocksize

##[RAETHx 1: module init]## RA2880 Ethernet Driver Initilization. v1.01  256 rx/tx descriptors allocated!
dev_raether irq is 3(eth2)

ether setup [eth2]
Netlink init ok!
PROC INIT OK!
PPP generic driver version 2.4.2
PPP Deflate Compression module registered
PPP BSD Compression module registered
PPPoL2TP kernel driver, V0.13 (oleg@cs.msu.su)
FLASH_API: MAN_ID=C2 DEV_ID=22A8 SIZE=4MB
physmap flash device: 800000 at 1c400000
 Amd/Fujitsu Extended Query Table v1.1 at 0x0040
number of CFI chips: 1
cfi_cmdset_0002: Disabling fast programming due to code brokenness.
Using physmap partition definition
Creating 4 MTD partitions on "RT2880 SOC Physically mapped flash":
0x00000000-0x00030000 : "Bootloader"
mtd: Giving out device 0 to Bootloader
0x00030000-0x00040000 : "Config "
mtd: Giving out device 1 to Config
0x00040000-0x00050000 : "Factory"
mtd: Giving out device 2 to Factory
0x00050000-0x00400000 : "Kernel"
mtd: Giving out device 3 to Kernel
NET4: Linux TCP/IP 1.0 for NET4.0
IP Protocols: ICMP, UDP, TCP, IGMP
IP: routing cache hash table of 2048 buckets, 16Kbytes
TCP: Hash tables configured (established 16384 bind 32768)
Linux IP multicast router 0.06 plus PIM-SM
ip_conntrack version 2.1 (1536 buckets, 12288 max) - 308 bytes per conntrack
ip_tables: (C) 2000-2002 Netfilter core team, Type=Restricted Cone
NET4: Unix domain sockets 1.0/SMP for Linux NET4.0.
NET4: Ethernet Bridge 008 for NET4.0
802.1Q VLAN Support v1.8 Ben Greear <greearb@candelatech.com>
All bugs added by David S. Miller <davem@redhat.com>
RAMDISK: Compressed image found at block 0
Freeing initrd memory: 1904k freed
VFS: Mounted root (squashfs filesystem) readonly.
Mounted devfs on /dev
Freeing prom memory: 0kb freed
Freeing unused kernel memory: 100k freed
console started
trying to start /sbin/init
Algorithmics/MIPS FPU Emulator v1.5
2860 version : 1.8.0.0 (Sep 15 2008)

ether setup []


=== pAd = c00f3000, size = 400352 ===

ps2880_major = 244


@@ 20080401.1

chk mac
002215F9E159

chk mac: [00](0)
002215F9E159
002215F9E159

##[RAETHx 3]## ei open eth2
(MAC2MAC)MDIO_CFG = 1f01dc01
GDMA1_FWD_CFG = 10000

phy_tx_ring = 0bdf6000, tx_ring = abdf6000, size: 16 bytes

phy_rx_ring = 0bdf5000, rx_ring = abdf5000, size: 16 bytes
RX DESC aa232000  size = 2048
1. Phy Mode = 9
2. Phy Mode = 9
3. Phy Mode = 9
MCS Set = ff ff 00 00 01
Main bssid = 00:22:15:f9:e1:59
The UUID Hex string is:2880288028801880a880002215f9e159
The UUID ASCII string is:28802880-2880-1880-a880-002215f9e159!

ether setup []

ether setup [wds0]

ether setup []

ether setup [wds1]

ether setup []

ether setup [wds2]

ether setup []

ether setup [wds3]
0x1300 = 00064380
set eth2.2 hw addr as(cur) 00:22:15:F9:E1:59
 00 22 15 f9 e1 59.
VLAN (eth2.2):  Underlying device (eth2) has same MAC, not checking promiscious mode.

ether setup [br0]
device ra0 entered promiscuous mode
eth2.1: dev_set_promiscuity(master, 1)
device eth2 entered promiscuous mode
device eth2.1 entered promiscuous mode
eth2.1: attempt to add interface with same source address.
br0: port 2(eth2.1) entering listening state
br0: port 1(ra0) entering listening state
br0: port 2(eth2.1) entering learning state
br0: port 1(ra0) entering learning state
br0: port 2(eth2.1) entering forwarding state
br0: topology change detected, propagating
br0: port 1(ra0) entering forwarding state
br0: topology change detected, propagating
## start wan
bcmp cur_hwaddr, ifr.sa_data not match
rc start service

rc start upnp br0/eth2.2

G enable GRENN Ethernet
set green ethernet (tv=3) green=2, powersav=1
start wsc
WPS: PIN


BusyBox v1.4.2 (2008-09-15 19:26:10 CST) Built-in shell (ash)
Enter 'help' for a list of built-in commands.

/ #
}}}
