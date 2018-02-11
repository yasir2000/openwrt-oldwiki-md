\[\[TableOfContents\]\] = Status of the AMD Alchemy port of OpenWrt =

== What is this AMD Alchemy stuff? ==

\[<http://www.amd.com/us-en/ConnectivitySolutions/ProductInformation/0,,50_2330_6625,00.html>
AMD Alchemy\] is a processor developped by AMD focused on small
Web/Network applicances.

See the \[:TableOfHardware\] for supported AMD Alchemy routers.

See \[:OpenWrtDocs/InstallingAlchemy\] if you are brave enough to test
it.

== Finished tasks ==

The support for AMD Alchemy is completed :

> -   The images are working and you can do some more testing

== TODO ==

== Firmware/Bootloader ==

The bootloader is \[<http://www.linux-mips.org/wiki/YAMON> Yamon\], and
supports TFTP booting. The firmware can only be booted when it is in
SREC format. OpenWrt images are in the right format already.

= How to help =

Since the board is really well supported with both Linux 2.4 and 2.6,
you can test the firmware built with kamikaze and report any problems
you may encounter.

== Flashing images ==

The YAMON bootloader allows you to either run a kernel in RAM (for
testing purpose), or a filesystem in RAM, or both, but you can also
write these two components on the flash.

=== Loading a kernel in RAM ===

Once you have YAMON access just :

{{{ load &lt;IP address&gt;/&lt;kernel filename&gt; }}}

Then modify the {{{start}}} variable in order to load the kernel in ram
and not on the flash, for that do :

{{{ go . rootfstype=&lt;your fs type&gt; init=&lt;your init script&gt;
}}}

=== Loading filesystem from RAM ===

Almost same thing as before, except that you are likely to use either
the {{{\$start}}} variable, or the {{{go}}} command after having loaded
your filesystem via {{{load}}}

=== Writing combined images to Flash ===

First you need to erase the two sections of the flash which contain
filesystem and kernel :

{{{ erase 0xbfd00000 0xf0000 erase -s load &lt;your combined image&gt;
}}}

This will first erase the kernel, then filesystem, the box will reboot,
and next time you load a combined image, it will be written to the
flash.

=== Writing kernel to Flash ===

Almos the same idea as before :

{{{ erase 0xbfd00000 0xf0000 load &lt;your kernel&gt; }}}

Next time you load a kernel via TFTP it will be written in the flash

=== Writing filesystem to Flash ===

Once again, almost the same thing :

{{{ erase -s system load &lt;your system&gt; }}}

= Other stuff = == Model-specific information ==

Mode-specific information can be found on that model's page. See
\["CategoryAlchemyDevice"\].

== JTAG ==

The JTAG is an EJTAG and can be easily accessible :

\[<http://www.meshcube.org/meshwiki/DebugConnector?action=AttachFile&do=get&target=connectors.png>\]

The 12-pin connectors superposes a serial console, and a JTAG interface
:

\[<http://www.meshcube.org/meshwiki/DebugConnector?action=AttachFile&do=get&target=debug-pinout.png>\]

Here is the pinout :

| TXD (3.3v) | | RXD (3.3v) | | VCC (3.3v) | | TCK | | RST\# | | TMS | |
DINT | | TDO | | GND | | TDI | | GND | | TRST |

Standard EJTAG interface, as the one you can build for a WRT54G will
work. Same thing for the serial port.

Happy debugging !

----CategoryOpenWrtPort
