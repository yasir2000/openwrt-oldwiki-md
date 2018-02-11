= DDNS howto = \[\[TableOfContents\]\]

== About Dynamic DNS (DDNS) == The DDNS service comes in handy for
establishing connections from computers on the Internet to your network
at home. This is especially useful if you want to run server software or
SSH on your !OpenWrt and only have a dynamic IP.

!OpenWrt uses the package {{{ddns-scripts}}} for providing DDNS service.

== Requirements ==

:   -   A recent !OpenWrt version. This howto was written for the
        'Kamikaze 7.07' and later releases.

    \* An account with a compatible DDNS service, currently

    :   -   dyndns.org
        -   changeip.com
        -   zoneedit.com
        -   no-ip.com
        -   freedns.afraid.org
        -   Any other that can update when some URL is accessed. The
            script's quite versatile.

== Installation == Install the ddns-scripts package.

{{{ <root@OpenWrt>:\~\# opkg install ddns-scripts }}} If you like to
configure {{{ddns-scripts}}} using the LuCI WebUI also install this
package:

{{{ <root@OpenWrt>:\~\# opkg install luci-app-ddns }}} == Configuration
== The configuration is stored in /etc/config/ddns which contains more
thorough documentation.

In order to enable Dynamic DNS you need at least one section, and in
that section the "enabled" option must be set to one.

Each section represents an update to a different service. This sections
specifies several things:

> -   service (dyndns.org, etc.)
> -   domain
> -   username
> -   password
> -   IP source (wan, eth0, web)

Optionally, thse following may be specified:

> -   option update\_url (needed if the service isn't supported by
>     /usr/lib/ddns/services)
> -   check\_interval
> -   force\_interval

Use the "check\_interval" to specify how often to check whether an
update is necessary, and the "force\_interval" option to specify how
often to force an update. Specify the units for these values with the
"check\_unit" and the "force\_unit" options. Units can be "days",
"hours", "minutes" or "seconds". The default force\_unit is hours and
the default check\_unit is seconds. The default check\_interval is 600
seconds, or ten minutes. The default force\_interval is 72 hours or 3
days.

{{{ config service "myddns" option enabled "0" option service\_name
"dyndns.org" option domain "mypersonaldomain.dyndns.org" option username
"myusername" option password "mypassword" option ip\_source "network"
option ip\_network "wan" option force\_interval "72" option force\_unit
"hours" option check\_interval "10" option check\_unit "minutes"
\#option ip\_source "interface" \#option ip\_interface "eth0.1" \#option
ip\_source "web" \#option ip\_url
"<http://www.whatismyip.com/automation/n09230945.asp>" \#option
update\_url
"<http://%5BUSERNAME%5D>:\[PASSWORD\]@members.dyndns.org/nic/update?hostname=\[DOMAIN\]&myip=\[IP\]"
}}} A short example for a dyndns.org service to configure via UCI CLI:

{{{ <root@OpenWrt>:\~\# uci set ddns.myddns.enabled=1
<root@OpenWrt>:\~\# uci set ddns.myddns.domain=host.dyndns.org
<root@OpenWrt>:\~\# uci set ddns.myddns.username=&lt;username&gt;
<root@OpenWrt>:\~\# uci set ddns.myddns.password=&lt;password&gt;
<root@OpenWrt>:\~\# uci set ddns.myddns.enabled=1 <root@OpenWrt>:\~\#
uci commit ddns <root@OpenWrt>:\~\# ACTION=ifup INTERFACE=wan
/sbin/hotplug-call iface }}} == Trying it out == The script runs when
hotplug events happen or a monitored IP address changes, so initially,
you have to start it manually. After setting "enabled" to 1, run the
following:

{{{ sh . /usr/lib/ddns/dynamic\_dns\_functions.sh \# note the leading
period start\_daemon\_for\_all\_ddns\_sections exit }}} You can also
simulate a hotplug event to trigger a DDNS update manually:

{{{ <root@OpenWrt>:\~\# ACTION=ifup INTERFACE=wan /sbin/hotplug-call
iface }}} == Tweaks == === dyndns.org === Full API documentation
available here: <https://www.dyndns.com/developers/specs/syntax.html>

To enable wildcard domains (\*.foo.dyndns.org) on dyndns.org, replace
the line in {{{/usr/lib/ddns/services}}} with this:

{{{ "dyndns.org"
"<http://%5BUSERNAME%5D>:\[PASSWORD\]@members.dyndns.org/nic/update?wildcard=ON&hostname=\[DOMAIN\]&myip=\[IP\]"
}}} To retain the wildcard setting on dyndns.org, replace the line in
{{{/usr/lib/ddns/services}}} with this:

{{{ "dyndns.org"
"<http://%5BUSERNAME%5D>:\[PASSWORD\]@members.dyndns.org/nic/update?wildcard=NOCHG&hostname=\[DOMAIN\]&myip=\[IP\]"
}}} = Old methods = DDNS scripts have been a surprisingly complicated
part of OpenWrt. There have been many other scripts and packages used.

> -   ez-ipupdate (no longer maintained?)
> -   \[:OpenWrtDocs/Kamikaze/UpdateDD:updatedd\] (no longer
>     maintained?) Supports many other services
> -   \[<http://forum.openwrt.org/viewtopic.php?pid=48762> JimWright's
>     White Russian dyndns.org script\] (probably based off a script mbm
>     posted in the forum)
>
> \* \[<http://forum.openwrt.org/viewtopic.php?id=14040> exobyte's Kamikaze dyndns.org script (based of JimWright's)\]
>
> :   -   ddns-scripts's dyndns.org support was
>         \[<http://www.mail-archive.com/openwrt-devel@lists.openwrt.org/msg00922.html>
>         based off\] this
>
CategoryHowTo
