{{{Quick and dirty on how to get iptables to work with ULOG, and ULOG to
work with postgresql.

assuming <openwrt@192.168.1.1> assuming <pgsql@192.168.1.2> on Debian or
similar assuming database name ulogd, user ulogd, password ulogd, table
ulog (below)

Using postgres 7.4 on Debian:

/etc/postgresql/pg\_hba.conf:
(/etc/postgresql/&lt;version&gt;/main/pg\_hba.conf on Ubuntu) (Allows
md5 password auth for login from 192.168.1.\*)

\# IPv4-style local connections: host all all 127.0.0.1 255.255.255.255
md5 host all all 192.168.1.1 255.255.255.0 md5

/etc/postgresql/postgres.conf: (tcpip\_socket disabled by default in
Ubuntu; skip this step in Debian) \# - Connection Settings
-tcpip\_socket = true

Set up user/database: \# su postgres \$ psql template1 Welcome to
postgres, yadda yadda &gt; create user ulogd with encrypted password
'ulogd' createdb; &gt; q \$ psql -U ulogd template1 (If this doesn't
work, fiddle with pg\_hba.conf above and restart the db:
/etc/init.d/postgresql restart) &gt; create database ulogd; &gt; q \$
psql -U postgres ulogd (Create pgsql language and necessary casts as
master user) create or replace function plpgsql\_call\_handler() RETURNS
language\_handler AS '\$libdir/plpgsql', 'plpgsql\_call\_handler'
LANGUAGE c; create trusted procedural language plpgsql handler
plpgsql\_call\_handler;

CREATE FUNCTION integerip\_to\_str(integer) RETURNS inet AS ' DECLARE t
inet; BEGIN t = ((\$1&gt;&gt;24) & 255::int8) | ((\$1&gt;&gt;16) &
255::int8) | ((\$1&gt;&gt;8) & 255::int8) | (\$1 & 255::int8); RETURN t;
END; ' LANGUAGE 'plpgsql';

CREATE CAST (integer AS inet)

:   WITH FUNCTION integerip\_to\_str(integer) AS IMPLICIT;

CREATE OR REPLACE FUNCTION ulogtimecast(integer)

:   RETURNS timestamp AS

'select "timestamp"(\$1::abstime);'

:   LANGUAGE 'sql' VOLATILE;

CREATE CAST (integer AS timestamp)

:   WITH FUNCTION ulogtimecast(integer) AS IMPLICIT;

\$ psql -U ulogd ulogd create sequence 'seq\_ulog'; CREATE TABLE ulog (
id int4 NOT NULL DEFAULT nextval('seq\_ulog'), oob\_prefix varchar(32),
oob\_time\_sec timestamp, oob\_time\_usec int4, oob\_mark int8, oob\_in
varchar(32), oob\_out varchar(32), raw\_mac varchar(80), raw\_pktlen
int8, ip\_ihl int2, ip\_tos int2, ip\_totlen int4, ip\_id int4,
ip\_fragoff int4, ip\_ttl int2, ip\_protocol int2, ip\_csum int4,
ip\_saddr inet, ip\_daddr inet, tcp\_sport int4, tcp\_dport int4,
tcp\_seq int8, tcp\_ackseq int8, tcp\_urg bool, tcp\_ack bool, tcp\_psh
bool, tcp\_rst bool, tcp\_syn bool, tcp\_fin bool, tcp\_window int4,
tcp\_urgp int4, udp\_sport int4, udp\_dport int4, udp\_len int4,
icmp\_type int2, icmp\_code int2, icmp\_echoid int4, icmp\_echoseq int4,
icmp\_gateway int8, icmp\_fragmtu int4, pwsniff\_user varchar(30),
pwsniff\_pass varchar(30), ahesp\_spi int2, local\_time int8,
local\_hostname varchar ) WITH OIDS;

Using rc4:

<root@OpenWrt>:\~\# ipkg list | grep ulog iptables-mod-ulog - 1.3.3-1 -
Iptables (IPv4) extension for user-space packet logging kmod-ipt-ulog -
2.4.30-brcm-2 - Netfilter (IPv4) kernel module for user-space packet
logging ulogd - 1.23-2 - Netfilter userspace logging daemon
ulogd-mod-pgsql - 1.23-2 - Netfilter userspace logging daemon
(PostgreSQL plugin)

Make sure you have those installed: ipkg install iptables-mod-ulog
kmod-ipt-ulog ulogd ulogd-mod-pgsql

/etc/ulogd.conf : (Loads PGSQL plugin, sets PGSQL db settings)

\# output plugins. plugin="/usr/lib/ulogd/ulogd\_LOGEMU.so"
\#plugin="/usr/lib/ulogd/ulogd\_OPRINT.so"
\#plugin="/usr/lib/ulogd/ulogd\_MYSQL.so"
plugin="/usr/lib/ulogd/ulogd\_PGSQL.so"
\#plugin="/usr/lib/ulogd/ulogd\_SQLITE3.so"
\#plugin="/usr/lib/ulogd/ulogd\_PCAP.so"

\[PGSQL\] table="ulog" schema="public" user="ulogd" pass="ulogd"
host="192.168.1.2" db="ulogd"

/etc/modules : ipt\_ULOG

next, reboot or modprobe ipt\_ULOG

try a rule to be sure it all works. Maybe something like: iptables -A
forwarding\_rule ULOG --ulog-nlgroup 1 to log all forwarded packets.

Much of the above cobbled from following URLS:
<http://www.snort.org/docs/snortdb/snortdb_faq.html#faq_b4>
<http://lists.gnumonks.org/pipermail/ulogd/2004-January/000494.html>

TODO: mac addr in table schema should be pgsql's native mac addr type,
but ULOGD is sending it as "XX:XX:XX:XX:XX:XX:XX:XX " with a space at
the end which throws everything off TODO: clean this page up! SERIOUSLY!
}}} ----CategoryHowTo
