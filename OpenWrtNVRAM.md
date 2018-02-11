\[\[TableOfContents\]\] -----/!'''The {{{nvram}}} utility is unavailable
in OpenWrt builds based on the 2.6 kernel''' (at least with the
BCM947xx/953xx target), as discussed in these forum threads: \*
<http://forum.openwrt.org/viewtopic.php?pid=76160> \*
<http://forum.openwrt.org/viewtopic.php?id=6605> If only read access is
required, the nvram partition can be read directly, e.g.: {{{
<root@harriet>:\~\# cat /dev/mtd4 | grep boot boot\_wait=on NeedReboot=0
}}} -----== IP Interface Settings == In order to configure the IP
interfaces on the system, a number of settings are grouped into
"bundles" sharing the same prefix. The default bundles are called "lan",
"wan" and "wifi", but those names have no special significance other
than the fact that the startup script looks for bundles with these
names. They might as well have been called "rod", "jane" and "freddy".
In fact, let's demonstrate that:

{{{ freddy\_ifname=vlan2 freddy\_proto=static
freddy\_ipaddr=192.168.100.100 freddy\_netmask=255.255.255.0
freddy\_gateway=192.168.100.1 }}}

In order to trigger the configuration of this interface, you'd type
{{{ifup freddy}}}. This would configure the kernel IP interface
{{{vlan2}}} with the given IP address and netmask, and also insert a
defaultroute to 192.168.100.1 via this interface.

(Note: if you are perverse enough actually to want to call your IP
interfaces "rod", "jane" and "freddy", you will have to modify
{{{/etc/init.d/S40network}}} to call ifup appropriately. This is a
useful thing to do if you want to configure extra IP interfaces on your
system. You might want to separate off one of your ethernet ports and
call it "dmz", for example)

So, let's be sensible and stick to the default "wan", "lan" and "wifi".
The WRT54G requires LAN and WAN settings at least to boot properly.
Since the "lan" bundle normally consists of the LAN ports bridged
together with the wifi interface on the same subnet, most people should
leave the [wifi]()\* variables UNSET.

For each bundle you may set the following variables (ie lan\_ifname,
wan\_ifname, wifi\_ifname)

'''Meaning/Purpose''' | If the \_ifname is a bridge (br0) then \_ifnames
is the interfaces to be bridged | IP address to use if \_proto is static
| gateway to use if \_proto is static (X.X.X.X notation) | Enable
spanning tree if \_ifname is a bridge (0 or 1) |

Don't set {{{*\_gateway}}} more than once, or use {{{*\_proto=dhcp}}} on
more than one interface, or you'll find yourself with multiple default
routes inserted which is probably not what you want.

After changing any of these settings, type {{{ifup foo}}} (where foo is
one of lan, wan, wifi etc as appropriate)

== Bridging == You can create a layer 2 (ethernet) bridge between two or
more interfaces by setting {{{foo\_ifname=br0}}} and
{{{foo\_ifnames="iface1 iface2 iface3..."}}}. Typically you will see
something similar to this in your default settings:

{{{ lan\_ifname=br0 lan\_ifnames="vlan0 eth1" lan\_ipaddr=192.168.1.1
lan\_netmask=255.255.255.0 lan\_proto=static lan\_stp=1 }}}

In this case, the IP address is assigned to the bridge device (br0), and
the bridge device connects interfaces vlan0 and eth1, corresponding to
LAN ports 1-4 and the wireless interface respectively.

Note: the \[:OpenWrtDocs/Configuration\#NetworkInterfaceNames:network
interface names\] corresponding to physical ports on the Wrt vary from
one brand and model of router to another. Your particular device may use
different settings to those shown above.

