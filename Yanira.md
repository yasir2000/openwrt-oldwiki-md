\#pragma section-numbers off

'''MMC/SD card mod'''

This is a short guide to get an MMC/SD card working with !OpenWrt
Kamikaze 8.09 and an 2.6 Kernel. The driver can be configured using
either UCI CLI or the LuCI WebUI.

= GPIO Pinouts = {{{ Description GPIO
------------------------------------PIN 1, CS - Chip Select GPIO 7 PIN
2, DI - Data In GPIO 1 PIN 3, VSS - Ground GND PIN 4, VDD - 3.3 Volts
3.3 Volts PIN 5, CLK - Clock GPIO 3 PIN 7, DO - Data Out GPIO 4 }}} =
GPIO Solder Points = Images from PCB.

= Configuration =

== Using UCI CLI ==

=== Install packages ===

Required packages:

> -   kmod-mmc-over-gpio
> -   kmod-fs-ext3/kmod-fs-ext2/.... (we use the EXT3 file system here)
> -   cfdisk/fdisk (we use cfdisk here)
> -   e2fsprogs

{{{ <root@OpenWrt>:\~\# opkg update <root@OpenWrt>:\~\# opkg install
kmod-mmc-over-gpio kmod-fs-ext3 cfdisk e2fsprogs }}} === Configure GPIOs
=== {{{ <root@OpenWrt>:\~\# uci set
mmc\_over\_gpio.@mmc\_over\_gpio\[0\].enabled=1 <root@OpenWrt>:\~\# uci
set mmc\_over\_gpio.@mmc\_over\_gpio\[0\].DI\_pin=1 <root@OpenWrt>:\~\#
uci set mmc\_over\_gpio.@mmc\_over\_gpio\[0\].DO\_pin=4
<root@OpenWrt>:\~\# uci set
mmc\_over\_gpio.@mmc\_over\_gpio\[0\].CLK\_pin=3 <root@OpenWrt>:\~\# uci
set mmc\_over\_gpio.@mmc\_over\_gpio\[0\].CS\_pin=7 <root@OpenWrt>:\~\#
uci commit mmc\_over\_gpio <root@OpenWrt>:\~\#
/etc/init.d/mmc\_over\_gpio start <root@OpenWrt>:\~\#
/etc/init.d/mmc\_over\_gpio enable }}} === Mount the MMC/SD card via
fstab === To get partition mounted automatically you have to edit and
change START=20 to START=98 in the /etc/init.d/fstab init script. {{{
<root@OpenWrt>:\~\# uci set fstab.@mount\[0\].enabled=1
<root@OpenWrt>:\~\# uci set fstab.@mount\[0\].fstype=ext3
<root@OpenWrt>:\~\# uci set fstab.@mount\[0\].device=/dev/mmcblk0p1
<root@OpenWrt>:\~\# uci set fstab.@mount\[0\].target=/mnt/mmc
<root@OpenWrt>:\~\# uci set fstab.@mount\[0\].options=rw,sync,noatime
<root@OpenWrt>:\~\# uci commit fstab <root@OpenWrt>:\~\#
/etc/init.d/fstab restart }}} == Using the LuCI WebUI ==
