[[TableOfContents]]

= Status =
currently a bootlog can be pulled. kernel source for 2.4 is available and is being forward ported to 2.6

= prereq =
first the amazon port needs to be complete the todo is here http://wiki.openwrt.org/OpenWrtDocs/Hardware/Infineon/Amazon

= Things we already know =
== original firmware ==
 * original firmware from t-com --> [http://www.t-home.de/is-bin/INTERSHOP.enfinity/WFS/EKI-PK-Site/de%20DE/-/EUR/ViewFAQTheme-Download;sid=McR43bq5YDR43PzBEJ9yRZuXxxFB7ru8uPJEuBo%20h5CDLpty2x4=?ProductThemeId=theme-1000&selaction=themen&FaqId=theme-2001628&pageNr=0&bound=3&itemLocator=Bedienungsanleitungen&headerSelection=2&SelectedTheme=theme-2000178&SelectedTheme=theme-2001628&SelectedTheme=theme-6512161 link]
 * some  informations about the firmware --> [http://www.kessler-design.com/speedport-w700v/firmware.html link]
== Hardware ==
 * * Router is build by Arcadyan for Siemens and they resell it to t-com
  * http://www.arcadyan.com/Downloads/downloads.htm  (you can find some source code here :-D)
 * Infineon amazon ADSL 2+ chipset with
  * MIPS32® 4KEc® Hard IP Core, CPU (u.a. OpenSSL 0.9.6a Crypto library, SSL/TLS Library und CCL/ITRI VoIP Middleware sip, sipTX, UACore)
  * Infineon ADM6996 'Samurai' 5+1 Port 10/100 Switch
  * eon en29lv3208 flash
 * [http://www.atheros.com/pt/AR5002XBulletin.htm Atheros AR5212] Multiprotocol MAC/baseband processor, WLAN CPU
 * [http://www.atheros.com/pt/AR5002XBulletin.htm Atheros AR5112] Dual band Radio-on-a-Chip (RoC), WLAN
 * [http://www.atheros.com/pt/AR5005G.htm Atheros AR2413] Single-Chip  CMOS MAC/Baseband/Radio, WLAN
 * [http://www.infineon.com/cgi-bin/ifx/portal/ep/channelView.do?channelId=-86154&pageTypeId=17099 Infineon VINETIC®-2CPE] Voice Processor / Analog Termination Chipsat
 * [http://www.infineon.com/cgi-bin/ifx/portal/ep/channelView.do?channelId=-88949&pageTypeId=17099 Infineon SLIC-DC] Ringing SLIC with Integrated DC/DC Convertor
 * [http://www.infineon.com/cgi-bin/ifx/portal/ep/channelView.do?channelId=-65153&pageTypeId=17099 Infineon ISDN ST Transceiver] on a single chip
 * more Information in german  available hhttp://www.kessler-design.com/speedport-w700v/hardware.htmlere
== Serial Port ==
image coming soon in the meantime look for pictures of the serialport [http://www.kessler-design.com/speedport-w700v/hardware.html here]["http"]

== boot loader ==
the bootloader is a brn loader --> http://ar7-firmware.berlios.de/doc/loader.html.en

== Bootlog ==
{{{
Version 1.0.1
Read EEPROM
Jump to Flash
=======================================================================
 Wireless ADSL Gateway AMAZON Loader V0.93.0 build Nov 10 2006 17:16:22
                    Broad Net Technology, INC.
=======================================================================
EON EN29LV320B bottom boot 16-bit mode found
[GPIO FLOW] SetGpio() Begin ..
[GPIO FLOW] SetGpio() End.
Copying boot params.....DONE
Press Space Bar 3 times to enter command mode ...
Flash Checking [1]  Passed.
Unzipping firmware [1(3)] at 0x80002000 ... [ZIP 1]  done
In c_entry() function ...
CPU : Yangtse Version
Chip V1.3, CPU Speed 235 MHz, FPI Speed 117 MHz
install_exception
[INIT] Interrupt ...
[GPIO FLOW] SetGpio() Begin ..
[GPIO FLOW] SetGpio() End.
##### _ftext      = 0x80002000
##### _fdata      = 0x8047FC40
##### __bss_start = 0x804EF8E4
##### end         = 0x812AB660
##### Backup Data from 0x8047FC40 to 0x812B3660~0x81323304 len 457892
##### Backup Data completed
##### Backup Data verified
[INIT] System Log Pool startup ...
[INIT] MTinitialize ..
[INIT] usrclk
mips_counter_frequency:117500000
r4k_offset: 0001cafc(117500)
init_US_counter : time1 = 183585 , time2 = 32183613, diff 32000028
US_counter = 59
 cnt1 32694569 cnt2 32695803, diff 1234
Runtime code version: 1.22.000
System startup...
[INIT] Memory COLOR 0, 1500000 bytes ..
[INIT] Memory COLOR 1, 1048576 bytes ..
[INIT] Memory COLOR 2, 3227776 bytes ..
InitCommSys: RESOURCE_BASE = 27, NUMRES = 640
InitCommSys: EVENT_BASE = 131, NUMEVT = 768
InitCommSys: MAILBOX_BASE = 6, NUMMBX = 64
EON EN29LV320B bottom boot 16-bit mode found
Set flash memory layout to Boot Parameters found !!!
Bootcode version: V0.93.0
Serial number: J634513733
Hardware version: 01B
sizeof(struct III_Config_t) is 186032
EON EN29LV320B bottom boot 16-bit mode found
====xxx==============================-->425@-230;*(.4/.4/1,.4/3/1)
-->425@-230;30(.1/.25/1,.1/3/1)
-->600(.9/4.8)
-->600(.3/.5,.3/3)
-->600(.9/3.9)
GPIO15 = 1
check_WAN_switch returns 1
GPIO15 = 1
Enable interface 26
default route: 0.0.0.0
BufferInit:
BUF_HDR_SZ=64 BUF_ALIGN_SZ=12 BUFFER_OFFSET=128
BUF_BUFSZ0=384 BUF_BUFSZ1=2176
NUM_OF_B0=0 NUM_OF_B1=1000
BUF_POOL0_SZ=0 BUF_POOL1_SZ=2240000
sizeof(BUFFER0)=448,sizeof(BUFFER1)=2240
*BUF0=0x80ea8bec *BUF1=0x80c85ddc
Altgn *BUF0=0x80ea8bf0 *BUF1=0x80c85de0
End at BUF0:0x80ea8bf0, BUF1:0x80ea8be0
BUF0[0]=0x80ea8bf0 BUF1[0]=0x80c85de0
buffer0 pointer init OK!
buffer1 pointer init OK!
[qm_lnk_init] CLOCKHZ=1000 ...
[qm_cbq_enable] no QM attached
[qm_cbq_detach] no QM is attached at link 0
f=4214967298/100000, ns_per_byte=1086556160/100000
New cls: id=0, bw=799 ns/byte, maxd=0 ms,
         maxb=32, minb=2, avgpktsz=250, maxpktsz=1604,
         offtime=362, parent=0, borrow=0
         pri=0, maxidle=113, minidle=-2566,
         maxq=96, clsfg=17
f=3896529814/100000, ns_per_byte=1079574528/100000
New cls: id=1, bw=79999 ns/byte, maxd=0 ms,
         maxb=16, minb=1, avgpktsz=1604, maxpktsz=1604,
         offtime=128191, parent=812aa578, borrow=812aa578
         pri=7, maxidle=34157, minidle=-256639,
         maxq=48, clsfg=21
qm_cbq_attach(): cbqp->cbq_res=22
f=1157355966/100000, ns_per_byte=1086543359/100000
New cls: id=2, bw=808 ns/byte, maxd=0 ms,
         maxb=32, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=745, parent=812aa578, borrow=812aa578
         pri=0, maxidle=114, minidle=-2592,
         maxq=96, clsfg=21
f=3892314111/100000, ns_per_byte=1072693248/100000
[qm_cbq_newcls] warning: bandwidth of the class may be low enough to cause INT overflow
New cls: id=3, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=0, maxidle=0, minidle=-4294966,
         maxq=96, clsfg=21
f=3892314111/100000, ns_per_byte=1072693248/100000
[qm_cbq_newcls] warning: bandwidth of the class may be low enough to cause INT overflow
New cls: id=4, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=2, maxidle=0, minidle=-4294966,
         maxq=48, clsfg=23
f=3892314111/100000, ns_per_byte=1072693248/100000
[qm_cbq_newcls] warning: bandwidth of the class may be low enough to cause INT overflow
New cls: id=5, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=3, maxidle=0, minidle=-4294966,
         maxq=48, clsfg=23
f=3892314111/100000, ns_per_byte=1072693248/100000
[qm_cbq_newcls] warning: bandwidth of the class may be low enough to cause INT overflow
New cls: id=6, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=4, maxidle=0, minidle=-4294966,
         maxq=48, clsfg=23
f=3892314111/100000, ns_per_byte=1072693248/100000
[qm_cbq_newcls] warning: bandwidth of the class may be low enough to cause INT overflow
New cls: id=7, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=5, maxidle=0, minidle=-4294966,
         maxq=48, clsfg=23
f=1157355966/100000, ns_per_byte=1086543359/100000
New cls: id=8, bw=808 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=745, parent=812a8f78, borrow=812a8f78
         pri=7, maxidle=48, minidle=-2592,
         maxq=48, clsfg=20
CLOCKHZ=1000
gConfig.Interface[0].IP_Addr = 127.0.0.1
gConfig.Interface[0].Subnet_Mask = 255.255.255.255
time = 08/01/2003, 00:00:00
TRAP(linkUp) : send ok!
Interface 0 ip = 127.0.0.1
gConfig.Interface[1].IP_Addr = 192.168.2.1
gConfig.Interface[1].Subnet_Mask = 255.255.255.0
vlan=1, vid=1, port_mask=0x1e
switch_init ...
Reg 0xA0: 0x1023 0xA1:0x7 0x12:0x3602
mac_0_init: interface 1 registered to VLAN 1
MAC Address: 00:12:bf:b3:b2:2d
time = 08/01/2003, 00:00:00
TRAP(linkUp) : send ok!
Interface 1 ip = 192.168.2.1
gConfig.Interface[2].IP_Addr = 0.0.0.0
gConfig.Interface[2].Subnet_Mask = 0.0.0.0
PCI: Probing PCI hardware on host bus 0.
*(AMAZON_RST_REQ)=0
*(AMAZON_RST_REQ+4)=fffc0002
*(AMAZON_PMU_PWDCR)=3fff
IRM 1=0x0
IRM 2=0x18000000
CLOCK_CONTROL=0x2
FPI_SEGMENT_ENABLE=0x3ff
PCI_MODE=0x1000103
  1a168c
  2900000
  2000001
        0
        0
        0
        0
        0
       0
        0
     5001
 44131113
        0
       44
        0
 1c0a0100
Autoconfig PCI channel 0x804DF268
Scanning bus 0, I/O 0xb2400000:0xb2600001, Mem 0xb2000000:0xb2400001
201
203
pci_devfn=0x70
vid=0x168c
201
203
00:0e.0 Class 0200: 168c:001a (rev 01)
        Mem at 0xb2000000 [size=0x10000]
108
Scanning bus 00
Found 00:70 [168c/001a] 000200 00
201
203
201
203
Fixups for bus 00
Bus scan for 00 returning with max=00
  1a168c  2900006  2000001     8000 b2000000        0        0        0
       0        0     5001 44131113        0       44        0 1c0a014e
[HWLAN] ifno=2 irno=7 port=0x00000000
[PCI] devtag=00000070 probe=8005c384
[HWLAN] devtag = 00000070
[HWLAN] Vendor ID 0x168c
[HWLAN] Device ID 0x1a
[HWLAN] Base Addr 0xb2000000
[HWLAN] SVendor ID 0x1113
[HWLAN] SDevice ID 0x4413
[HWLAN] Revision ID 0x1
[HWLAN] interrupt vector 0x1
201
203
201
203
PCI: Enabling device 00:0e.0 (0006 -> 0006)
apCfgRadioDefaultSet> gSetting.channel=0, pRadio->channel=5260, pRadio->freqSpec=1
####ming#### : WPA2-PSK ..
####ming#### : ENCRYPTION_AES_CCM ..
[HWLAN] pRadio->abolt = 00000000
apCfgRadioDefaultSet> gSetting.channel=0, pRadio->channel=2462, pRadio->freqSpec=8
[HWLAN] pRadio->abolt = 00000010
[HWLAN] gSetting.BasicRate=f
[HWLAN] apCfgDefaultSet : prepare to set WDS..
apInit: Initialize Access Point.
[HWLAN] ar5hwcCreatePhy : ifno:2 pdevInfo=807c9804, devno=1
[HWLAN] devno 1 pdevInfo 807c9804
[HWLAN] Base address = b2000000, irq 1
Attach AR5212 0x1a 0x807c9804
[HWLAN] DOMAIN 00008114
ar5212GetMacAddr: EEPROM MAC is all 0's or all F's
[HWLAN] Set HWLAN MAC as LAN MAC ..
[HWLAN] MAC Address=00-12-BF=B3-B2-2F
[HWLAN] wlan1 revisions: mac 7.8 phy 4.5 analog 5.6 eeprom 5.2
[apCfgRadioCheck] nRadio=1
[apCfgRadioCheck] wmode=12, pRadio1->freqSpec=8
[ar5hwcCreatePhy]  devno=1, opModes=8
[wlanInitChannelList] wireless_selection=0x8
[HWLAN] country code = 00000114
wlanInitChannelList -- country: DE, mode: 8 11g
[HWLAN] wlanInitChannelList -- 13 channels
[HWLAN]  2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472
[wlanGetChannelPtr] channel=2462, modeSelect=8, pCList->listSize=13[wlanGetChannelPtr] pCList->chanArray[0].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[1].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[2].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[3].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[4].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[5].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[6].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[7].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[8].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[9].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[wlanGetChannelPtr] pCList->chanArray[10].channelFlags=0x5400, modeTable[3].channelFlags=0x1400
[HWLAN] phwChannel 2462, channelFlags 00005400
[HWLAN] size of ATHEROS_DESC hardware part 32
[HWLAN] CACHE_LINE_SIZE 16, AR_DESC_SIZE 128
[HWLAN] AR_HEADER_SIZE 96, AR_BUF_SIZE 1696numDescriptors = 704
[HWLAN] wlan1: pDmaBuf=A04F1890
[HWLAN] pMemBuf a04f1890 pdevInfo->pDmaBuf a04f1890
[HWLAN] ar5hwcQueueCreate: semaphore id 80687a24
[HWLAN] ar5hwcQueueCreate: semaphore id 80687a38
[HWLAN] ar5hwcQueueCreate: semaphore id 80687a4c
[HWLAN] ar5hwcQueueCreate: semaphore id 80687a60
[HWLAN] pMemBuf a050d890, pdevInfo->pDmaBuf + pdevInfo->dmaBufSize a05280b0
[HWLAN] muxDevLoad is called for vportNum 10000, loadfn 8007b840, vportStr 16: 0: 1
OFDM 2X Maxes: MaxRD: 40 TurboMax: 38 MaxCTL: 27 TPC_Reduction 0 Final Scaled Power 27
*** Power Level Debug on channel 2462 mode 0x5400 ***
*** Rates per Power - exact dBm (pre power table adjustment) ***
  6mb OFDM  13.5 dBm | 1L   CCK   16.5 dBm
  9mb OFDM  13.5 dBm | 2L   CCK   16.5 dBm
 12mb OFDM  13.5 dBm | 2S   CCK   16.5 dBm
 18mb OFDM  13.5 dBm | 5.5L CCK   16.5 dBm
 24mb OFDM  13.5 dBm | 5.5S CCK   16.5 dBm
 36mb OFDM  13.5 dBm | 11L  CCK   16.5 dBm
 48mb OFDM  13.5 dBm | 11S  CCK   16.5 dBm
 54mb OFDM  13.5 dBm | XR         13.5 dBm
ch: 2462, minCalPower2413_t2 = 0
dBm  0 -> pdadc  0 | dBm  1 -> pdadc  0
dBm  2 -> pdadc  0 | dBm  3 -> pdadc  0
pdadc[0] = 0x0 [offset = 0xa280]
dBm  4 -> pdadc  0 | dBm  5 -> pdadc  0
dBm  6 -> pdadc  0 | dBm  7 -> pdadc  0
pdadc[1] = 0x0 [offset = 0xa284]
dBm  8 -> pdadc  1 | dBm  9 -> pdadc  1
dBm 10 -> pdadc  2 | dBm 11 -> pdadc  2
pdadc[2] = 0x2020101 [offset = 0xa288]
dBm 12 -> pdadc  3 | dBm 13 -> pdadc  4
dBm 14 -> pdadc  6 | dBm 15 -> pdadc  8
pdadc[3] = 0x8060403 [offset = 0xa28c]
dBm 16 -> pdadc 10 | dBm 17 -> pdadc 12
dBm 18 -> pdadc 12 | dBm 19 -> pdadc 14
pdadc[4] = 0xe0c0c0a [offset = 0xa290]
dBm 20 -> pdadc 16 | dBm 21 -> pdadc 18
dBm 22 -> pdadc 20 | dBm 23 -> pdadc 22
pdadc[5] = 0x16141210 [offset = 0xa294]
dBm 24 -> pdadc 24 | dBm 25 -> pdadc 26
dBm 26 -> pdadc  0 | dBm 27 -> pdadc  0
pdadc[6] = 0x1a18 [offset = 0xa298]
dBm 28 -> pdadc  0 | dBm 29 -> pdadc  0
dBm 30 -> pdadc  1 | dBm 31 -> pdadc  2
pdadc[7] = 0x2010000 [offset = 0xa29c]
dBm 32 -> pdadc  3 | dBm 33 -> pdadc  4
dBm 34 -> pdadc  5 | dBm 35 -> pdadc  5
pdadc[8] = 0x5050403 [offset = 0xa2a0]
dBm 36 -> pdadc  6 | dBm 37 -> pdadc  6
dBm 38 -> pdadc  7 | dBm 39 -> pdadc  7
pdadc[9] = 0x7070606 [offset = 0xa2a4]
dBm 40 -> pdadc  8 | dBm 41 -> pdadc  8
dBm 42 -> pdadc  9 | dBm 43 -> pdadc 10
pdadc[10] = 0xa090808 [offset = 0xa2a8]
dBm 44 -> pdadc 11 | dBm 45 -> pdadc 12
dBm 46 -> pdadc 13 | dBm 47 -> pdadc 15
pdadc[11] = 0xf0d0c0b [offset = 0xa2ac]
dBm 48 -> pdadc 16 | dBm 49 -> pdadc 18
dBm 50 -> pdadc 19 | dBm 51 -> pdadc 21
pdadc[12] = 0x15131210 [offset = 0xa2b0]
dBm 52 -> pdadc 23 | dBm 53 -> pdadc 25
dBm 54 -> pdadc 27 | dBm 55 -> pdadc 29
pdadc[13] = 0x1d1b1917 [offset = 0xa2b4]
dBm 56 -> pdadc 31 | dBm 57 -> pdadc 34
dBm 58 -> pdadc 38 | dBm 59 -> pdadc 42
pdadc[14] = 0x2a26221f [offset = 0xa2b8]
dBm 60 -> pdadc 45 | dBm 61 -> pdadc 49
dBm 62 -> pdadc 49 | dBm 63 -> pdadc 53
pdadc[15] = 0x3531312d [offset = 0xa2bc]
dBm 64 -> pdadc 57 | dBm 65 -> pdadc 61
dBm 66 -> pdadc 65 | dBm 67 -> pdadc 69
pdadc[16] = 0x45413d39 [offset = 0xa2c0]
dBm 68 -> pdadc 73 | dBm 69 -> pdadc 77
dBm 70 -> pdadc 81 | dBm 71 -> pdadc 85
pdadc[17] = 0x55514d49 [offset = 0xa2c4]
dBm 72 -> pdadc 89 | dBm 73 -> pdadc 93
dBm 74 -> pdadc 97 | dBm 75 -> pdadc 101
pdadc[18] = 0x65615d59 [offset = 0xa2c8]
dBm 76 -> pdadc 101 | dBm 77 -> pdadc 101
dBm 78 -> pdadc 101 | dBm 79 -> pdadc 101
pdadc[19] = 0x65656565 [offset = 0xa2cc]
dBm 80 -> pdadc 101 | dBm 81 -> pdadc 101
dBm 82 -> pdadc 101 | dBm 83 -> pdadc 101
pdadc[20] = 0x65656565 [offset = 0xa2d0]
dBm 84 -> pdadc 101 | dBm 85 -> pdadc 101
dBm 86 -> pdadc 101 | dBm 87 -> pdadc 101
pdadc[21] = 0x65656565 [offset = 0xa2d4]
dBm 88 -> pdadc 101 | dBm 89 -> pdadc 101
dBm 90 -> pdadc 101 | dBm 91 -> pdadc 101
pdadc[22] = 0x65656565 [offset = 0xa2d8]
dBm 92 -> pdadc 101 | dBm 93 -> pdadc 101
dBm 94 -> pdadc 101 | dBm 95 -> pdadc 101
pdadc[23] = 0x65656565 [offset = 0xa2dc]
dBm 96 -> pdadc 101 | dBm 97 -> pdadc 101
dBm 98 -> pdadc 101 | dBm 99 -> pdadc 101
pdadc[24] = 0x65656565 [offset = 0xa2e0]
dBm 100 -> pdadc 101 | dBm 101 -> pdadc 101
dBm 102 -> pdadc 101 | dBm 103 -> pdadc 101
pdadc[25] = 0x65656565 [offset = 0xa2e4]
dBm 104 -> pdadc 101 | dBm 105 -> pdadc 101
dBm 106 -> pdadc 101 | dBm 107 -> pdadc 101
pdadc[26] = 0x65656565 [offset = 0xa2e8]
dBm 108 -> pdadc 101 | dBm 109 -> pdadc 101
dBm 110 -> pdadc 101 | dBm 111 -> pdadc 101
pdadc[27] = 0x65656565 [offset = 0xa2ec]
dBm 112 -> pdadc 101 | dBm 113 -> pdadc 101
dBm 114 -> pdadc 101 | dBm 115 -> pdadc 101
pdadc[28] = 0x65656565 [offset = 0xa2f0]
dBm 116 -> pdadc 101 | dBm 117 -> pdadc 101
dBm 118 -> pdadc 101 | dBm 119 -> pdadc 101
pdadc[29] = 0x65656565 [offset = 0xa2f4]
dBm 120 -> pdadc 101 | dBm 121 -> pdadc 101
dBm 122 -> pdadc 101 | dBm 123 -> pdadc 101
pdadc[30] = 0x65656565 [offset = 0xa2f8]
dBm 124 -> pdadc 101 | dBm 125 -> pdadc 101
dBm 126 -> pdadc 101 | dBm 127 -> pdadc 101
pdadc[31] = 0x65656565 [offset = 0xa2fc]
gainBoundaries [16, 46, 46, 46] and gainOverlap : 10
reg 0xa26c = 0xbaeb90a
reg 0xa258 = 0xcc75380 [numPdGans = [15:14], forceDacGain = [29]]
reg 0xa274 = 0x1b7caa [forceTxGain = [0]]
TX Power settings: curve(s) returned minPower 27 (2x dBm), maxPower 33 (2x dBm)
        Power Offset 0 (2x dBm)
XpdGain's [0] 0x3, [1] 0x1
*** Actual power per rate table as programmed ***
  6mb OFDM  13.5 dBm | 1L   CCK   16.5 dBm
  9mb OFDM  13.5 dBm | 2L   CCK   16.5 dBm
 12mb OFDM  13.5 dBm | 2S   CCK   16.5 dBm
 18mb OFDM  13.5 dBm | 5.5L CCK   16.5 dBm
 24mb OFDM  13.5 dBm | 5.5S CCK   16.5 dBm
 36mb OFDM  13.5 dBm | 11L  CCK   16.5 dBm
 48mb OFDM  13.5 dBm | 11S  CCK   16.5 dBm
 54mb OFDM  13.5 dBm | XR         13.5 dBm
[HWLAN] ioctl CMD=0xb
[HWLAN] bridgePortAdd : vp, 10000
[HWLAN] bridgePortAdd (base BSS) succeeded for vp1
should call linkSockAttach()
wlan1 added STA: 00:12:bf:b3:b2:2f (0)
[HWLAN] ifno=2 after call apInit() : .... bg 1 , a 0 ....
time = 08/01/2003, 00:00:00
TRAP(linkUp) : send ok!
[HWLAN] hwlan_ioctl() ..
Interface 2 ip = 192.168.2.1
[HWLAN] hwlan_ioctl() ..
gConfig.Interface[3].IP_Addr = 0.0.0.0
gConfig.Interface[3].Subnet_Mask = 0.0.0.0
Init SAR ifno:3 QID:1 VPI/VCI:1/32
CBM_CELL_SIZE(64) * dev->cbm.free_cell_cnt(16384)=1048576 Addr=0x80b565c0
CBM_QD_SIZE(16) * AMAZON_ATM_MAX_QUEUE_NUM(32)=512 Addr=0x80b56380
amazon_atm_devs[0] = 0x80b562bc
registering device 0
ATM_UBR
vpi 1 vci 32  itf 0 aal 5
tx cl 1 bw 0 mtu 0
rx cl 1 bw 4000 mtu 0
Class=1 QSB_CLK=58750000 MAX_PCR=0 PCR=4000 MIN_PCR=0 SCR=4000 MBS=10 CDV=0
tprs:0 twfq:16383 ts:0 taus:0
MAC Address: 00:12:bf:b3:b2:2e
Interface 3 ip = 0.0.0.0
gConfig.Interface[11].IP_Addr = 0.0.0.0
gConfig.Interface[11].Subnet_Mask = 0.0.0.0
IFLNK_PPPOE init : (Linkp)ifno = 11 idx = 2
IFLNK_PPPOE init : (Driverp)ifno = 11 idx = 3
Interface 11 ip = 0.0.0.0
gConfig.Interface[26].IP_Addr = 0.0.0.0
gConfig.Interface[26].Subnet_Mask = 0.0.0.0
vlan=1, vid=2, port_mask=0x1
mac_0_init: interface 26 registered to VLAN 2
MAC Address: 00:12:bf:b3:b2:2d
time = 08/01/2003, 00:00:00
TRAP(linkUp) : send ok!
iput_IpLinkUp(ifno=26)> ifp->add_default_route:1
Re-Init NAT data structure
Init NAT data structure
Interface 26 ip = 0.0.0.0
ruleCheck()> Group: 0,  Error: Useless rule index will be truncated
ruleCheck()> Group: 1,  Error: Useless rule index will be truncated
ruleCheck()> Group: 2,  Error: Useless rule index will be truncated
CBAC rule format check succeed !!
reqCBACBuf()> init match pool, Have: 1000
Memory Address: 0x8124951c ~ 0x81250298
reqCBACBuf()> init timeGap pool, Have: 10000
Memory Address: 0x81250298 ~ 0x81280fec
reqCBACBuf()> init sameHost pool, Have: 2000
Memory Address: 0x81280fec ~ 0x81290a0c
CBAC rule pool initialized !!
1 set_timer: ti=0 type=1 ina[00000000] gid[00000000]
2 set_timer: ti=0 type=1 ina[00000000] gid[00000000]
3 set_timer: ti=0 type=1 ina[00000000] gid[00000000]
11 set_timer: ti=0 type=1 ina[00000000] gid[00000000]
26 set_timer: ti=0 type=1 ina[00000000] gid[00000000]
Init NAT data structure
RUNTASK id=2 if_task if0...
RUNTASK id=3 if_task if1...
RUNTASK id=4 if_task if2...
RUNTASK id=5 if_task if3...
RUNTASK id=6 if_task if26...
RUNTASK id=7 timer_task...
RUNTASK id=8 conn_mgr...
RUNTASK id=9 main_8021x...
RUNTASK id=10 period_task...
========== ADSL Modem initialization OK ! ======
RUNTASK id=11 dhcp_daemon...
Primary image: 1, flash area 3
---[ ZIP1 head start in 0xB3220000 ]---
found signature: 78h 56h 34h 12h
ulImgLens=1175395, LENGTH[3]-12=1900532
length checking OK
[0] find End at 0xB333EC00 len=1175395
---[ ZIP1 head start in 0xB333F000 ]---
found signature: 78h 56h 34h 12h
ulImgLens=239948, LENGTH[3]-12=1900532
length checking OK
[1] find End at 0xB3379800 len=239948
---[ ZIP1 head start in 0xB3379C00 ]---
found signature: 78h 56h 34h 12h
ulImgLens=93086, LENGTH[3]-12=1900532
length checking OK
[2] find End at 0xB3390400 len=93086
---[ ZIP1 head start in 0xB3390800 ]---
found signature: 78h 56h 34h 12h
ulImgLens=149729, LENGTH[3]-12=1900532
length checking OK
[3] find End at 0xB33B5000 len=149729
Image[1] at 0x0011EF63, len = 1175395
Image[2] at 0x0003A94C, len = 239948
Image[3] at 0x00016B9E, len = 93086
Image[4] at 0x000248E1, len = 149729
Unzipping from B333F000 to 81D00000 ... [ZIP1] done
Uncompressed size = 1487492
drive start addr[0]=81d00000, [1]=81e6b290
[HTTPD] flash_init: failed!!
httpd: listen at 192.168.2.1:80
httpd: listen at 0.0.0.0:9000
httpd: listen at 0.0.0.0:8085
RUNTASK httpd...
RUNTASK id=16 SSLRelayServer ...
RUNTASK id=17 SSLClient ...
RUNTASK id=18 dnsproxy...
RUNTASK id=19 dhcpd_mgmt_task...
UPnP is disabled
update_device_OUI: OUI_str=0012BF
[0] Allocate mailbox 6
Tr69_Init: Tr69_Task sleeping
Tr69_Init: Tr69_TimerTask sleeping
Starting Multitask...
[0] Allocate resource 27, FreeResource = 1
ifx_ssc_init : nbytes 136
ifx_ssc_set_baud: br = 2
call ifx_ssc_init() = 0
call ifx_ssc_open() = 0
SPI_Init!
T_WLAN_INT=2
gSetting.CountryCode=0x8114
gSetting.Interface[T_WLAN_INT].Link_Type=1
RUNTASK id=25 apAppInit...
RUNTASK id=28 ScanMIH_task...
[qm_outbnd_bw_update] ...
[qm_cbq_resetclst] n_totalcls=9
aft rst: id=0, bw=799 ns/byte, maxd=0 ms,
         maxb=32, minb=2, avgpktsz=250, maxpktsz=1604,
         offtime=362, parent=0, borrow=0
         pri=0, maxidle=113, minidle=-2566,
         maxq=96, clsfg=17
aft rst: id=1, bw=79999 ns/byte, maxd=0 ms,
         maxb=16, minb=1, avgpktsz=1604, maxpktsz=1604,
         offtime=128191, parent=812aa578, borrow=812aa578
         pri=7, maxidle=34157, minidle=-256639,
         maxq=48, clsfg=21
aft rst: id=2, bw=808 ns/byte, maxd=0 ms,
         maxb=32, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=745, parent=812aa578, borrow=812aa578
         pri=0, maxidle=114, minidle=-2592,
         maxq=96, clsfg=21
aft rst: id=3, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=0, maxidle=0, minidle=-4294966,
         maxq=96, clsfg=21
aft rst: id=4, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=2, maxidle=0, minidle=-4294966,
         maxq=48, clsfg=23
aft rst: id=5, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=3, maxidle=0, minidle=-4294966,
         maxq=48, clsfg=23
aft rst: id=6, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=4, maxidle=0, minidle=-4294966,
         maxq=48, clsfg=23
aft rst: id=7, bw=1338830 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=1370962, parent=812a8f78, borrow=812a8f78
         pri=5, maxidle=0, minidle=-4294966,
         maxq=48, clsfg=23
aft rst: id=8, bw=808 ns/byte, maxd=0 ms,
         maxb=16, minb=4, avgpktsz=250, maxpktsz=1604,
         offtime=745, parent=812a8f78, borrow=812a8f78
         pri=7, maxidle=48, minidle=-2592,
         maxq=48, clsfg=20
Unzip DSP firmware ...
Unzipping from B3390800 to 811AE29C ... [ZIP1] [qm_cbq_enable] CBQ enabled at link: 0, DeQTask=29, wakeup event=12
                low_pri_mtu: 25031
[qm_outbnd_bw_update] ok
done
Uncompressed size = 465468
ADSL Firmware: 1.4.0.13.0.2 [Annex B:0x4208 0x0]
ifno2dot1x_if[2]=0
ifno2dot1x_if[24]=81
[main_8021x] dot1x_build_if_mapping() completed.
init psock cnt=1
rzMemory start: 0x809F6AAC, end 0x80B562AC, size 1439744 VOICE_DRV_Init
Reset Duslic 2 High begin
Reset Duslic end
VINETIC module, version : 1.1.17.3 loaded
Board_InitPlatform!
Board_InitIrq!
[26] Allocate resource 28, FreeResource = 2
[26] Allocate resource 29, FreeResource = 3
[26] Allocate resource 30, FreeResource = 4
[26] Allocate resource 31, FreeResource = 5
[26] Allocate resource 32, FreeResource = 6
[26] Allocate resource 33, FreeResource = 7
Devnum 1, Ch 0 opened
 VINETIC_Open /dev/vin10: -2140429624
VINETIC_BasicDeviceInit!
VINETIC_BasicDeviceInit: ret = 0! state ------> 20
Devnum 1, Ch 1 opened
 VINETIC_Open /dev/vin11: -2140429540
[26] Allocate resource 34, FreeResource = 8
[26] Allocate resource 35, FreeResource = 9
[26] Allocate resource 36, FreeResource = 10
[26] Allocate resource 37, FreeResource = 11
initVineticTapi: pInit = 0x808fc3c8
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
VINETIC_Host_InitChip: nRev= 20832
VINETIC_Host_InitChip: pDev->nChipRev = 96
Dwld_LoadFirmware: P_RAM ret = 0
Dwld_LoadFirmware: D_RAM ret = 0
Dwld_LoadFirmware: ActivateEdsp ret = 0
Host_DownloadCram: bbd->buf = 0x8043e510, bbd->size = 288
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
Host_SetSlic: bbd->buf = 0x8043e510, bbd->size = 288
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
Host_SetSlic: bbd_slic.data = 0x8043e60a, bbd_slic.size = 2
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
bbd_get_block: bbd->buf = 0x8043e510, bbd->size = 288
initVineticTapi: ret = 0
 VINETIC_Ioctl: Phone 1 IFXPHONE_INIT ok!
 VINETIC_Ioctl: Phone 1 IFXPHONE_SET_LINEFEED ok!
Devnum 1, Ch 2 opened
 VINETIC_Open /dev/vin12: -2140428156
 VINETIC_Ioctl: Phone 2 IFXPHONE_INIT ok!
 VINETIC_Ioctl: Phone 2 IFXPHONE_SET_LINEFEED ok!
Devnum 1, Ch 3 opened
 VINETIC_Open /dev/vin13: -2140426772
 VINETIC_Ioctl: Phone 3 IFXPHONE_INIT ok!
 VINETIC_Ioctl: Phone 3 IFXPHONE_SET_LINEFEED ok!
Devnum 1, Ch 4 opened
 VINETIC_Open /dev/vin14: -2140425388
 VINETIC_Ioctl: Phone 4 IFXPHONE_INIT ok!
 VINETIC_Ioctl: Phone 4 IFXPHONE_SET_LINEFEED ok!
RUNTASK id=23 VINETIC_DRV_Task...
[30] Allocate mailbox 7
RUNTASK id=30 VINETIC_T38_Task...
RUNTASK id=32 FXO_flash_task...
RUNTASK id=31 VOICE_API_task...
Step 0
Duslic debug: open device!
Board_DevOpen
Board_DevOpen 4 77
---------------------duslic_ResetChMember begin
DUSLIC_GpioInit begin
DUSLIC_GpioInit end
---------------------duslic_ResetChMember end
DUSLIC_Init begin
DUSLIC_GpioInit begin
DUSLIC_GpioInit end
Dwld_LoadCram begin 0
----> Dwld_LoadCram end
DUSLIC_GpioInit begin
DUSLIC_GpioInit end
Dwld_LoadCram begin 1
----> Dwld_LoadCram end
DUSLIC_Init end
DUSLIC_Set_Pcm begin
DUSLIC_Set_Pcm End
init_DTMF_data: time 3141
cpc5621_open: open devNum=0, chNum=0, pDev=00000000, CPC5621_Devices=804df81c
Drv_CPC5621_Init: 0, 1
Drv_CPC5621_Init end pDev->devHandle 806C5D4C
cpc5621_open: open devNum=0, chNum=0, pDev=809eb2b4
before pDev->bOpen == TRUE
after pDev->bOpen == TRUE
CPC_IO_InstanceInit pDev 809EB2B4 begin
before DUSLIC_GpioReserve pDev 809EB2B4 pDev->devHandle 806C5D4C
before DUSLIC_GpioConfig 0
:::::::::::::_GpioConfig begin 806C5D4C Gpio->nIO 0
     :::GPIO_MODE_OUTPUT
     :::GPIO_INT_DUP_605
:::::::::::::_GpioConfig end
before DUSLIC_GpioConfig 1
before DUSLIC_GpioConfig 2
:::::::::::::_GpioConfig begin 806C5D4C Gpio->nIO 1
     :::GPIO_MODE_INPUT
     :::GPIO_MODE_INT
     :::GPIO_INT_FALLING
     :::GPIO_INT_DUP_605
:::::::::::::_GpioConfig end
before DUSLIC_GpioConfig 3
:::::::::::::_GpioConfig begin 806C5D4C Gpio->nIO 2
     :::GPIO_MODE_INPUT
     :::GPIO_MODE_INT
     :::GPIO_INT_RISING
     :::GPIO_INT_FALLING
     :::GPIO_INT_DUP_605
:::::::::::::_GpioConfig end
before DUSLIC_GpioConfig 4
:::::::::::::_GpioConfig begin 806C5D4C Gpio->nIO 3
     :::GPIO_MODE_INPUT
     :::GPIO_MODE_INT
     :::GPIO_INT_FALLING
     :::GPIO_INT_DUP_605
:::::::::::::_GpioConfig end
force_daa_offhook ch 2 onoff 1 time 3164
SSLServer> SSLServer() run
PrivateKey=
MyCertificate=
CaCertificate=
SSL_method=31
verify_mode=0
cipher_suites=ADH:SHA1:HIGH:EXP
tftp_server_ip=140.92.61.131
fn_myCerttificate=ServCert.pem
fn_privateKey=PrivKey.pem
fn_caCertificate=CaCert.pem
force_daa_offhook ch 2 onoff 0 time 3193
CID_Detect CH 2 onoff 1
FXO_PolState_DTMFCID --> Detect Polarity Changed Enable DTMF CID
 TEL_DRV_Init
 TEL_DRV_StartDetVOIPHook
 TEL_DRV_StartDetDTMF
 TEL_DRV_StartDetCEDTone
 TEL_DRV_StartDetVOIPHook
 TEL_DRV_StartDetDTMF
 TEL_DRV_StartDetCEDTone
 TEL_DRV_InitTone
When * the value is 60000
When * the value is 60000
sys_voip_cfg->cpt.reorder = 425@-230;20(0.24/0.24/1)
TEL_DRV_TONE_REORDER  duration = 480
 TEL_DRV_InitRing
 TEL_DRV_SetImpedance
>>>> STUN Setting <<<<
stun Enable : 1
stun Test Enable : 1
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
stun Server : "", IP : 0.0.0.0
stun Port : 3478
RUNTASK id=33 STUNC_InitTask() ...
[26] Allocate resource 38, FreeResource = 12
[26] Allocate mailbox 8
 VOICE_DRV_SetRxDataHookFunc
 VOICE_DRV_SetRxDataHookFunc
TEL_MGR_Init
[26] Allocate resource 39, FreeResource = 13
tel_mgr_mutex=39
[26] Allocate mailbox 9
 TEL_DM_Init
[26] Allocate resource 40, FreeResource = 14
[26] Allocate resource 41, FreeResource = 15
 TEL_DRV_SetPhoneEvtHookFunc
 TEL_DRV_SetLineEvtHookFunc
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: *31#
TEL_MGR_SetDigitMap: #31#
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: *37#
TEL_MGR_SetDigitMap: #37#
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: [2-9]#
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: (x.|0900326690xx|019301xx|0191066730)
TEL_MGR_SetDigitMap: *91#
TEL_MGR_SetDigitMap: #91#
TEL_MGR_SetDigitMap: *09#
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: **258***
TEL_MGR_SetDigitMap: **258**1
TEL_MGR_SetDigitMap: **258**2
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: *31#
TEL_MGR_SetDigitMap: #31#
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: *37#
TEL_MGR_SetDigitMap: #37#
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: [2-9]#
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: (x.|0900326690xx|019301xx|0191066730)
TEL_MGR_SetDigitMap: *91#
TEL_MGR_SetDigitMap: #91#
TEL_MGR_SetDigitMap: *09#
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap:
TEL_MGR_SetDigitMap: **258***
TEL_MGR_SetDigitMap: **258**1
TEL_MGR_SetDigitMap: **258**2
 TEL_MGR_VoiceChannelInit
 TEL_MGR_SetVoiceChannel
TEL_MGR_SetVoiceChannel: ec mode is 16ms
 TEL_MGR_SetPhoneEC
 VOICE_DRV_SetEC
 TEL_MGR_SetPhoneVAD
 VOICE_DRV_SetVAD
 TEL_MGR_SetPhoneGain
 VOICE_DRV_SetGain
------------------------>TAPI_LL_Phone_Volume Ch 0 TxGain 0 RxGain -7
 TEL_MGR_SetPhoneJitter
 VOICE_DRV_SetJitter
 TEL_MGR_SetVoiceChannel
TEL_MGR_SetVoiceChannel: ec mode is 16ms
 TEL_MGR_SetPhoneEC
 VOICE_DRV_SetEC
 TEL_MGR_SetPhoneVAD
 VOICE_DRV_SetVAD
 TEL_MGR_SetPhoneGain
 VOICE_DRV_SetGain
------------------------>TAPI_LL_Phone_Volume Ch 1 TxGain 0 RxGain -7
 TEL_MGR_SetPhoneJitter
 VOICE_DRV_SetJitter
SIP_CALLBACK_EnableDebug
SIP_CORE_EnableDebug
SIP_CORE_Init
[26] Allocate resource 42, FreeResource = 16
[26] Allocate mailbox 10
SIP_CORE_SetSipStatus
SIP_CORE_CreateTask
>>> SIP_CORE_Task
[26] Allocate resource 43, FreeResource = 17
[26] Allocate mailbox 11
[26] Allocate mailbox 12
[26] Allocate mailbox 13
[26] Allocate mailbox 14
SSLClient> SSLClient() run
dt_provision_task> running
dt_provision_task> activate TR69
ScanMIH_task> DHCP ip give: 192.168.2.100 ~ 199, range: 100
pCtx=0x809e9784 in ssld_conf_init
dt_provision_task> provisioned, enable GUI
sslc_conf_init> pCtx=0x809e72c0 in ssld_conf_init
SSL client> re-LoadCACertificate( 3rdCA ) ok!!
SSL client> re-LoadCACertificate( Verisign ) ok!!
SSL client> re-LoadCACertificate( FUN ) ok!!
SSL client> re-LoadServerCertAndKey() ok!!
sslc_conf_init> sslc_conf_init() ok
SSLClient> want to bind socket for client application!
SSL server: re-LoadCACertificate() ok!!
SSL server : re-LoadServerCertAndKey() ok!!
ssld_conf_init> ssld_conf_init() ok
RUNTSK l2s3 22
RUNTSK l3l2 26
RUNTSK lll2 36
RUNTSK decrTout 37
ISDN: Q_L2S3:3, Q_L3L2:4, Q_LLL2:5
!IFX_FA_Init: IFIN_FA_StatusBitsInfo error
 TEL_DRV_EOFEvtCB: channelId(0)
!TEL_MGR_ProcPhoneFaxStop
!TEL_MGR_ProcPhoneFaxStop: no active call
!TEL_MGR_ProcMsg: TEL_MGR_ProcPhoneFaxStop
Atheros_NetTask running ...
RUNTASK id=39 Atheros_NetTask ...
513
pVportBaseBss=0x807c7140
514
[HWLAN] Ready
muxDevStop is called ...
bridgePortRemove : vp, 10000
Bridge port (base BSS) remove succeeded for vp1
rapi_tmr_cancel: cookie 80f7d2fc, Initialized=1
 00000000 80f7d2fc
cookie 80f7d2fc 80f7d2fc
rapi_tmr_cancel: can't find time structure cookie 80f7d2fc
[HWLAN] ar5hwcIntDisable : STATE_AP_DOWN , pdevInfo->localSta=807BF2F0 state=0100
wlan1 stopped
[reset_802dot1x] wireless module ready
[init_wpa] dot1x_ready[0]=3
[init_wpa] dot1x_ready[1]=3
[init_wpa] dot1x_ready[2]=3
[init_wpa] dot1x_ready[3]=3
[init_wpa] dot1x_ready[4]=3
[init_wpa] dot1x_ready[5]=3
[reset_802dot1x] 802.1Xv2 ready
}}}
