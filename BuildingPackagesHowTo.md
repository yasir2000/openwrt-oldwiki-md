'''!OpenWrt building packages howto'''

\[\[TableOfContents\]\]

= About the OpenWrt SDK = This howto is for people who would like to
port/package applications to OpenWrt with the !OpenWrt Software
Development Kit (SDK).

When using the SDK you don't require a full buildroot. The SDK is a
stripped down version of it, which includes the toolchain and all the
required library and header files to cross-compile applications for
!OpenWrt.

= Requirements =

:   -   a recent GNU/Linux distribution
    -   GNU Make (at least 3.80 with the Debian patch)
    -   wget
    -   patch

These packages suffice for the "Hello world" program described below

= Using the OpenWrt SDK = '''TIP:''' Before you begin porting your own
package to !OpenWrt, check if it has not been already done by someone
else. To check that \[<http://dev.openwrt.org/browser/trunk/package/>
browse the subversion repository\] of the development version in your
web browser and see if the package is already there. Don't do the work
twice.

Let's start with porting and packaging the well known "Hello world"
program as an example.

== Obtaining and installing the SDK == The SDK can be downloaded from
<http://downloads.openwrt.org/whiterussian/newest/>.

Download it into your home directory (don't use the root account) and
untar the tarball. After that change into the new directory.

{{{ cd \~ wget
<http://downloads.openwrt.org/whiterussian/newest/OpenWrt-SDK-Linux-i686-1.tar.bz2>
bzcat OpenWrt-SDK-Linux-i686-1.tar.bz2 | tar -xvf -cd
\~/OpenWrt-SDK-Linux-i686-1 }}} == Creating the directories == Create
the following directories:

{{{ cd \~/OpenWrt-SDK-Linux-i686-1 mkdir -p package/helloworld/ipkg
mkdir -p package/helloworld/patches }}} Directories and their contents:

{{{ \#!CSV Directory; Description ipkg; Control file which contains
information about your package patches; Patch files for example
100-foo.patch }}} '''TIP:''' To compile more than one package in a
special order do:

{{{ cd \~/OpenWrt-SDK-Linux-i686-1 mkdir -p package/100-packagename/ipkg
mkdir -p package/100-packagename/patches mkdir -p
package/200-packagename/ipkg mkdir -p package/200-packagename/patches
}}} == Creating the required files == '''TIP:''' If you intend to 'copy
& paste' the text from this Wiki to create files on your system, then
use the Unix command {{{unexpand}}} to translate the leading spaces into
tabs.

{{{ unexpand --first-only - &gt;package/helloworld/Config.in }}}

After pasting it into a text editor, press {{{ENTER}}} and then
{{{CTRL-D}}} keys to save the file.

You can also create your own files in the {{{package/helloworld}}}
directory (for example config files). That files you can access in your
{{{package/helloworld/Makefile}}} with {{{./filename}}} and copy it to
your {{{\$(PKG\_INSTALL\_DIR)}}} directory.

=== package/helloworld/Config.in === {{{ config BR2\_PACKAGE\_HELLO
prompt "hello............................. The classic greeting, and a
good example" tristate default m if CONFIG\_DEVEL help The GNU hello
program produces a familiar, friendly greeting. It allows
non-programmers to use a classic computer science tool which would
otherwise be unavailable to them. . Seriously, though: this is an
example of how to do a Debian package. It is the Debian version of the
GNU Project's \`hello world' program (which is itself an example for the
GNU Project). <http://www.wheretofindpackage.tld> }}}

You need to create a Config.in file. It identifies the package name. In
the above example BR2\_PACKAGE\_HELLO identifes that this is a 'BR2'(??)
formatted 'PACKAGE' whose name is 'HELLO'. The final executable is
'hello'.

=== package/helloworld/Makefile === The Makefile determines the way the
software package is shipped. Basically we can divide the software into 2
main categories :

> \* C (or ANSI-C) programs
>
> :   -   shipped with configure script
>     -   shipped with Makefile script (with references to gcc or \$(CC)
>         )
>     -   sources files only
>
> \* C++ programs
>
> :   -   potentially uClibc++ linkables
>     -   not uClibc++ linkables
>
'''TIP:''' Use the {{{md5sum}}} command to create the {{{PKG\_MD5SUM}}}
from the original tarball. Use {{{@SF/hello}}} (choose a and expanded
random !SourceForge mirror) for the {{{PKG\_SOURCE\_URL}}} when your
program has a download location on !SourceForge.

If PKG\_SOURCE\_URL and PKG\_SOURCE are correctly identified, then the
file will be downloaded into the \~/OpenWrt-SDK-Linux-i686-1/dl/
directory. It will be expanded into the
\~/OpenWrt-SDK-Linux-i686-1/build\_mipsel/ directory for compilation and
processing.

==== Sample Makefile for C/C++ programs shipped with configure script
==== {{{ include \$(TOPDIR)/rules.mk PKG\_NAME:=hello
PKG\_VERSION:=2.1.1 PKG\_RELEASE:=1
PKG\_MD5SUM:=70c9ccf9fac07f762c24f2df2290784d
PKG\_SOURCE\_URL:=ftp://ftp.cs.tu-berlin.de/pub/gnu/hello
 <http://mirrors.sunsite.dk/gnu/hello>
 <http://ftp.gnu.org/gnu/hello>
PKG\_SOURCE:=\$(PKG\_NAME)-\$(PKG\_VERSION).tar.gz PKG\_CAT:=zcat
PKG\_BUILD\_DIR:=\$(BUILD\_DIR)/\$(PKG\_NAME)-\$(PKG\_VERSION)
PKG\_INSTALL\_DIR:=\$(PKG\_BUILD\_DIR)/ipkg-install include
\$(TOPDIR)/package/rules.mk \$(eval \$(call
PKG\_template,HELLO,\$(PKG\_NAME),\$(PKG\_VERSION)-\$(PKG\_RELEASE),\$(ARCH)))
\$(PKG\_BUILD\_DIR)/.configured: \$(PKG\_BUILD\_DIR)/.prepared (cd
\$(PKG\_BUILD\_DIR);
 \$(TARGET\_CONFIGURE\_OPTS)
 CFLAGS="\$(TARGET\_CFLAGS)"
 CPPFLAGS="-I\$(STAGING\_DIR)/usr/include -I\$(STAGING\_DIR)/include"
 LDFLAGS="-L\$(STAGING\_DIR)/usr/lib -L\$(STAGING\_DIR)/lib"
 ./configure
 --target=\$(GNU\_TARGET\_NAME)
 --host=\$(GNU\_TARGET\_NAME)
 --build=\$(GNU\_HOST\_NAME)
 --prefix=/usr
 --without-libiconv-prefix
 --without-libintl-prefix
 --disable-nls
 ); \#\# Add software specific configurable options above \#\# See :
./configure --help touch \$@ \$(PKG\_BUILD\_DIR)/.built: rm -rf
\$(PKG\_INSTALL\_DIR) mkdir -p \$(PKG\_INSTALL\_DIR)/usr/bin \$(MAKE) -C
\$(PKG\_BUILD\_DIR)/src
 \$(TARGET\_CONFIGURE\_OPTS)
 prefix="\$(PKG\_INSTALL\_DIR)/usr" \$(CP) \$(PKG\_BUILD\_DIR)/src/hello
\$(PKG\_INSTALL\_DIR)/usr/bin touch \$@ \$(IPKG\_HELLO): install -d
-m0755 \$(IDIR\_HELLO)/usr/bin \$(CP)
\$(PKG\_INSTALL\_DIR)/usr/bin/hello \$(IDIR\_HELLO)/usr/bin \$(RSTRIP)
\$(IDIR\_HELLO) \$(IPKG\_BUILD) \$(IDIR\_HELLO) \$(PACKAGE\_DIR)
mostlyclean: make -C \$(PKG\_BUILD\_DIR) clean rm
\$(PKG\_BUILD\_DIR)/.built }}} ==== Sample Makefile for C/C++ software
shipped with a Makefile containing references to gcc or \$(CC) ==== If
you Makefile contains harcoded "gcc" commands, then you will have to
patch the makefile and replace gcc with \$(CC) in order to define at
"make time" the cross-compiler to use.

/!'''Note this Makefile is provided as an example only; it will not
compile'''

{{{ include \$(TOPDIR)/rules.mk PKG\_NAME:=hello PKG\_VERSION:=2.1.1
PKG\_RELEASE:=1 PKG\_MD5SUM:=70c9ccf9fac07f762c24f2df2290784d
PKG\_SOURCE\_URL:=ftp://ftp.cs.tu-berlin.de/pub/gnu/hello
 <http://mirrors.sunsite.dk/gnu/hello>
 <http://ftp.gnu.org/gnu/hello>
PKG\_SOURCE:=\$(PKG\_NAME)-\$(PKG\_VERSION).tar.gz PKG\_CAT:=zcat
PKG\_BUILD\_DIR:=\$(BUILD\_DIR)/\$(PKG\_NAME)-\$(PKG\_VERSION)
PKG\_INSTALL\_DIR:=\$(PKG\_BUILD\_DIR)/ipkg-install include
\$(TOPDIR)/package/rules.mk \$(eval \$(call
PKG\_template,HELLO,\$(PKG\_NAME),\$(PKG\_VERSION)-\$(PKG\_RELEASE),\$(ARCH)))
\$(PKG\_BUILD\_DIR)/.configured: \$(PKG\_BUILD\_DIR)/.prepared \#Since
there is no configure script, we can directly go to the building step
touch \$@ \$(PKG\_BUILD\_DIR)/.built: rm -rf \$(PKG\_INSTALL\_DIR) mkdir
-p \$(PKG\_INSTALL\_DIR)/usr/bin \#Note here that we pass cross-compiler
as default compiler to use \$(MAKE) -C \$(PKG\_BUILD\_DIR)/src
 CC=\$(TARGET\_CC)
 \$(TARGET\_CONFIGURE\_OPTS)
 prefix="\$(PKG\_INSTALL\_DIR)/usr" \$(CP) \$(PKG\_BUILD\_DIR)/src/hello
\$(PKG\_INSTALL\_DIR)/usr/bin touch \$@ \$(IPKG\_HELLO): install -d
-m0755 \$(IDIR\_HELLO)/usr/bin \$(CP)
\$(PKG\_INSTALL\_DIR)/usr/bin/hello \$(IDIR\_HELLO)/usr/bin \$(RSTRIP)
\$(IDIR\_HELLO) \$(IPKG\_BUILD) \$(IDIR\_HELLO) \$(PACKAGE\_DIR)
mostlyclean: make -C \$(PKG\_BUILD\_DIR) clean rm
\$(PKG\_BUILD\_DIR)/.built }}} ==== Sample Makefile for C/C++ programs
without makefiles (usually one or two source files) ==== /!'''Note this
Makefile is provided as an example only; it will not compile'''

{{{ include \$(TOPDIR)/rules.mk PKG\_NAME:=hello PKG\_VERSION:=2.1.1
PKG\_RELEASE:=1 PKG\_MD5SUM:=70c9ccf9fac07f762c24f2df2290784d
PKG\_SOURCE\_URL:=ftp://ftp.cs.tu-berlin.de/pub/gnu/hello
 <http://mirrors.sunsite.dk/gnu/hello>
 <http://ftp.gnu.org/gnu/hello>
PKG\_SOURCE:=\$(PKG\_NAME)-\$(PKG\_VERSION).tar.gz PKG\_CAT:=zcat
PKG\_BUILD\_DIR:=\$(BUILD\_DIR)/\$(PKG\_NAME)-\$(PKG\_VERSION)
PKG\_INSTALL\_DIR:=\$(PKG\_BUILD\_DIR)/ipkg-install include
\$(TOPDIR)/package/rules.mk \$(eval \$(call
PKG\_template,HELLO,\$(PKG\_NAME),\$(PKG\_VERSION)-\$(PKG\_RELEASE),\$(ARCH)))
\$(PKG\_BUILD\_DIR)/.configured: \$(PKG\_BUILD\_DIR)/.prepared \#Since
there is no configure script, we can directly go to the building step
touch \$@ \$(PKG\_BUILD\_DIR)/.built: rm -rf \$(PKG\_INSTALL\_DIR) mkdir
-p \$(PKG\_INSTALL\_DIR)/usr/bin \$(TARGET\_CC)
\$(PKG\_BUILD\_DIR)/src/\$(PKG\_NAME).c -o
\$(PKG\_BUILD\_DIR)/\$(PKG\_NAME) \#\# -lyourlib \#Note we directly call
the cross-compiler and define its output \$(CP)
\$(PKG\_BUILD\_DIR)/src/hello \$(PKG\_INSTALL\_DIR)/usr/bin touch \$@
\$(IPKG\_HELLO): install -d -m0755 \$(IDIR\_HELLO)/usr/bin \$(CP)
\$(PKG\_INSTALL\_DIR)/usr/bin/hello \$(IDIR\_HELLO)/usr/bin \$(RSTRIP)
\$(IDIR\_HELLO) \$(IPKG\_BUILD) \$(IDIR\_HELLO) \$(PACKAGE\_DIR)
mostlyclean: make -C \$(PKG\_BUILD\_DIR) clean rm
\$(PKG\_BUILD\_DIR)/.built }}} ==== Sample Makefile for C++ shipped with
configure script, and uClibc++ linkables ==== /!'''Note this Makefile is
provided as an example only; it will not compile'''

{{{ include \$(TOPDIR)/rules.mk PKG\_NAME:=hello PKG\_VERSION:=2.1.1
PKG\_RELEASE:=1 PKG\_MD5SUM:=70c9ccf9fac07f762c24f2df2290784d
PKG\_SOURCE\_URL:=ftp://ftp.cs.tu-berlin.de/pub/gnu/hello
 <http://mirrors.sunsite.dk/gnu/hello>
 <http://ftp.gnu.org/gnu/hello>
PKG\_SOURCE:=\$(PKG\_NAME)-\$(PKG\_VERSION).tar.gz PKG\_CAT:=zcat
PKG\_BUILD\_DIR:=\$(BUILD\_DIR)/\$(PKG\_NAME)-\$(PKG\_VERSION)
PKG\_INSTALL\_DIR:=\$(PKG\_BUILD\_DIR)/ipkg-install include
\$(TOPDIR)/package/rules.mk \$(eval \$(call
PKG\_template,HELLO,\$(PKG\_NAME),\$(PKG\_VERSION)-\$(PKG\_RELEASE),\$(ARCH)))
\$(PKG\_BUILD\_DIR)/.configured: \$(PKG\_BUILD\_DIR)/.prepared (cd
\$(PKG\_BUILD\_DIR);
 \$(TARGET\_CONFIGURE\_OPTS)
 CFLAGS="\$(TARGET\_CFLAGS)"
 CPPFLAGS="-I\$(STAGING\_DIR)/usr/include -I\$(STAGING\_DIR)/include"
 LDFLAGS="-L\$(STAGING\_DIR)/usr/lib -L\$(STAGING\_DIR)/lib"
 ./configure
 CXXFLAGS="\$(TARGET\_CFLAGS) -fno-builtin -fno-rtti -nostdinc++"
 CPPFLAGS="-I\$(STAGING\_DIR)/usr/include -I\$(STAGING\_DIR)/include"
 LDFLAGS="-nodefaultlibs -L\$(STAGING\_DIR)/usr/lib
-L\$(STAGING\_DIR)/lib" \#do not use default libraries since we want
uClibc++ linking LIBS="-luClibc++ -lc -lm -lgcc" \# You may need to add
other libraries : lpcap, lssl ... \# --target=\$(GNU\_TARGET\_NAME)
 --host=\$(GNU\_TARGET\_NAME)
 --build=\$(GNU\_HOST\_NAME)
 --prefix=/usr
 --without-libiconv-prefix
 --without-libintl-prefix
 --disable-nls
 ); \#\# Add software specific configurable options above \#\# See :
./configure --help touch \$@ \$(PKG\_BUILD\_DIR)/.built: rm -rf
\$(PKG\_INSTALL\_DIR) mkdir -p \$(PKG\_INSTALL\_DIR)/usr/bin \$(MAKE) -C
\$(PKG\_BUILD\_DIR)/src
 \$(TARGET\_CONFIGURE\_OPTS)
 prefix="\$(PKG\_INSTALL\_DIR)/usr" \$(CC) \$(PKG\_BUILD\_DIR)/src/hello
\$(PKG\_INSTALL\_DIR)/usr/bin touch \$@ \$(IPKG\_HELLO): install -d
-m0755 \$(IDIR\_HELLO)/usr/bin \$(CP)
\$(PKG\_INSTALL\_DIR)/usr/bin/hello \$(IDIR\_HELLO)/usr/bin \$(RSTRIP)
\$(IDIR\_HELLO) \$(IPKG\_BUILD) \$(IDIR\_HELLO) \$(PACKAGE\_DIR)
mostlyclean: make -C \$(PKG\_BUILD\_DIR) clean rm
\$(PKG\_BUILD\_DIR)/.built }}} === package/helloworld/ipkg/hello.control
=== The control file, as you might have guessed, controls the package
information reported by ipkg.

Anyone familiar with Debian packaging will be aware of the format - a
deeper description than provided here is available in the
\[<http://handhelds.org/moin/moin.cgi/BuildingIpkgs> ipkg
documentation\].

{{{ Package: hello Priority: optional Section: misc Description: The GNU
hello world program }}} The following fields are available:

> . '''Package''' - should be the package name, as in the Makefile.
> '''Priority''' - should be set to ''optional'' for almost all
> packages. '''Section''' - indicates the type of package - useful
> sections include ''comm'', ''editors'', ''graphics'', ''libs'',
> ''net'', ''text'', ''web'', or if you can't decide, ''misc''.
> '''Description''' - a short description of the package. (You can
> include a longer description here in a similar manner to the help text
> in Config.in. Start a new line after the short description, and use a
> line containing a single full stop ('.') as a replacement for blank
> lines. '''Depends''' (not in the example above) - a list of package
> names that this package ''requires'' to operate. Use package names
> without versions here where possible (e.g. ''openssh-client'').

Note: had to modify package/rules.mk changing ./ipkg/\$(2) to the real
directory ./ did not work for me ===
package/helloworld/patches/100-hello.patch === This example applies a
Debian patch, which isn't essential for (so you can skip this point).

Other Linux and free UNIX distributions are often an excellent source of
patches for non-portable programs. You might like to try searching for
packages from \[<http://packages.ubuntu.com/dapper/source/> Ubuntu\],
\[<http://sources.gentoo.org/> Gentoo\], or
\[<http://www.freshports.org/> FreeBSD's Ports\].

{{{ cd package/helloworld/patches wget
<http://ftp.debian.org/debian/pool/main/h/hello/hello_2.1.1-4.diff.gz>
gunzip hello\_2.1.1-4.diff.gz mv hello\_2.1.1-4.diff 100-hello.patch }}}
'''TIP:''' You can apply as many patches as you like. To apply them in a
special order name them like:

{{{ 100-xxx.patch 200-xxx.patch }}} == Compile the package == The
{{{make}}} command below compiles every package that you have created in
the {{{package}}} directory.

{{{ cd \~/OpenWrt-SDK-Linux-i686-1 make clean && make world }}}

Note that there is a fault in the default package/rules.mk file. There
is ".." following the "\$(PKG\_BUILD\_DIR)/" which causes the files to
be extracted to the wrong directory. Here is the corrected version: {{{
ifneq (\$(strip \$(PKG\_CAT)),) \$(PKG\_BUILD\_DIR)/.prepared:
\$(DL\_DIR)/\$(PKG\_SOURCE) rm -rf \$(PKG\_BUILD\_DIR) mkdir -p
\$(PKG\_BUILD\_DIR) \$(PKG\_CAT) \$(DL\_DIR)/\$(PKG\_SOURCE) | tar -C
\$(PKG\_BUILD\_DIR)/ \$(TAR\_OPTIONS) if \[ -d ./patches \]; then
 \$(PATCH) \$(PKG\_BUILD\_DIR) ./patches ;
 fi touch \$(PKG\_BUILD\_DIR)/.prepared endif }}} '''NOTE:''' If you are
using GNU make 3.80 (current "latest") and get a "virtual memory
exhausted" message while making, see \[<http://gamecontractor.org/Make>
this page\].

For Slackware users there is a fixed make package
\[<http://internetghetto.org/files/index.php?download>=./make-fix/make-fixed-3.80-i386-1.tgz
here\] and sources + patch are
\[<http://internetghetto.org/files/index.php?dir>=./make-fix/orig/
here\].

When the compiling is finished you have a ready to use ipkg package for
!OpenWrt in the {{{\~/OpenWrt-SDK-Linux-i686-1/bin/packages}}}
directory.

{{{ cd bin/packages; ls -al hello\_2.1.1-1\_mipsel.ipk -rw-r--r-- 1
openwrt-dev openwrt-dev 3976 Sep 14 13:03 hello\_2.1.1-1\_mipsel.ipk }}}
= Contribute your new ported program = When you like you can contribute
your program/package to the !OpenWrt community. It may be included in
further versions of !OpenWrt.

To do this create a patch from your {{{package/&lt;PKG\_NAME&gt;}}}
directory with:

{{{ cd \~/OpenWrt-SDK-Linux-i686-1 diff -ruN
package/&lt;PKG\_NAME&gt;.orig package/&lt;PKG\_NAME&gt; &gt;
&lt;PKG\_NAME&gt;-&lt;PKG\_VERSION&gt;.patch }}} Once you have created a
patch \[<https://dev.openwrt.org/newticket> open a ticket\] and submit
your new package (the patch).

= Native Development = You need 150Mb storage unit (USB or SD Card)

-   Download the file
    \[<http://www.uclibc.org/downloads/root_fs_mipsel.ext2.bz2> Native
    Mipsel Toolchain\] (24Mb)
-   Bunzip2 (120mb) it to the storage unit in a ext2 partition.
-   unmount partition
-   Execute this script, I have it at /sbin/devel.sh

{{{ \#!/bin/sh \# Kill unusefull tasks (uncoment it) we need memory
\#killall logger \#killall syslogd \#killall telnetd \#killall crond
\#killall klogd \#killall udhcpc \#killall httpd \#rmmod ext3 \#rmmod
jbd \#My SD card have 2 partitions 1.ext2 2.swap \#\#\#\#\#\#\#\#\#\#\#
Change this line to your system mount /dev/mmc/disc0/part1 /mnt -o
noatime async \# Swap for large sources. I have 30Mb \#swapoff -a
\#mkswap /dev/mmc/disc0/part2 \#swapon /dev/mmc/disc0/part2 mount -o
move /tmp /mnt/tmp echo " **\* exit**\* to back - Para volver al
sistema" chroot /mnt/ /bin/ash -echo " **\* Me are here again - De
vuelta al sistema original**\*" mount -o move /mnt/tmp/ /tmp/ umount
/mnt }}} - Go /home

-   download the source. Example:
    \[<http://www.didiwiki.org/sources/didiwiki-0.5.tar.gz>
    Didiwiki-0.5.tar.gz\] from <http://www.didiwiki.org>
-   tar -xvzf didiwiki-0.5.tar.gz
-   cd didiwiki-0.5
-   configure (1 minute)
-   make (1 minute)
-   You have your new Binary in the SRC directory (didiwiki)
-   copy it to the /tmp directori
-   type exit

You have the binary in /tmp directory. copy it to /usr/bin

The result \[<http://gepage.googlepages.com/didiwiki.mipsel.binary.gz>
didiwiki.mipsel.binary.gz\] a small wiki for our router at 8000 port. If
you don't use storage unit, you must create /home to store new pages.
/home/.didiwiki/\*

= Links = You can find an useful reference for the packaging process in
nbd's paper to the '!OpenWrt Hacking' talk on the 22C3: \[\[BR\]\]-
<http://events.ccc.de/congress/2005/fahrplan/attachments/567-Paper_HackingOpenWRT.pdf>

Full buildroot documentation (for compiling kernel modules and such
things, for the rest the SDK should be used) \[\[BR\]\]-
<http://downloads.openwrt.org/docs/buildroot-documentation.html>
