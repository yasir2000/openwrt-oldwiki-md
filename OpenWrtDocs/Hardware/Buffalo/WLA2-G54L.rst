'''Buffalo WLA2-G54L'''

The WLA2-G54L is based on the Broadcom 4712 board. It has a 200MHz CPU, 4Mb flash and 16Mb RAM.
The wireless NIC is integrated to the board. boot_wait is on by default.

The next experimental snapshot will support these units.
Until that, You can use a hacked version of the .trx file from http://wifi.rulez.org/buffalo.trx

Please keep in mind that for now we can't control the leds.

/!\ '''This router sits on 192.168.11.100 as default, but flashable always on the last IP You have set up, so You have to TFTP to that address.'''