== VLAN Settings == Because of the way the interfaces are done in
hardware (one interface, multiple ports), there are required ''vlan''
settings for the device. If these aren't set to the proper values, then
the interfaces will not be assigned correctly. Note that if you're using
''admcfg'' or similar, this may not apply to you. (I'm not sure).

Be sure the NVRAM has settings for the following, and the recommended
defaults:

'''Recommended Value''' | 1 2 3 4 5\* | 0 5 ||

In other words, an interface called "vlan0" is linked to ports 1-4 of
the internal switch (typically labelled "LAN 1-4" on the box, although
you may find that they are in reverse order),and an interface called
"vlan1" is linked to port 0 of the internal switch (typically labelled
"WAN" on the box). Port 5 of the internal switch carries all VLANs
tagged (that's what the asterisk is for) to the real interface et(h)0.

{{{ PHYSICALLY: tagged +-------------------+ eth0 ============ | 5
SWITCH | | 4 3 2 1 0 | +-------------------+ | | | | | ...LAN 1-4... WAN

LOGICALLY:

:   vlan0 ------------- LAN 1-4 vlan1 ------------- WAN

}}}

If the NVRAM is set with those values, then the recommended values for
'''wan\_ifnames''' and '''lan\_ifnames''' will be correct. Note that by
changing the ports around, you are able to change which port is the WAN
port and so on, but that isn't a very good idea in general.

Now let's say you want to syphon off the port labelled "LAN 1" as a DMZ
port on a separate subnet. On an Asus router this is actually switch
port 4. So you'd reconfigure as:

et0 | et0 | et0 |

Once you've done this, you can configure interface {{{vlan2}}} with its
own IP address on its own subnet, and Wrt will route between them.

{{{ dmz\_ifname=vlan2 dmz\_ipaddr=192.168.2.1 dmz\_netmask=255.255.255.0
dmz\_proto=static }}}

Type {{{ifup dmz}}} to perform the configuration, and modify
{{{/etc/init.d/S40network}}} so that this is done when your box is next
rebooted too. See DemilitarizedZoneHowto for more details.

Another possibility is that if you don't need a separate WAN port, you
could get rid of vlan1 and configure vlan0 so that all 5 ports are on
the LAN subnet. Going to the other extreme, you could configure five
separate vlans and have a five-port ethernet router.

== Wireless Configuration == Although the [wifi]()\* variables can be
used to configure the IP network settings of the wireless interface, the
default setting is to include the wireless interface in lan\_ifnames and
leave the [wifi]()\* variables unset. If you remove the wireless
interface from the lan bridge (which you MUST do to use ad-hoc mode)
configure the [wifi]()\* variables according to the general settings
above.

There are separate variables called [wl0]()\* which configure the
characteristics of the ''physical'' wireless interface - which are
applicable whether or not the wifi interface is bridged or a separate IP
network.

'''Note:''' There are [wl]()\* and [wl0]()\* variables; the [wl]()\*
variables are obsoleted and were replaced by [wl0]()\*.

'''Meaning''' | Set by wlconf, use il0macaddr to change the mac | (0/1)
0: allow clients to see each other 1: hide clients from each other |
(0/1) 0: broadcast ssid 1: hide ssid | (disabled/allow/deny) used to
(allow/deny) mac addresses listed in wl0\_maclist | Enable / disable the
radio (1=enable) | '''wl0\_gmode''' '''wl0\_gmode\_protection''' all |
Set fragmentation threshold (default 2346) | Set DTIM period (default 1)
| (on/off) enable/disable frameburst | See wl -h| (per Whiterussian RC5)
Adjusts timing for signal propagation time. Unit: \[m\] (one-way).
Setting this variable overrules setting of shortslot/longslot timing.
Setting this variable is only needed over distances greater than appr.
1.5 km. The need usually shows when communication throughput is very low
although the ratio of signal strength to noise is good. |

For WPA: (See \["OpenWrtDocs/Configuration"\] on how to enable WPA on
current snapshots)

obsolete, use '''wl0\_akm''' NOTE: set to psk or radius because some
configurations don't work without it. See
<http://www.bingner.com/openwrt/wpa.html>,
<http://wiki.openwrt.org/OpenWrtDocs/Wpa2Enterprise> or maybe you can
use some other wpa supplicant instead of nas.| WPA pre-shared key | | |

For WEP:

enabled/disabled | primary key index: the wl0\_key\[1234\] used (values:
''1'',''2'',''3'',''4'') | 1 (shared key) / 0 (open); the 'shared key'
option is the most vulnerable WEP option as it most facilitates an
intruder due to a fundamental security flaw in WEP. The 'open' setting
will allow association but will make it an intruder more difficult to
find the encryption key, needed for traffic. ||

For WDS:

Set lazywds mode - dynamically grant WDS to anyone(''1=enable /
0=disable'') |

'''NOTE:''' if you want to use a wrt54gs as a WDS client with
'''wl0\_wds''' set, the '''wl0\_gmode''' setting must not be in
afterburner (6) mode (apparently no linksys speedboost is available for
WDS clients). Also, '''wl0\_mode''' should be set to ''ap''.

Misc:

Supported 802.11 modes, automatically set by wlconf | Set by wlconf to
the wireless revision, (4:v1.0 hardware, 7:v2,gs) ||

In summary, you could find the wifi interface known by three different
identifiers: as {{{[wl0]()*}}} for the physical interface settings, as
{{{[wifi]()*}}} for its IP settings if it's on a separate subnet, and as
{{{eth1}}} or {{{eth2}}} to the kernel, depending on your hardware.
Confused? :)

== Static Routes == Static routes are a bit uglier to maintain, but they
are still maintainable. Until RC5 there is only one NVRAM setting for
them: '''{{{static\_route}}}'''. This contains all the static routes to
be added upon boot-up. From RC6 there can be
'''{{{&lt;ifname&gt;\_static\_route}}}''' NVRAM variables for each
interface, e.g. '''{{{lan\_static\_route}}}'''.

The syntax of the {{{static\_route}}} NVRAM variable is as follows:

{{{

:   static\_route=ip:netmask:gatewayip:metric:interface \# until RC5
    interface\_static\_route=ip:netmask:gatewayip:metric \# from RC6

}}}

So, for example, to set a static route to 10.1.2.0/255.255.255.0 via
vlan1, use:

{{{ nvram set static\_route=10.1.2.0:255.255.255.0:0.0.0.0:1:vlan1 \#
until RC5 nvram set lan\_static\_route=10.1.2.0:255.255.255.0:0.0.0.0:1
\# from RC6 }}}

This will make 10.1.2.0 directly connected. To route via a router, use:

{{{ nvram set static\_route=10.1.2.0:255.255.255.0:192.168.1.1:1:vlan1
\# until RC5 nvram set
lan\_static\_route=10.1.2.0:255.255.255.0:192.168.1.1:1 \# from RC6 }}}

This will use vlan1 to send packets to 10.1.2.0 via router 192.168.1.1

As of the most recent CVS build, all values must be present. The
networking script doesn't detect missing values, and will thererfore not
create the route if the syntax is incorrect (things missing, etc.).

To add multiple routes, seperate each route formatted as above with a
space. To avoid the shell truncating after the first space, you need to
quote:

{{{ nvram set static\_route="10.1.2.0:255.255.255.0:192.168.1.1:1:vlan1
10.1.3.0:255.255.255.0:192.168.1.1:1:vlan1" \# until RC5 nvram set
lan\_static\_route="10.1.2.0:255.255.255.0:192.168.1.1:1
10.1.3.0:255.255.255.0:192.168.1.1:1" \# from RC6 }}}

To see if the new settings are working, try {{{ \# ifup lan }}}

If you need to debug this, {{{ \# DEBUG=echo ifup lan }}}

This will list all the commands to be run. You can then copy and paste
the "route" command from the output, and run it by hand to see what's
wrong.

== misc == DHCP Settings:

'''Meaning''' | The number of addresses in DHCP pool ||

Unsetting these values will not stop the dhcp server from running; it
will use default values of dhcp\_start=100 and dhcp\_num=150. To turn
off the dhcp server, use {{{chmod -x /etc/init.d/S50dnsmasq}}} \[jffs2
systems\] or {{{rm /etc/init.d/S50dnsmasq}}} \[squashfs systems\]

NOTE: In the unlikely event you're using a lan\_netmask other than
255.255.255.0, be aware that {{{dhcp\_start}}} is an offset into your
network segment, as described by
{{{int2ip(ip2int(lan\_ipaddr)&ip2int(lan\_netmask))}}}. Furthermore, the
startup script S50dnsmasq does not allow for the possibility that you
might want to run DHCP servers on multiple interfaces, or that you might
want to run it on a different interface than [lan]()\*

Hostname:

The hostname of your router. ||

\[\[Anchor(NVRAMCommitting)\]\] == NVRAM committing == When you set/get
nvram settings, you are get/setting them in RAM. "nvram commit" writes
them persistenly to the flash. But you don't have to commit in order to
test, in fact it's safer not to because the flash memory has a limited
write cycle life. (Don't be scared though, it's something like
1000-10.000 times; still better to only save it when really needed! NB
In \["Faq"\] it is however stated that this figure, according to
manufacturers, can be in the range of 100,000 - 1,000,000) You can save
your settings to RAM, check them out by ifdown/ifup'ing all your
interfaces, and then "nvram commit" them if they are to your liking. If
not, you can reboot and you're back to the last working configuration
you had.

You can find out the type of flash chip you have with:

{{{ <root@OpenWrt>:\~\# nvram get flash\_type Samsung K8D3216UBC 2Mx16
BotB }}}

Here is a table of known flash chips and their endurance according to
the manufacturers datasheet:

| '''Endurance''' | Samsung K8D3216UBC 2Mx16 BotB |

== Applying changes to wireless settings == To apply the changes made to
the nvram settings that start with '''{{{[wl0]()}}}''' (e.g. to the
{{{wl0\_maclist}}} entry) run the '''{{{wifi}}}''' command (or
'''{{{wl}}}''' if you have not installed the wificonf package) to
reconfigure the Broadcom {{{wl.o}}} module in the kernel.
