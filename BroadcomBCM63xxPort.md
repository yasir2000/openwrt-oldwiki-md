\[\[TableOfContents\]\]

This page covers the BCM63xx SoC specificities, but the BCM33xx SoC
(excluding BCM3302 which is a CPU) are the exact same chip, except that
the DSL core is replaced with a DOCSIS/EuroDOCSIS one.

= Status of the Broadcom 63xx port of OpenWrt =

:   -   The Broadcom BCM963xx currently only works with BCM6348/BCM6358
        boards. Others expected soon.
    -   We have GPL drivers for USB (OHCI and EHCI), Ethernet, DSL is
        still binary though.
    -   Belkin has released
        \[<http://www.belkin.com/uk/support/article/?lid=enu&pid=F5D7633uk4A&aid=9294&scid=314>
        "GPL code for the F5D7633"\], but it only has binary Broadcom
        drivers.
    -   TP-link also has some code out for the platform --&gt; see
        \[<http://www.tp-link.com/support/gpl.asp> TP-link GPL code for
        many of their products (many are broadcom based).\]
    -   D-Link GPL download center: \[<http://tsd.dlink.com.tw/> D-Link
        GPL code for all of their products (DSL-2640B, DSL-2740B).\]

== What is this Broadcom 63xx stuff? ==
\[<http://www.broadcom.com/products/DSL/xDSL-CPE-Solutions/BCM6348>
Broadcom63xx SoC\]integrates ADSL/ADSL2+ features, routing, and external
Wireless NIC.

The ethernet SOC is similar to that used in the BCM7318 which is also
used in the Tivo. Tivo has release complete sources of their ethernet
driver \[<http://dynamic.tivo.com/linux/linux.asp> tivo source\] which
could be the basis of a working BCM63xx driver. See the files:

{{{

:   

    linux-2.4/drivers/net/

    :   bcmemac7318.c bcmemac7318.h bcmenet.c bcmenet.h bcmenet\_regs.h

}}} == What are 63xx variants? == There are six 63xx variants:
bcm6345,bcm6338,bcm6348,bcm6358,bcm6368,bcm6816 CPU Mhz USB Host ADSL2
VDSL | 75 - Yes No | ??? - ? ? 240 - Yes No 240 1.1 Yes No 300 2.0 Yes
Yes 300 2.0 Yes Yes 300 2.0 Yes Yes

There are also some other variants like bcm6341, which is a DSP used in
VoIP products.

== Known 63xx platforms == === Known 6345 platforms:\* === === Known
6338 platforms\*: === === Known 6348 platforms\*: === | === Known 6358
platforms\*: === \* If no dedicated Openwrt page is found an external
link is supplied

== Finished tasks == The support for Broadcom 63xx is at this state :

> -   Full linux-2.6.27 support with GPL drivers for Ethernet and USB,
>     only DSL is binary

== TODO ==

:   

    \* Talk with Broadcom related vendors to make them release some sources

    :   . Pirelli Broadband Solutions relesed a GPL source code of its
        Alice Gate 2+ Wi-Fi at this
        \[<http://www.it.pirellibroadband.com/web/products-solutions/solutions/sme-net/gpl/default.page>
        link\] ans a group of people are adapting the USR9108 source
        code to better work on the same router at this
        \[<http://jackthevendicator.dlinkpedia.net/files/broadcom/pirelli_alice_gate_2_plus_wifi/src/>
        link\]

== Firmware/Bootloader == Some devices use RedBoot such as Inventel
Liveboxes. Other run CFE with a built-in LZMA decompressor such as
Siemens SE515, Free Freebox ... CFE is not using standard LZMA
compression arguments, and most noticebly, changes the dictionnary size,
so beware.

= How to help =

:   -   Port ATM/ADSL changes to kernel 2.6.27 and use the binary DSL
        for now
    -   Test the currently merged kernel in order to see if it boots on
        CFE based boards.

---- . CategoryOpenWrtPort ---- \["CategoryBCM63xx"\]
