\#\# Please edit system and help pages ONLY in the moinmaster wiki! For
more \#\# information, please see MoinMaster:MoinPagesEditorGroup.
\#\#acl MoinPagesEditorGroup:read,write,delete,revert All:read
\#\#master-page:HelpTemplate \#\#master-date:Unknown-Date \#format wiki
\#language en == Running OpenWRT on any x86 machine == WARNING: If you
are not familiar with configuring the linux kernel to work on specific
hardware, you should not be attempting this.

The purpose of this HowTo is mostly focused around making OpenWRT use a
vga console instead of a serial console. It is assumed you will know how
to select other necessary kernel modules for your specific hardware.
Network cards will most likely be the first hardware you will have to
add to get it to work on your particular hardware.

This HowTo was developed using Kamikaze revision 6435. Unless support
for Generic x86 hardware is added to OpenWRT this information will
undoubtedly become stale.

=== Getting started: === The Soekris port is almost enough to get
OpenWRT running on any x86 machine. Follow the Soekris page to get
started:SoekrisPort

Once you have built OpenWRT for the Soekris you can proceed to modify it
for your hardware.

=== Files to modify: ===

:   -   trunk/target/linux/x86-2.6/config/default
    -   trunk/target/linux/x86-2.6/image/grub/menu.lst
    -   trunk/package/base-files/etc/inittab

=== Kernel config changes: === At the minimum you will need to add the
following lines to'''trunk/target/linux/x86-2.6/config/default:'''

{{{ \# \# Console display driver support \# CONFIG\_VGA\_CONSOLE=y \#
CONFIG\_VGACON\_SOFT\_SCROLLBACK is not set CONFIG\_VIDEO\_SELECT=y
CONFIG\_MDA\_CONSOLE=m CONFIG\_DUMMY\_CONSOLE=y
CONFIG\_FRAMEBUFFER\_CONSOLE=y \# CONFIG\_FRAMEBUFFER\_CONSOLE\_ROTATION
is not set \# CONFIG\_FONTS is not set CONFIG\_FONT\_8x8=y
CONFIG\_FONT\_8x16=y \# CONFIG\_BT\_HIDP is not set CONFIG\_INPUT=y \#
CONFIG\_INPUT\_FF\_MEMLESS is not set \# \# Userland interfaces \# \#
CONFIG\_INPUT\_MOUSEDEV is not set \# CONFIG\_INPUT\_JOYDEV is not set
\# CONFIG\_INPUT\_TSDEV is not set \# CONFIG\_INPUT\_EVDEV is not set \#
CONFIG\_INPUT\_EVBUG is not set \# \# Input Device Drivers \#
CONFIG\_INPUT\_KEYBOARD=y CONFIG\_KEYBOARD\_ATKBD=y \#
CONFIG\_KEYBOARD\_SUNKBD is not set \# CONFIG\_KEYBOARD\_LKKBD is not
set \# CONFIG\_KEYBOARD\_XTKBD is not set \# CONFIG\_KEYBOARD\_NEWTON is
not set \# CONFIG\_KEYBOARD\_STOWAWAY is not set \# CONFIG\_INPUT\_MOUSE
is not set \# CONFIG\_INPUT\_JOYSTICK is not set \#
CONFIG\_INPUT\_TOUCHSCREEN is not set \# CONFIG\_INPUT\_MISC is not set
CONFIG\_SERIO=y CONFIG\_SERIO\_I8042=y CONFIG\_SERIO\_SERPORT=y \#
CONFIG\_SERIO\_CT82C710 is not set \# CONFIG\_SERIO\_PCIPS2 is not set
CONFIG\_SERIO\_LIBPS2=y \# CONFIG\_SERIO\_RAW is not set \#
CONFIG\_SONYPI is not set \# CONFIG\_USB\_KBD is not set \#
CONFIG\_USB\_MOUSE is not set \# CONFIG\_USB\_AIPTEK is not set \#
CONFIG\_USB\_WACOM is not set \# CONFIG\_USB\_ACECAD is not set \#
CONFIG\_USB\_KBTAB is not set \# CONFIG\_USB\_POWERMATE is not set \#
CONFIG\_USB\_TOUCHSCREEN is not set \# CONFIG\_USB\_YEALINK is not set
\# CONFIG\_USB\_XPAD is not set \# CONFIG\_USB\_ATI\_REMOTE is not set
\# CONFIG\_USB\_ATI\_REMOTE2 is not set \# CONFIG\_USB\_KEYSPAN\_REMOTE
is not set \# CONFIG\_USB\_APPLETOUCH is not set CONFIG\_VT=y
CONFIG\_VT\_CONSOLE=y CONFIG\_HW\_CONSOLE=y \#
CONFIG\_VT\_HW\_CONSOLE\_BINDING is not set \# \# Console display driver
support \# CONFIG\_VGA\_CONSOLE=y \# CONFIG\_VGACON\_SOFT\_SCROLLBACK is
not set \# CONFIG\_VIDEO\_SELECT is not set \# CONFIG\_MDA\_CONSOLE is
not set CONFIG\_DUMMY\_CONSOLE=y }}} This will enable the keyboard and
vga console in the linux kernel.

You still need to make OpenWRT use the vga console. In order to do this
you need to modify two other files:

> -   trunk/target/linux/x86-2.6/image/grub/menu.lst

Comment out the first two lines that put the grub menu on the serial
console and remove the console kernel parameter on the "kernel" line:

{{{ \#serial --unit=0 --<speed=@BAUDRATE>@ --word=8 --parity=no --stop=1
\#terminal --timeout=10 serial default 0 timeout 5 title OpenWrt root
(hd0,0) kernel /boot/vmlinuz @CMDLINE@ noinitrd reboot=bios boot }}} \*
trunk/package/base-files/etc/inittab Busybox's init will use the console
the kernel finds by removing the device from the "askfirst" line:

{{{ ::askfirst:/bin/ash --login}}} === Additional work: === ==== Other
hardware modules: ==== If your x86 machine uses network adapters other
than those natively built with OpenWRT you will have to add them to the
default kernel config. The easiest way that I know of doing this is by
first building OpenWRT and then working in
the'''trunk/build\_i386/linux'''directory.

> 1.  Make a backup copy of '''.config''' and run '''make menuconfig'''
>     while in the '''trunk/build\_i386/linux '''directory.
> 2.  Select your new kernel modules and save the kernel config.
> 3.  Do a diff between '''.config''' and the backup of the previous
>     version
> 4.  Add the changes back to
>     '''trunk/target/linux/x86-2.6/config/default'''

(I personally have been using the four port ethernet adapter from
Soekris and have not had to add any additional modules, so if these
steps are incomplete please correct this page.)

==== Fix the network config: ==== Once you have OpenWRT booting and
running on your x86 hardware you will need to modify
'''/etc/config/network''' to suit your hardware.

==== Kernel optimizations: ==== By default the x86 build for OpenWRT is
optimized only for 486 processors. Additional speed will undoubtedly be
had by selecting kernel optimizations specific to your processors.
However, I have my doubts that building a 64 bit kernel will be usable
without making other changes to buildroot system.
