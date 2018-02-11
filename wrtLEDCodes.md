\#\#language:en == WRT LED Codes Analysis == Some LED sequences
explained. LED goes on and off at intervals of 1 second or less | LED
stays on |

=== POWER flashes === flashes. || '''''Reason''''':\[\[BR\]\] The power
led can flash for a number of reasons; a flashing power led by itself is
rarely cause for concern. The power led will flash on bootup until the
firmware stops the flashing, but it may start flashing again with
certain wireless settings (eg: configured as wifi client, not connected)

=== DMZ lights === on || '''''Reason''''':\[\[BR\]\] !OpenWrt uses the
DMZ led to signal bootup. Once the kernel has booted !OpenWrt will turn
on the DMZ led; it won't turn off again until after !OpenWrt has
processed all the startup scripts in /etc/init.d. If it doesn't turn off
then there is a problem with one of the startup scripts.

=== POWER flashes, DMZ alternates === flashes. |
'''''Reason''':\[\[BR\]\] You flashed a '''corrupt''' image.\[\[BR\]\]
This might be because you did not set the binary flag on your tftp
program, or have a bad image, or your upload was interrupted. ''

'''''Solution''''':\[\[BR\]\] Just reflash, and make sure your tftp
program is configured right. Check your image.

== Controlling the LEDs in RC5 == Some of the front panel LEDs can be
controlled by writing to /proc/sys/diag. A single hex value written here
will control the state of the LEDs, e.g.

> . echo "0x01" &gt; /proc/sys/diag

The values are bitfields, so add up the ones you want in the table below
to control the desired LEDs. Action Defaults DMZ LED on Power LED
flashing

For example, if you want the power LED to flash (0x04) and the DMZ LED
to light up (0x01) you'd echo "0x05" &gt; /proc/sys/diag

On RC5, the CISCO LED found on some linksys routers is now supported.
Here's a table of bitfields that work for me Action Defaults DMZ LED on
Power LED flashing White Cisco LED ON Orange Cisco LED ON

Feel free to edit this out of here, but I put together this chart which
helps me. I didn't list the values in hexadecimal as my scripts use
''cat /proc/sys/diag'' and it doesn't return a hex value. Also I see no
need for the hex value as ''echo "8" &gt; /proc/sys/diag'', etc. works
just as well without me doing hex math. Also I cannot confirm the
versions these codes work for, but I'm running a v1.0 & v3.0 wrt54g.
'''Cisco''' '''DMZ''' | Off On | Off On | White On | White On | Orange
On | Orange On | White & Orange On | White & Orange On ||

I also noticed this thread:
<http://forum.openwrt.org/viewtopic.php?id=4796> which has a simple
script from vincentfox to check diag bits.

== Controlling the LEDs in RC6 ==

In RC6, there are separate files in /proc/diag/led/ for each of the
separately controllable LEDs; you can echo 0
&gt;/proc/diag/led/ses\_orange to turn the SES orange LED off, echo 1 to
turn it on, or echo f to start it flashing. The same codes work for all
five LEDs.
