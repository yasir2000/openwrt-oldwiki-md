= Setting up OpenWRT to be a wireless bridge on a WPA2-PSK wireless
network = == Rationale == This page describes how to set up an OpenWRT
install -- White Russian RC3 on a WRT54G v4 in my case -- to be a
wireless bridge to an existing wireless network. The goal is to end up
with a plain bridge (or get as close as possible to one), with no WDS,
no routing, no anything except for holding one IP for administration, on
a WPA2-PSK enabled wireless network. This allows the following network
configuration:

{{{

:   +---------+ +--------------+ | OpenWRT | &lt;--- WPA2-PSK secured
    WiFi ---&gt; | Existing | | bridge | | access point | -- (DHCP
    server) +---------+ +--------------+ | | | PC PC PC

> (DHCP clients)

> -   No wireless repeater on this end \*

}}}

It's very easy to set up bridged client mode ({{{wl0\_mode=wet}}}) on an
unsecured network - see \["OpenWrtDocs/WhiteRussian/ClientMode"\] for
example - and this also works with WEP or WPA1. You can also set up
routed client mode ({{{wl0\_mode=sta}}}) on a WPA2 network; in this
case, the LAN ports must be on a different subnet from the !WiFi
network.

It turns out, however, that bridged client mode on a WPA2 network is not
quite as easy. It's also easy to get this configuration working in WDS
mode, but where the bridge and the AP are located close to each other,
some clients would jump on the repeater side of the network and cause a
loss in throughput. ''Note: There are some reports that this doesn't
work any more under RC4.''

Most of the information was gleaned from the OpenWRT wiki and posts made
in <http://forum.openwrt.org> and dozens of other web sites, and I thank
the multitude of unattributed contributors for sharing their knowledge.

== Instructions (This section should be expanded) ==

:   1.  Install wl
    2.  Install nas
    3.  Install dhcp-forwarder
    4.  Install libpthread and parprouted (from
        <http://downloads.openwrt.org/people/nico/testing/mipsel/packages/>
        no longer existing?) (try
        <http://downloads.openwrt.org/whiterussian/newest/packages/> for
        libpthread and <http://downloads.openwrt.org/backports/rc5/> for
        parprouted ?)
    5.  Delete /etc/init.d/S45firewall
    6.  Delete /etc/init.d/S50dnsmasq
    7.  Delete /etc/init.d/dhcp-fwd
    8.  Delete /etc/dhcp-fwd.conf
    9.  Create /etc/init.d/S50dhcp-fwd (code below)
    10. Create /etc/init.d/S47sleep (code below)

    1. Edit /etc/init.d/S41wpa and rename it S41supplicant

    :   

        \* Remove the -l parameter from nas - it does not work in Supplicant mode (see \["OpenWrtDocs/nas"\])

        :   -   On two separate lines, this text must be deleted:
            -   \${wifi\_ifname:+ -l \${wifi\_ifname}}

    1. Set NVRAM

    :   

        \* Networking:

        :   -   wl0\_radio=1
            -   wl0\_infra=1
            -   wl0\_ssid=''&lt;your SSID&gt;''
            -   wl0\_mode=sta

            \* wl0\_akm=psk2

            :   -   psk is also acceptable, but use wl0\_crypto=tkip
                -   "psk psk2" will not work - pick only one

            \* wl0\_crypto=aes+tkip

            :   -   This is the group=tkip, pairwise=aes mix that is
                    commonly used

            -   wl0\_wpa\_psk=''&lt;your psk&gt;'' (in ASCII)

        \* Break the bridge:

        :   

            \* Note:

            :   -   The built-in hardware bridge would see a mix of
                    encrypted and unencrypted frames, so the bridging
                    needs to be done in software.
                -   If you are using vlan1 (or whatever wan\_ifname is)
                    then you will need to unset wan\_ifname and change
                    the failsafes in S05nvram.
                -   Change these interface names to match your
                    hardware - these work for the WRT54G v4 and similar
                    hardware.

            -   lan\_ifname=vlan0 ''(Oddly, eth0 here seems not to
                work.)''
            -   wifi\_ifname=eth1

        \* Enable DHCP on wireless side:

        :   -   wifi\_proto=dhcp

        \* Put all LAN ports on vlan0:

        :   -   unset vlan1ports
            -   unset vlan1hwname
            -   vlan0ports="4 3 2 1 0 5\*"

    1.  Double-check everything, then mentally prepare yourself for a
        bricking. (Failsafe mode should still work fine, but who knows?
        I bricked mine enough times while figuring all of this out that
        the circuit board is sitting naked on top of a stack of paper as
        I type this.)
    2.  nvram commit
    3.  reboot

