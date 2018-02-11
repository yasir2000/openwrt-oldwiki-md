\#\# Please edit system and help pages ONLY in the moinmaster wiki! For
more \#\# information, please see MoinMaster:MoinPagesEditorGroup.
\#\#acl MoinPagesEditorGroup:read,write,delete,revert All:read
\#\#master-page:HelpTemplate \#\#master-date:Unknown-Date \#format wiki
\#language en = Traffic logging with ULOG & MySQL =

This procedure has been implemented in a RC9 openwrt but it should also
work in RC6 (though you're advised to *update*). Assuming you have
enough knowledge to work your way through a linux console this tutorial
should be fairly easy to follow.

== Installing ULOG & requisites ==

''ssh into your openwrt box first''

{{{ ipkg update ipkg install ulogd ulogd-mod-mysql kmod-ipt-ulog
iptables-mod-ulog }}}

== Setting up your MySQL database ==

''copy & paste these lines into a text file in your linux box (the one
running the mysql server), name it ulog-mysql.sql''

{{{ CREATE TABLE ulog ( id INT UNSIGNED AUTO\_INCREMENT UNIQUE,

> raw\_mac VARCHAR(80),
>
> oob\_time\_sec INT UNSIGNED, oob\_time\_usec INT UNSIGNED, oob\_prefix
> VARCHAR(32), oob\_mark INT UNSIGNED, oob\_in VARCHAR(32), oob\_out
> VARCHAR(32),
>
> ip\_saddr INT UNSIGNED, ip\_daddr INT UNSIGNED, ip\_protocol TINYINT
> UNSIGNED, ip\_tos TINYINT UNSIGNED, ip\_ttl TINYINT UNSIGNED,
> ip\_totlen SMALLINT UNSIGNED, ip\_ihl TINYINT UNSIGNED, ip\_csum
> SMALLINT UNSIGNED, ip\_id SMALLINT UNSIGNED, ip\_fragoff SMALLINT
> UNSIGNED,
>
> tcp\_sport SMALLINT UNSIGNED, tcp\_dport SMALLINT UNSIGNED, tcp\_seq
> INT UNSIGNED, tcp\_ackseq INT UNSIGNED, tcp\_window SMALLINT UNSIGNED,
> tcp\_urg TINYINT, tcp\_urgp SMALLINT UNSIGNED, tcp\_ack TINYINT,
> tcp\_psh TINYINT, tcp\_rst TINYINT, tcp\_syn TINYINT, tcp\_fin
> TINYINT,
>
> udp\_sport SMALLINT UNSIGNED, udp\_dport SMALLINT UNSIGNED, udp\_len
> SMALLINT UNSIGNED,
>
> icmp\_type TINYINT UNSIGNED, icmp\_code TINYINT UNSIGNED, icmp\_echoid
> SMALLINT UNSIGNED, icmp\_echoseq SMALLINT UNSIGNED, icmp\_gateway INT
> UNSIGNED, icmp\_fragmtu SMALLINT UNSIGNED,
>
> pwsniff\_user VARCHAR(30), pwsniff\_pass VARCHAR(30),
>
> ahesp\_spi INT UNSIGNED,
>
> KEY index\_id (id)

> );

}}}

''now create the database, the table and the user''

:   -   ''create database named ulogdb and select it''
    -   ''create table ulog from description given in 'ulog-mysql.sql'
        ''
    -   ''create user 'uloguser' with permissions to log in from any
        host ('%') and password 'ulogpass' and grant him privileges to
        do almost anything to this database''

''the following guidelines are given to hint you how to perform this
process in a linux based MySQL server, right from the mysql console
administration tool''

{{{ create database ulogdb; use ulogdb; source
/path/to/textfile/ulog-mysql.sql; grant
select,insert,update,drop,delete,create temporary tables, on ulogdb.\*
to 'uloguser'@'%' identified by 'ulogpass'; flush privileges; quit; }}}

== Configuring ULOGD ==

''go back to your openwrt shell window and edit /etc/ulogd.conf, find
the "output plugins" section, comment everything but the ulogd\_MYSQL
plugin, just like the example below''

{{{ \# output plugins. \#plugin="/usr/lib/ulogd/ulogd\_LOGEMU.so"
\#plugin="/usr/lib/ulogd/ulogd\_OPRINT.so"
plugin="/usr/lib/ulogd/ulogd\_MYSQL.so"
\#plugin="/usr/lib/ulogd/ulogd\_PGSQL.so"
\#plugin="/usr/lib/ulogd/ulogd\_SQLITE3.so"
\#plugin="/usr/lib/ulogd/ulogd\_PCAP.so" }}}

''while editing this file find the plugin configuration section,
identifiable by the plugin type enclosed in squared brackets, go to the
\[MYSQL\] section and change all parameters to fit your setup, paying
special attention to the host line where -of course- you will have to
specify your mysql server's network name or ip number and *it should be
reachable from your openwrt box*''

{{{ \[MYSQL\] table="ulog" pass="ulogpass" user="uloguser" db="ulogdb"
host="mysql server ip or name" }}}

== Configuring kernel module ==

''you must load and provide for the ipt\_ULOG kernel module to be
reloaded everytime your router is rebooted, in order to do that just
type the following commands in a shell window''

{{{ insmod ipt\_ULOG echo ipt\_ULOG &gt;
/etc/modules.d/60-iptableslogging }}}

<http://dev.mysql.com/doc/refman/5.0/en/old-client.html>
----CategoryHowTo
