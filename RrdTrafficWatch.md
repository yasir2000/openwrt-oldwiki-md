= Downloading needed things =

:   

    \* first you have to add Nico's testing directory to your /etc/ipkg.conf

    :   . add {{{ src unstable
        <http://downloads.openwrt.org/people/nico/testing/mipsel/packages/>
        }}} to your ipkg.conf . '''The URL above is not valid!''' . Use
        {{{ src unstable <http://home.mag.cx/openwrt/packages/> }}}
        instead . '''This URL stopped working 2006-08-11''' but you can
        download and install librrd and rrdtool1 from
        <http://nthill.free.fr/openwrt/ipkg/testing/> . home.mag.cx
        works again

    \* update your ipkg database and install the needed rrdtool

    :   . {{{ ipkg update ipkg install rrdtool1 }}} now you should have
        rrdtool and rrdupdate installed

    \* have a look if your iptables are able to monitor traffic

    :   . {{{ iptables -N traffic iptables -F traffic iptables -I
        FORWARD -j traffic }}} if you receive an error make sure \* you
        have all necessary packages installed \* the kernel-modules are
        loaded \* make sure, that the traffic-chain exists after
        booting. (Just add the iptable-commands to a start-up script.
        Remember to load the needed kernel-modules befor running the
        iptable-stuff) \* To Create the start-up script: {{{ cat &gt;
        /etc/init.d/S50rrd }}} {{{ \#!/bin/sh iptables -N traffic
        iptables -F traffic iptables -I FORWARD -j traffic }}} After
        pasting it, press {{{ENTER}}} and then hit {{{CTRL-D}}} keys to
        save the file. Now make sure the init script is executable and
        start it with {{{ chmod a+x /etc/init.d/S50rrd
        /etc/init.d/S50rrd start }}} now you are capable to monitor
        traffic with iptables

= Using the traff\_graph - script = Place the following script in /sbin
or where you like

{{{ cat &gt; /sbin/traff\_graph }}} {{{ \#!/bin/sh

\# traff\_graph version 0.0.2 by twist \# script for monitoring
mac-based traffic on an openwrt-box \# comments, suggestion, patches
.... --&gt; twist \_[at]() evilhome \_[dot]() de

SITENAME="tuxland" \# change for your site

mkdir -p /tmp/rrd

iptables -L traffic -vnxZ -t filter &gt; /tmp/traffic.tmp

ALL\_UP=0 ALL\_DOWN=0
INDEX="&lt;html&gt;&lt;head&gt;&lt;title&gt;traffic @
\$SITENAME&lt;/title&gt;&lt;/head&gt;&lt;body&gt;"

\# \$1 = ImageFile, \$2 = Time in secs to go back, \$3 = RRDfile, \$4 =
GraphText CreateGraph () { \# only run, if no other rrdtool is running
if \[ -n "\$(ps | grep rrdtool | grep -v grep)" \] ; then return fi

> rrdtool graph "\${1}" -a PNG -s -"\${2}" -w 550 -h 240 -v "bytes/s"
>  'DEF:in='\${3}':down:AVERAGE'
>  'DEF:out='\${3}':up:AVERAGE'
>  'CDEF:out\_neg=out,-1,\*'
>  'AREA:in\#32CD32:Incoming'
>  'LINE1:in\#336600'
>  GPRINT:in:"MAX: Max\\: %5.1lf %s"
>  GPRINT:in:"AVERAGE: Avg\\: %5.1lf %S"
>  GPRINT:in:"LAST: Current\\: %5.1lf %Sbytes/sec\\n"
>  'AREA:out\_neg\#4169E1:Outgoing'
>  'LINE1:out\_neg\#0033CC'
>  GPRINT:out:"MAX: Max\\: %5.1lf %S"
>  GPRINT:out:"AVERAGE: Avg\\: %5.1lf %S"
>  GPRINT:out:"LAST: Current\\: %5.1lf %Sbytes/sec"
>  'HRULE:0\#000000' -t "\${4}"

}

for MAC in \$(cat /proc/net/arp | grep -v address | awk '{print \$4}') ; do

:   [MAC]()=\$(echo \$MAC | sed 's/:/-/g') IP=\$(cat /proc/net/arp |
    grep \$MAC | awk '{print \$1}') \# This assumes that a local dns
    server (like dnsmasq) is running NAME=\$(nslookup \$IP 127.0.0.1 |
    grep "Name:" | awk '{print \$2}') \# echo "mac: \$MAC ip: \$[IP]()
    name: \$NAME"

    UP=\$(cat /tmp/traffic.tmp | awk '{print \$2 " " \$7}' | grep \$IP |
    awk '{print \$1}' | tr -d 'n' ) UP=\$((\$UP+0))
    ALL\_UP=\$((\$ALL\_UP+\$UP)) DOWN=\$(cat /tmp/traffic.tmp | awk
    '{print \$2 " " \$8}' | grep \$IP | awk '{print \$1}' | tr -d 'n' )
    DOWN=\$((\$DOWN+0)) ALL\_DOWN=\$((\$ALL\_DOWN+\$DOWN))

COUNTIP=\$(iptables -vnL traffic | grep \$IP | wc -l | awk '{print \$1}')

:   

    if \[ "\$COUNTIP" -eq 0 \] ; then

    :   iptables -A traffic -s \$IP iptables -A traffic -d \$IP

    fi

    > \# create db if not exists if \[ ! -e /tmp/rrd/\${[MAC]()}.rrd \]
    > ; then \# echo creating /tmp/rrd/\${[MAC]()}.rrd rrdtool create
    > /tmp/rrd/\${[MAC]()}.rrd -s 300
    >  DS:up:ABSOLUTE:600:0:600000000
    >  DS:down:ABSOLUTE:600:0:600000000
    >  RRA:AVERAGE:0.5:1:2016
    >  RRA:AVERAGE:0.5:3:2688
    >  RRA:AVERAGE:0.5:12:6360 fi
    >
    > \# echo "up: \$UP down: \$DOWN" rrdtool update
    > /tmp/rrd/\${[MAC]()}.rrd N:\$UP:\$DOWN
    >
    > CreateGraph "/tmp/rrd/\${[MAC]()}.day.png" 86400
    > /tmp/rrd/\${[MAC]()}.rrd "IP: \$IP MAC: \$[MAC]() Host: \$NAME"
    > INDEX=\$INDEX"&lt;img src='\${[MAC]()}.day.png'&gt;&lt;br&gt;"
    >
    > \# traffic/week \# i don´t use this \# CreateGraph
    > "/tmp/rrd/\${[MAC]()}.week.png" 604800 /tmp/rrd/\${[MAC]()}.rrd
    > "IP: \$IP MAC: \$[MAC]() Host: \$NAME" \# INDEX=\$INDEX"&lt;img
    > src='\${[MAC]()}.week.png'&gt;&lt;br&gt;"

done

\# build sum-graph if \[ ! -e /tmp/rrd/all.rrd \] ; then rrdtool create
/tmp/rrd/all.rrd -s 300
 DS:up:ABSOLUTE:600:0:600000000
 DS:down:ABSOLUTE:600:0:600000000
 RRA:AVERAGE:0.5:1:2016
 RRA:AVERAGE:0.5:3:2688
 RRA:AVERAGE:0.5:12:6360 fi

rrdtool update /tmp/rrd/all.rrd N:\$ALL\_UP:\$ALL\_DOWN CreateGraph
/tmp/rrd/all.png 86400 /tmp/rrd/all.rrd "all traffic from \$SITENAME"

INDEX=\$INDEX"&lt;br&gt;&lt;img
src='all.png'&gt;&lt;/body&gt;&lt;/html&gt;"

echo \$INDEX &gt; /tmp/rrd/index.html }}} Make the file executable

{{{ chmod a+x /sbin/traff\_graph }}} This script will create and update
the rrd-database for each mac found in /proc/net/arp. If a host is not
online no update will be performed. This will safe some cpu-cycles :) .
traff\_graph stores the rrd-db, the created pictures/graphs and the
index.html for viewing the graphs in /tmp/rrd. This means, after a
reboot all informations are lost and you will start at 0.

Now you can test traff\_graph. Make sure, you have only a single
traffic-chain/host in your iptable rules. You can list this with

{{{ iptables -L traffic -vx}}} Now run traff\_graph. This will need a
while... get a coffee ;-) Add traff\_graph to your crontab and run it
every 5 minutes. Be carefull not to monitore to much hosts since rrdtool
graph needs a lot of time. For viewing the graphs you have to add an
symlink in /www which points to /tmp/rrd.

{{{ cd /www ln -s /tmp/rrd/ traffic }}} everything will be available via

{{{ <http://192.168.0.1/traffic/> }}} To schedule an update every 5
minutes, use crontab.

Add this to the /etc/crontabs/root file :

{{{ \# create traffic graphs every 5 minutes (i.e. run if minutes mod 5
== 0) 0-55/5 \* \* \* \* /sbin/traff\_graph &gt; /dev/null 2&gt;&1}}} Do
not forget to enable cron. See HowtoEnableCron

= Other links = <http://forum.openwrt.org/viewtopic.php?id=3741>