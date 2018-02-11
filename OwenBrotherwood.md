'''Owen Brotherwood (oxo), Denmark'''

:   Hello world!

    -   \[<http://www.sitecenter.dk/o-o/sendemail/> Send email\]
    -   \[<http://www.sitecenter.dk/o-o> Home Page\]

  -------------------------------------------------------------------------------------------------------------------------------------
  \[\[TableOfContents(3)\]\]
  = shellwrt =
  Well \[<http://www.bitsum.com/smf/index.php?topic=332.msg1589#msg1589> multi-web\] needs some research ...
  == /usr/bin/shellwrt ==
  shellwrt should:
  \* be an ipkg package and run on
  \* OpenWrt box
  \* Local Pc Shell (CPU friendly for OpenWrt box ...)
  \* SSH keys for no password
  \* Find out which client: OpenSSH or dropbear
  \* Send public keys to all boxes in WDS
  \* Configuration
  \* Network
  \* do some things that are special per box
  \* do some things that are global for a WDS
  \* Hardware box
  \* enable special things: eg USB/Printer on Asus Premium
  \* be "Developer friendly"
  {{{
  \$shellwrt <root@192.168.1.1>
  }}}
  === Detect ssh client ===
  We "drop"bear for the time being as I cannot get Whiterussian-&gt;Whiterussian with dropbear to work, only OpenSSh works so far ...
  {{{
  SSHCLIENTOPENSSH='openssh'
  OPENSSHDIR="\$HOME/.ssh"
  OPENSSHFILE='id\_dsa.pub'
  OPENSSHPUB="\${OPENSSHDIR}/\${OPENSSHFILE}"
  setuppubdropbear(){
  return 1
  }
  setuppubopenssh(){
  return 1
  }
  {
  \[ -d \${OPENSSHDIR} \] && my\_sshclient='openssh'
  \[ -d \${DROPBEARDIR} \] && my\_sshclient='dropbear'
  case \$my\_sshclient in
  \${SSHCLIENTDROPBEAR}) \#
  \[ -f \${DROPBEARPUB} \] | return 1
  cat \${DROPBEARPUB}
  ;;
  \${SSHCLIENTOPENSSH}) \#
  \[ -f \${OPENSSHPUB} \] | return 1
  cat \${OPENSSHPUB}
  ;;
  \*) \#
  echo &gt;&2 SSHCLIENTUNKNOWN || return 1
  ;;
  esac
  return 0
  }
  }}}
  {{{
  \#
  \# shellwrt
  \#
  \# v000: just do keys ..
  MY\_ROOT="\$HOME/src/shellwrt/"
  MY\_UNAME=\$(uname -a)
  MY\_SHELL=\$SHELL
  MY\_SHELLWRTLIB="\${MY\_ROOT}usr/lib/shellwrt"
  MY\_LIBS='shellwrt.lib'
  {
  for lib in \${MY\_LIBS};do
  . \${MY\_SHELLWRTLIB}/\${lib}
  done
  for device in \$\*;do
  setupssh \${device} \${MY\_PUBLICDIR} \${MY\_PUBLICFILE} \${MY\_DEVAUTDIR} \${MY\_DEVAUTFILE}
  done
  }
  }}}
  == /usr/lib/shellwrt/shellwrt.lib ==
  {{{
  \#
  \#doc\#shellwrt.lib
  \#doc\#source this file
  \#todo\#allow ssh from Openwrt box to Openwrt box
  MY\_PUBLICDIR="\$HOME/.ssh"
  MY\_PUBLICFILE='id\_dsa.pub'
  MY\_DEVAUTDIR='/etc/dropbear'
  MY\_DEVAUTFILE='authorized\_keys'
  debugmy(){ \#doc\#todo just shoe [MY]()'s
  for my in \${!MY\_\*};do
  eval echo \${my}
  done
  return 0 \#doc\#
  }
  setupssh(){ \#doc\#scp public key to device
  MY\_DEVICE=\$1
  MY\_PUBLICDIR=\$2
  MY\_PUBLICFILE=\$3
  MY\_DEVAUTDIR=\$4
  MY\_DEVAUTFILE=\$5
  MY\_PUBLIC="\$2/\$3"
  MY\_DEVAUT="\$4/\$5"
  MY\_TEMP='/tmp'
  MY\_SSHCMDS="
  cp \${MY\_DEVAUT} \${MY\_TEMP};
  cat \${MY\_TEMP}/\$3 &gt;&gt;\${MY\_TEMP}/\$5;
  sort -u \${MY\_TEMP}/\$5 &gt;\$4/\$5;
  chmod 0600 \$4/\$5;
  "
  scp \${MY\_PUBLIC} \${MY\_DEVICE}:\${MY\_TEMP}
  ssh \${MY\_DEVICE} \${MY\_SSHCMDS}
  return \$? \#doc\#return \$? from ssh cmd
  }
  {
  return 0
  }
  }}}
  = ssh =
  ssh is very important in the solution: to be able to ssh with keys so no password.
  It is easy to ssh from a ssh box to an OpenWrt box. However OpenWrt box to another OpenWrt box with a key is also needed.
  dropbear doesn't work openssh does.
  == OpenSSH ==
  Works.
  === /etc/init.d/S50sshd ===
  {{{\#!/bin/sh
  for type in rsa dsa; do {
  \# check for keys
  key=/etc/ssh/[ssh\_host]()\${type}\_key
  \[ ! -f \$key \] && {
  \# generate missing keys
  \[ -x /usr/bin/ssh-keygen \] && {
  /usr/bin/ssh-keygen -N '' -t \$type -f \$key 2&gt;&- &gt;&- && exec \$0 \$\*
  } &
  exit 0
  }
  }; done
  mkdir -p /var/empty
  /usr/sbin/sshd
  }}}
  == dropbear ==
  Doesn't work.
  Maybe it is because the dropbear key generation with -y does not place a <someone@somwhere> at the end of the public key ...
  === dropbear README ===
  {{{
  In the absence of detailed documentation, some notes follow:
  -------------------------------------------------------------------------------------------------------------------------------------

Server public key auth:

You can use \~/.ssh/authorized\_keys in the same way as with OpenSSH,
just put the key entries in that file. They should be of the form:

ssh-rsa
AAAAB3NzaC1yc2EAAAABIwAAAIEAwVa6M6cGVmUcLl2cFzkxEoJd06Ub4bVDsYrWvXhvUV+ZAM9uGuewZBDoAqNKJxoIn0Hyd0Nk/yU99UVv6NWV/5YSHtnf35LKds56j7cuzoQpFIdjNwdxAN0PCET/MG8qyskG/2IE2DPNIaJ3Wy+Ws4IZEgdJgPlTYUBWWtCWOGc=
<someone@hostname>

You must make sure that \~/.ssh, and the key file, are only writable by
the user. Beware of editors that split the key into multiple lines.

NOTE: Dropbear ignores authorized\_keys options such as those described
in the OpenSSH sshd manpage, and will not allow a login for these keys.

------------------------------------------------------------------------

Client public key auth:

Dropbear can do public key auth as a client, but you will have to
convert OpenSSH style keys to Dropbear format, or use dropbearkey to
create them.

If you have an OpenSSH-style private key \~/.ssh/id\_rsa, you need to
do:

dropbearconvert openssh dropbear \~/.ssh/id\_rsa \~/.ssh/id\_rsa.db
dbclient -i \~/.ssh/id\_rsa.db &lt;hostname&gt;

Currently encrypted keys aren't supported, neither is agent forwarding.
At some stage both hopefully will be.

------------------------------------------------------------------------

If you want to get the public-key portion of a Dropbear private key,
look at dropbearkey's '-y' option.

------------------------------------------------------------------------

To run the server, you need to generate server keys, this is one-off:
./dropbearkey -t rsa -f dropbear\_rsa\_host\_key ./dropbearkey -t dss -f
dropbear\_dss\_host\_key

or alternatively convert OpenSSH keys to Dropbear: ./dropbearconvert
openssh dropbear /etc/ssh/ssh\_host\_dsa\_key dropbear\_dss\_host\_key

============================================================================
}}} === /etc/init.d/S50dropbear === {{{ \#!/bin/sh

\[ ! -f /etc/dropbear/dropbear\_rsa\_host\_key \] && {

:   

    for type in rsa dss; do {

    :   \# check for keys
        key=/tmp/dropbear/[dropbear]()\${type}\_host\_key \[ ! -f \$key
        \] && { \# generate missing keys mkdir -p /tmp/dropbear \[ -x
        /usr/bin/dropbearkey \] && { /usr/bin/dropbearkey -t \$type -f
        \$key 2&gt;&- &gt;&- && exec \$0 \$\* } & exit 0 }

    }; done lock -w /tmp/.switch2jffs mkdir -p /etc/dropbear mv
    /tmp/dropbear/[dropbear]()\* /etc/dropbear/

}
=

== o-o ==

:   \[<http://wiki.openwrt.org/DropbearPublicKeyAuthenticationHowto>
    authorized\_hosts\]


