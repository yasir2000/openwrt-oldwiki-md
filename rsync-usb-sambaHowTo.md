\~+\_\_Remote backups via rsync using a WRTSL54GS router\_\_+\~

''(Pieces heavily adopted from the Samba, USBStorage, &
DropbearPublicKeyAuthentication HowTo's)''

> 1.  Install the OpenWRT White Russian RC5 firmware on your router; DO
>     THIS FROM A WIRED CONNECTION, NOT WIRELESSLY:
>     <http://downloads.openwrt.org/whiterussian/rc5/bin/openwrt-wrtsl54gs-squashfs.bin>
> 2.  Release/renew your IP address; telnet to 192.168.0.1
> 3.  Change the password upon login; then ssh to 192.168.0.1 and login
>     as root with your new password
>
> 1\. For already existent files in /etc, you may need to remove them first
> before you can edit them; we need to add a line to the ipkg.conf file,
> so do the following: {{{

\# rm /etc/ipkg.conf \# cat&gt;/etc/ipkg.conf src whiterussian
<http://downloads.openwrt.org/whiterussian/packages> src non-free
<http://downloads.openwrt.org/whiterussian/packages/non-free> src
backports <http://downloads.openwrt.org/backports/rc5> dest root / dest
ram /tmp \^D }}} 1. Update the ipkg registry: {{{ \# ipkg update \# ipkg
install kmod-usb2 (USB 2.0 support) \# ipkg install kmod-usb-storage
(USB storage device support) \# rm /etc/modules.d/60-usb-storage \#
cat&gt;/etc/modules.d/60-usb-storage (adding the max\_scsi\_luns line)
scsi\_mod sd\_mod usb-storage max\_scsi\_luns=8 \^D }}} 1. Reboot to
activate the USB modules: {{{ \# reboot }}} 1. Install the file-system
support (you only need what is on your External USB drive) {{{ \# ipkg
install kmod-vfat (for legacy FAT partitions) \# ipkg install kmod-ext2
(Recommended: for ext2 filesystems) \# ipkg install kmod-ext3 (for ext3
filesystems) }}} I prefer using ext2 because it seems to be the most
portable.. ext2 lets you hook the external USB drive to a Windows box if
you need to, with the help of Ext2Fsd project
(<http://ext2fsd.sourceforge.net/>). Ext3 has read-support, but doesn't
have stable write-support. FAT has many limitations to it 1. Reboot
again for the file-system support to be enabled {{{ \# reboot }}} 1.
Install the fdisk package so you can see your USB device {{{ \# ipkg
install fdisk }}} 1. Create a mount point for the external disk {{{ \#
mkdir /mnt }}} 1. Look at what device paths your disks are on {{{ \#
fdisk -l <root@localhost>:\~\# fdisk -l Disk
/dev/scsi/host0/bus0/target0/lun0/disc: 250.0 GB, 250059350016 bytes 255
heads, 63 sectors/track, 30401 cylinders Units = cylinders of 16065 \*
512 = 8225280 bytes Device Boot Start End Blocks Id System
/dev/scsi/host0/bus0/target0/lun0/part1 1 15935 127997856 7 HPFS/NTFS
/dev/scsi/host0/bus0/target0/lun0/part2 15936 28989 104856255 83 Linux
}}} 1. Mount your filesystem {{{ \# mount
/dev/scsi/host0/bus0/target0/lun0/part2 /mnt}}} 1. Install the samba
package {{{ \# ipkg install samba }}} 1. Add an entry to your router's
name to /etc/hosts {{{ \# rm /etc/hosts \# cat&gt;/etc/hosts 127.0.0.1
localhost OpenWrt MyRouterName \^D }}} 1. Add entries as applicable to
the Samba configuration (TODO: Add better security): {{{ \# rm
/etc/samba/samba.conf \# cat&gt;/etc/samba/samba.conf \[global\] syslog
= 0 syslog only = yes workgroup = OpenWrt server string = OpenWrt Samba
Server security = share encrypt passwords = yes guest account = nobody
local master = yes name resolve order = lmhosts hosts bcast \[tmp\]
comment = /tmp path = /tmp browseable = yes public = yes writeable = no
\[All\_Partitions\] comment = /mnt path = /mnt browseable = yes public =
yes writeable = yes \^D }}} 1. Start up Samba; you should now be able to
access your shares on the network {{{ \# /etc/init.d/samba start }}} 1.
Next is creating your SSH keys for use with rsync: {{{ \# dropbearkey -t
rsa -f /etc/id\_rsa -s 2048 Will output 2048 bit rsa secret key to
'/etc/id\_rsa' Generating key, this may take a while... Public key
portion is: Fingerprint: md5
aa:fa:b7:5f:05:23:53:aa:4e:09:ad:db:10:0c:58:2d }}} 1. Connect to the
machine that will be storing the backups; copy the "ssh-rsa AAAA" line
(bolded above) to .ssh/authorized\_keys; '''make sure it is one line
long (it's ok for it to wrap around the screen).''' {{{ backuphost
\~/.ssh \$ cat&gt;&gt;authorized\_keys ssh-rsa AAAA.... \^D }}} 1. Make
sure the authorized\_keys and the .ssh directory have the proper
permissions: {{{ backuphost \~/.ssh \$ chmod 0600 authorized\_keys
backuphost \~/.ssh \$ chmod 0700 \~/.ssh }}} 1. Make a directory to hold
the backup {{{ \# mkdir \~/backup }}} 1. Back on the WRTSL54GS router,
install the rsync package: {{{ \# ipkg install rsync }}} 1. You can now
create an rsync script or crontab entry that will rsync your files to
the backup server! {{{ \# rsync -vv -u -a --rsh="ssh -i /etc/id\_rsa"
--stats --progress &lt;source&gt;
&lt;user&gt;@&lt;domain&gt;:&lt;destination&gt; }}}

'''Bonus: email yourself the results'''

> \* Install the mini-sendmail package {{{

\# ipkg install mini-sendmail

:   }}} \* Create a script /etc/rsync-scrypt {{{

\#!/bin/sh echo Subject: Backup Complete on date "+%m/%d/%y %l:%M %p"
&gt; /tmp/rsync.log rsync -v -u -a --rsh="ssh -i /etc/id\_rsa" --stats
&lt;from directory&gt; &lt;user&gt;@&lt;backup server&gt;:&lt;backup
location&gt; &gt;&gt; /tmp/rsync.log cat /tmp/rsync.log | sendmail
-f&lt;from email&gt; -s&lt;smtp server&gt; &lt;to email&gt; rm
/tmp/rsync.log }}}