{i} FIXME: DHCP over the bridge works for me without setting up a dhcp
forwarder (OpenWRT 1.0-RC3 on Linksys WRT54GS V4) And why break the
bridge? I did not and everything works... -- MarcSchiffbauer
\[\[DateTime(2005-11-23T14:17:24Z)\]\]

Are you using WPA2? The hardware bridge works fine without encryption;
and if you're using the hardware bridge, broadcasting (such as for DHCP)
will also work fine. When I have br0 connecting eth1+vlan0, with WPA2,
the encryption negotiation fails. I'd be very happy if this weren't the
case! -- \["wmono"\] \[\[DateTime(2005-11-23T17:44:06Z)\]\]

I saw the same thing. Everything configured, no joy, broke the bridge
and rebooted, connected. Funny thing is my other AP in AP mode, both
running WR RC4, doesn't have a problem with the bridge intact. I'm going
to investigate soon, but for now it seems like \["wmono"\]'s right. --
PeterKahle \[\[DateTime(2005-11-30T04:50:35Z)\]\]

OK, I stand corrected. It seems to work. I'm using WPA, not WPA2, but
somehow it's working. Only setting differences are lan\_ifname=br0,
lan\_ifnames=vlan0 eth1, wl0\_mode=wet, wl0\_akm=psk, and
wl0\_crypto=tkip. I may try WPA2 later, but for now this is good enough.
-- PeterKahle \[\[DateTime(2005-12-01T06:54:02Z)\]\]

It seems either possible to run the bridge with WPA (as reported by
PeterKahle) or to use WPA2 in wet mode without a layer 2 bridge (but you
can still use IP forwarding and ARP proxy; lan\_ifname=vlan0
wifi\_ifname=eth1 wl0\_mode=wet wl0\_akm=psk2 wl0\_crypto=aes+tkip) --
GeorgLukas \[\[DateTime(2006-02-09T12:34:23Z)\]\]

Should that be wl0\_mode=sta not wl0\_mode=wet? I tidied the
''rationale'' section to make it clear this procedure is only needed for
WPA2 together with bridged client mode. I have tested WPA1 bridged
client, and WPA2 routed client, and both worked without this procedure.
In fact, calling this "WPA2 bridged client" is rather misleading; the
box is still really a router, it's just using ARP trickery to fake
itself as the next-hop. It's not a genuine bridge, since non-IP frames
would not be passed. -- BrianCandler

Thanks BrianCandler, you're quite right: this is not really a bridge,
but I think it's close as one can get without the use of wet mode. If
you (or anyone else) can make a proper bridge using WPA2 then please
replace this page with instructions on how to do so. -- \["wmono"\]
\[\[DateTime\]\]

Turns out that even wet mode is not a true bridge - it does ARP
masquerading. See <http://forum.openwrt.org/viewtopic.php?id=5105> --
BrianCandler \[\[DateTime(2006-04-25T08:12:00Z)\]\]

Thank you guys, this howto work perfectly for me with white russian 4.
To make this a bit more simple I use a static IP for the wlan interface,
so I don't need to wait to get an IP and so I remove the sleep script.
-- \[:RafMazBrianCandler:RafMaz\] \[\[DateTime\]\]

I was trying now for weeks to set up wet mode but it didn't work out.
Finally I looked through the source of wlconfig.c (/sbin/wifi) and
*gotcha*: Don't use a WPA key longer than 63 chars (minlength is 8). --
Huedi \[\[DateTime(2007-03-09T20:23:00Z)\]\]

