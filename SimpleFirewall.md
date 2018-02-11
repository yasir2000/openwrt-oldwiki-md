I need to forward and allow external access to various ports, but
editing the default firewall.user is error-prone and complex. Here's an
example of what my firewall uses:

file: /etc/firewall.user

{{{

\#!/bin/sh . /etc/fwlib.sh flush\_firewall

\#\#\# Ports accessible on the router from the WAN \# allow\_tcp\_port
22 \# SSH \# allow\_tcp\_port 465 \# HTTPS

\#\#\# Ports accessible from specific hosts to the router from the WAN
allow\_tcp\_port\_fromhost 80 remote\_access \# HTTP
allow\_tcp\_port\_fromhost 22 remote\_access \# HTTP

\#\#\# Ports accessible to client machines. \# forward\_port 22 server
forward\_port 9100 printer\_01

\#\#\# if we really need \_[all]() ports... \# register\_dmz server

\# forward workstation port for application development \#forward\_port
8080 workstation1

\# forward a few utility port-ranges to make it easier to deal with \#
bittorrent configurations and the like \# forward\_port 10000:10099
workstation1 \# forward\_port 10100:10199 laptop1 \# forward\_port
10200:10299 laptop2

\#\#\# Translate port for client machines. translate\_port 8080
printer\_01 80

\#\#\# Trusted hosts, full access to router \# trusted\_host
my\_support\_company

}}}

And here's the extremely simple firewall library that it uses:

'''Note:''' if you plan on pasting this script into an ssh window, note
that the character quoted in the {{{cut}}} command below should probably
be a tab, not a space (unless you use spaces to format your /etc/hosts
file). ('''''Not relevant now as cut has been replaced with awk''''')

file: /etc/hosts

{{{

127.0.0.1 localhost OpenWrt 192.168.1.100 server 192.168.1.50
workstation1 192.168.1.80 laptop1 192.168.1.81 laptop2 195.xxx.xxx.xxx
my\_support\_company 192.168.1.55 printer\_01 195.xxx.xxx.xxx
remote\_access xxx.xxx.xxx.xxx router xxx.xxx.0.0 lan }}}

file: /etc/fwlib.sh

{{{

\#!/bin/sh

. /etc/functions.sh

WAN=\$(nvram get wan\_ifname) LAN=\$(nvram get lan\_ifname)

flush\_firewall () {

:   iptables -F input\_rule iptables -F output\_rule iptables -F
    forwarding\_rule iptables -t nat -F prerouting\_rule iptables -t nat
    -F postrouting\_rule

    \#create logdrop rule iptables -N LOGDROP iptables -A LOGDROP -j LOG
    --log-level warning --log-prefix 'BLOCKED: ' --log-tcp-sequence
    --log-ip-options --log-tcp-options iptables -A LOGDROP -j DROP

}

\#\#\# BIG FAT DISCLAIMER \#\#\# The "-i \$WAN" literally means packets
that came in over the \$WAN interface; \#\#\# this WILL NOT MATCH
packets sent from the LAN to the WAN address.

allow\_tcp\_port () {

:   ALLOWPORT=\$1 iptables -t nat -A prerouting\_rule -i \$WAN -p tcp
    --dport \$ALLOWPORT -j ACCEPT iptables -A input\_rule -i \$WAN -p
    tcp --dport \$ALLOWPORT -j ACCEPT

}

allow\_tcp\_port\_fromhost () {

:   ALLOWPORT=\$1 ALLOWHOSTNAME=\$2 ALLOWHOST=\`sucky\_resolve
    \$ALLOWHOSTNAME\` echo "Allowing tcp from \$ALLOWHOSTNAME to port
    \$ALLOWPORT" iptables -t nat -A prerouting\_rule -i \$WAN -p tcp -s
    \$ALLOWHOST --dport \$ALLOWPORT -j ACCEPT iptables -A input\_rule -i
    \$WAN -p tcp -s \$ALLOWHOST --dport \$ALLOWPORT -j ACCEPT

}

sucky\_resolve () {

:   HOSTNAME=\$1 \#\#\# grep \$HOSTNAME /etc/hosts | awk '{ print \$1 }'

}

forward\_port() {

:   ALLOWPORT=\$1 ALLOWHOSTNAME=\$2 ALLOWHOST=\`sucky\_resolve
    \$ALLOWHOSTNAME\` ROUTER=\`sucky\_resolve router\`
    LAN=\`sucky\_resolve lan\`

    echo "FORWARDING \$ALLOWPORT TO \$ALLOWHOSTNAME (\$ALLOWHOST)"

    \# Original outside to WAN forwarding lines \#iptables -t nat -A
    prerouting\_rule -i \$WAN -p tcp --dport \$ALLOWPORT -j DNAT --to
    \$ALLOWHOST \#iptables -A forwarding\_rule -i \$WAN -p tcp --dport
    \$ALLOWPORT -d \$ALLOWHOST -j ACCEPT

    \# Updated to handle LAN-&gt;WAN and external -&gt; WAN \# your
    router needs to be in hosts, see updated hosts iptables -t nat -A
    prerouting\_rule -d \$ROUTER -p tcp --dport \$ALLOWPORT -j DNAT --to
    \$ALLOWHOST iptables -A forwarding\_wan -p tcp --dport \$ALLOWPORT
    -d \$ALLOWHOST -j ACCEPT iptables -t nat -A postrouting\_rule -s
    \$LAN/12 -p tcp --dport \$ALLOWPORT -d \$ALLOWHOST -j MASQUERADE

}

translate\_port() {

:   ALLOWPORT=\$1 ALLOWHOSTNAME=\$2 ALLOWHOSTPORT=\$3
    ALLOWHOST=\`sucky\_resolve \$ALLOWHOSTNAME\` echo "TRANSLATING
    \$ALLOWPORT TO \$ALLOWHOSTNAME (\$ALLOWHOST:\$ALLOWHOSTPORT)"
    iptables -t nat -A prerouting\_rule -i \$WAN -p tcp --dport
    \$ALLOWPORT -j DNAT --to \$ALLOWHOST:\$ALLOWHOSTPORT iptables -A
    forwarding\_rule -i \$WAN -p tcp --dport \$ALLOWHOSTPORT -d
    \$ALLOWHOST -j ACCEPT

}

trusted\_host (){

:   ALLOWHOSTNAME=\$1 TRUSTEDHOST=\`sucky\_resolve \$ALLOWHOSTNAME\`
    iptables -t nat -A prerouting\_rule -i \$WAN -p tcp -s \$TRUSTEDHOST
    -j ACCEPT iptables -A input\_rule -i \$WAN -p tcp -s \$TRUSTEDHOST
    -j ACCEPT

}

drop\_incoming (){

:   echo "BLOCKING INCOMING PACKETS FROM \$1" iptables -A input\_rule -i
    \$WAN -s \$1 -p tcp -j LOGDROP

}

}}}