== S47sleep == {{{ \#!/bin/sh \# S47sleep - Delay before starting
services \# Sometimes the interfaces take a while to come up after being
started. \# This script simply sleeps for 20 seconds while flashing the
Power LED, \# giving enough time for the network to come up before
continuing.

DIAG=\`cat /proc/sys/diag\`

echo 0x05 &gt; /proc/sys/diag sleep 20 echo \${DIAG} &gt; /proc/sys/diag
}}}

== S50dhcp-fwd == The DHCP forwarder (dhcp-fwd) configuration file
contains several hard-coded values that are better being detected from
NVRAM and the current network configuration. This start-up script
queries those sources and writes a configuration file tailored to the
current environment, then starts dhcp-fwd using that configuration file.

{{{ \#!/bin/sh

\# /etc/init.d/S50dhcp-fwd \# Runs dhcp-fwd after creating configuration
file

\# Start configuration section LOG\_DIR=/var/log RUN\_DIR=/var/run
JAIL\_DIR=\${RUN\_DIR}/dhcp-fwd PID\_FILE=\${RUN\_DIR}/dhcp-fwd.pid
CFG\_FILE=\${RUN\_DIR}/dhcp-fwd.conf LOG\_FILE=\${LOG\_DIR}/dhcp-fwd.log
\# End configuration section

. /etc/functions.sh

WIFI\_IF=\$(nvram get wifi\_ifname) LAN\_IF=\$(nvram get lan\_ifname)

GIADDR=\`ifconfig
 | awk 'BEGIN { RS="nn" } /\^'\${WIFI\_IF}' / { print \$7 }'
 | cut -d ':' -f 2\`

if \[ "\$GIADDR" = "" \]; then

:   logger -s "Unable to detect GIADDR - no IP address on \$IFACE?" exit
    1

fi

createdirs () {

:   \[ -e \$LOG\_DIR \] && \[ ! -d \$LOG\_DIR \] && rm -f \$LOG\_DIR \[
    ! -d \$LOG\_DIR \] && mkdir -p \$LOG\_DIR

    \[ -e \$RUN\_DIR \] && \[ ! -d \$RUN\_DIR \] && rm -f \$RUN\_DIR \[
    ! -d \$RUN\_DIR \] && mkdir -p \$RUN\_DIR

    \[ -e \$JAIL\_DIR \] && \[ ! -d \$JAIL\_DIR \] && rm -f \$JAIL\_DIR
    \[ ! -d \$JAIL\_DIR \] && mkdir -p \$JAIL\_DIR

}

createcfg () {

:   cat &lt;&lt; EOF &gt; \$CFG\_FILE

\# This file was generated automatically by \$0 - Do not edit!

user 0 group 0 chroot \$JAIL\_DIR

logfile \$LOG\_FILE loglevel 1

pidfile \$PID\_FILE

ulimit core 0 ulimit stack 64K ulimit data 32K ulimit rss 200K ulimit
nproc 0 ulimit nofile 0 ulimit as 0

\# IFNAME clients servers bcast if \$LAN\_IF true false true if
\$WIFI\_IF false true true

server bcast \$WIFI\_IF

ip \$LAN\_IF \$GIADDR

EOF }

startdhcpfwd () {

:   dhcp-fwd -c \$CFG\_FILE

}

killdhcpfwd () {

:   \[ -f \$PID\_FILE \] && kill cat \$PID\_FILE

}

case \$1 in

:   

    start)

    :   createdirs createcfg startdhcpfwd ;;

    stop)

    :   killdhcpfwd ;;

    \*)

    :   echo "usage: \$0 start|stop" exit 1

esac

exit \$? }}}

== Testing it out == At this point, you should have a more or less
working wireless bridge: plug something in the LAN port and it'll be
virtually connected to the same network as your other wireless clients.

Note the delay in starting up - if there's a power failure to the
bridge, the DHCP clients behnid the bridge must be willing to wait a
while before giving up on getting a lease. On UNIX, this may involve
adding a S47sleep-like script on the client hosts, too. Windows systems
may have problems with this arrangement.

As noted in the parprouted documentation, broadcasting will not cross
the bridge. DHCP relaying was added as a special case. If you have other
applications that use broadcast, you'll have to work around those, too.

== Troubleshooting == This section needs to be expanded. If you try this
and it doesn't work, please list some things you tried (and why) here
for the benefit of future readers.

> \* Check that the wireless connection is up:
>
> :   1.  Set a machine to a static IP address on the same subnet as the
>         lan\_ipaddr and ssh in.
>     2.  Try ''wl assoclist'' to see if the bridge has associated with
>         the AP. (The AP's MAC address appears if so.)
>
>     1. Try ''wl sta\_info &lt;AP MAC address&gt;'' to see how far the connection has gone.
>
>     :   -   ASSOCIATED AUTHENTICATED AUTHORIZED is fully connected on
>             the transport layer.
>         -   ASSOCIATED AUTHENTICATED probably means the encryption is
>             not correct; double-check the wl0\_akm and wl0\_crypto and
>             wl0\_psk\_key variables.
>
>     1.  Look at ''iwconfig eth1'' - the Encryption: field should show
>         a key, not "off".
>
== Confirmation == If you follow this how-to, please note here if it
worked or didn't work for you!

=== WRT54GL === I got this working on a pair of WRT54GLs. This was my
first openWRT hack, so it took a little longer then it should have, but
I eventually got it working. I'm currently using WPA (PSK) +tkip, when I
get a chance I'll try enabling aes. I modified the S50dhcp-fwd command
by changing this:

{{{ GIADDR=\`ifconfig
 | awk 'BEGIN { RS="nn" } /\^'\${WIFI\_IF}' / { print \$7 }'
 | cut -d ':' -f 2\`

if \[ "\$GIADDR" = "" \]; then

:   logger -s "Unable to detect GIADDR - no IP address on \$IFACE?" exit
    1

fi
==

to

{{{ getip () { GIADDR=\`ifconfig \${WIFI\_IF} | awk '/inet addr:/ {
print \$2 }' | cut -d ':' -f 2\` }

DIAG=\`cat /proc/sys/diag\` echo 0x05 &gt; /proc/sys/diag getip

i=1 while \[ \$i -lt 120 \] do if \[ x"\$GIADDR" != x \]; then break; fi

> i = expr \$i + 1 sleep 1 getip

done

echo \${DIAG} &gt; /proc/sys/diag

if \[ x"\$GIADDR" = x \]; then echo "Error could not determine IP for
\${WIFI\_IF}" &gt; \$CFG\_FILE exit 0 fi }}}

and dropping the S47sleep script all together. For this to work, you
need to create the following symbolic link:

{{{ ln -s /bin/busybox /bin/expr }}}

This enables the expr functionality of busybox, which is required to
maintain the counter in the script.

This change causes the S50dhcp-fwd script to wait until the wireless
network interface has an ip before continuing. After 120 seconds it
gives up and exits. I found that the S47sleep script did not always wait
long enough.

=== WRT54GL (EU model, version 1.1, serial CL7B\*) ===

After some screaming and shouting, I also managed to have the same setup
on my WRT54GL running White Russian RC5. It was anything but painless,
though. Here are some notes for those who wish to avoid some pain:

> 1.  The first attempt ended in a mess, there were way too many unused
>     and apparently contradictory nvram variables flying around. Since
>     it is considered safe for this model -- which is apparently
>     identical to a WRT54G v4.0 (see
>     <http://wiki.openwrt.org/OpenWrtDocs/Hardware/Linksys/WRT54G>) --
>     a "mtd erase nvram" brought things back to zero before the second
>     attempt. After my experience, I would at least recommend a cleanup
>     the "safe way" (see <http://wiki.openwrt.org/Faq> item 2.7).
> 2.  It took quite a while to find a setting that would work for
>     WPA/WPA2 authentication. WEP always worked without any problems,
>     but it seemed impossible to set up WPA PSK authentication.
>     Although everything looked fine, the encryption key simply never
>     got set (see above for the troubleshooting on encryption). Some
>     attempts brought error messages that looked obscure, but did not
>     help much. The short version is: I '''seriously''' recommend to
>     systematically try all combinations before changing the other
>     settings substantially. In my case the only combination that works
>     is WPA2-PSK with AES. Here is the nvram dump of the wireless
>     settings for your reference:
>
> > {{{

wifi\_dns=192.168.1.1 wifi\_gateway=192.168.1.1 wifi\_ifname=eth1
wifi\_ipaddr=192.168.1.2 wifi\_netmask=255.255.255.0 wifi\_proto=static
wl0\_akm=psk2 wl0\_crypto=aes wl0\_ifname=eth1 wl0\_infra=1
wl0\_mode=sta wl0\_radio=1 wl0\_ssid=&lt;my essid&gt;
wl0\_wpa\_psk=&lt;my ASCII psk&gt; }}}

> 3.  I also made the same modifications you can see in the WRT54GL
>     experience report, and much for the same reasons. They seemed to
>     improve the situation.
> 4.  Final caveat: the parprouted configuration in
>     /etc/default/parprouted needed some tweaking to get its
>     information from nvram instead of expecting it in environment
>     variables. Otherwise it won't start.

Here is where it leaves the Bridge HOWTO, I guess, but I'll still share
the last point:

> 5.  Ultimately I decided that I did not feel comfortable with the ARP
>     fiddling. I saw some strange network hickups and bandwidth
>     problems. So I ended up removing the parprouted and
>     dhcp-forwarder, configuring the device as a router between two
>     subnets in 192.168.*.* and setting up dnsmasq for DHCP serving and
>     DNS caching. This now works stable and fast as far as current
>     experience goes.

== Appendix: Sample NVRAM configuration == {{{ <root@OpenWRT>:\~\# nvram
show | sort ... lan\_ifname=vlan0 lan\_ifnames=vlan0 eth1 eth2 \# This
is set by S05nvram and is not needed lan\_ipaddr=192.168.1.1 \# This
value doesn't matter lan\_netmask=255.255.255.0 lan\_proto=static ...
vlan0hwname=et0 vlan0ports=4 3 2 1 0 5\* ... wifi\_ifname=eth1
wifi\_proto=dhcp ... wl0\_akm=psk2 wl0\_crypto=aes+tkip wl0\_ifname=eth1
wl0\_infra=1 wl0\_mode=sta wl0\_radio=1 wl0\_ssid=&lt;&lt;SSID&gt;&gt;
wl0\_wpa\_psk=&lt;&lt;PSK&gt;&gt; ... }}}

CategoryWhiteRussian
