= Hardware performance of devices running under OpenWRT = Although some
people believe that these devices are not dependent on their CPU speed
due to the commonly low load averages, this is not necessarily true. A
processor at any moment is either executing applicable code or not,
essentially ON or OFF. Load averages are averages over time. Therefore,
increased processor (and memory) clocks can decrease latency, speed
execution of scripts and programs, and increase I/O.

Additionally, many processors can be overclocked (though this operation
has some risk and should not be attempted lightly). See
\["OpenWrtDocs/Customizing/Hardware/Overclocking"\] for overclocking
information. It includes arguments for and against overclocking. Users
on either side of the fence should refrain from forcing their opinions
on the majority.

\[See further discussion, which has been moved to end of this page...\]

== Devices performance table == '''Performance of CPU / Memory'''
'''Tester''' '''Time for pi''' '''Time for float''' '''OS''' '''CPU'''
'''Link to HW page''' | cabo 13.2s 10.2s (unknown) BCM3302 V0.7
\["OpenWrtDocs/Hardware/Linksys/WRT54GS"\] | arteqw 14.5s 11.3s OpenWRT
RC5 BCM3302 V0.7 \["OpenWrtDocs/Hardware/Motorola/WR850G"\] | mauritzius
25.3s 15.1s OpenWRT pre-RC5 BCM4710 V0.0
\["OpenWrtDocs/Hardware/Asus/WL500G"\] | pkirchhofer 61.4s 8.0s
Debonaras (gcc-3.4) XScale-IXP420
\[<http://www.nslu2-linux.org/wiki/Info/HomePage> NSLU2-LINUX\] | Ultimo
14.2s 9.1s OpenWRT RC5 BCM3302 V0.7
\["OpenWrtDocs/Hardware/Linksys/WRT54GL"\] | ''MickeyMouse'' 14.5s 11.4s
OpenWRT RC5 BCM3302 V0.7 \["OpenWrtDocs/Hardware/Linksys/WRT54G"\] |
LinuxInside 12.6s 7.6s (unknown) BCM3302 V0.7
\["OpenWrtDocs/Hardware/Linksys/WAG54GX2"\] | evildevil 29.8s 4.0s
!DebianSlug Sid XScale-IXP420
\[<http://www.nslu2-linux.org/wiki/Info/HomePage> NSLU2-LINUX\] |
CharlesAmos 11.0s 6.8s OpenWRT pre-RC5 BCM4704
\["OpenWrtDocs/Hardware/Linksys/WRTSL54GS"\] | jcollake 12.4s 7.7s
OpenWrt RC5 BCM3302 v0.8 \["OpenWrtDocs/Hardware/Linksys/WRT54G"\] |
achim71000 10.6s 6.7s OpenWRT RC5 BCM4704
\["OpenWrtDocs/Hardware/Asus/WL500GP"\] | RafalRzeczkowski 10.5s 6.5s
OpenWRT RC5 BCM4704 rev 8 \["OpenWrtDocs/Hardware/Buffalo/WZR-RS-G54"\]
| FrederikKlama 10.5s 6.6s OpenWRT RC5 BCM4704
\["OpenWrtDocs/Hardware/Asus/WL500GP"\] | jp 13.2s 8.5s OpenWRT RC5
BCM3302 V0.8 \["OpenWrtDocs/Hardware/Linksys/WRT54GL"\] | phlegmer 14.4s
9.4s DD-WRT v23 SP3 (01/26/07) std BCM5352 rev 0
\["OpenWrtDocs/Hardware/Buffalo/WHR-G54S"\] | kjrozema 14.2s 8.3s
OpenWRT RC6 BCM3302 v0.8 \["OpenWrtDocs/Hardware/Linksys/WRT54GL"\] |
kjrozema 12.6s 7.4s OpenWRT RC6 BCM3302 v0.8
\["OpenWrtDocs/Hardware/Linksys/WRT54GL"\] | Hauke | 1.2s | 0.1s |
Ubuntu Linux EdgyEft | Core 2 Duo | | ''IordanIordanov'' | 8.9s | 5.7s |
''WRT350N'' | ''300Mhz'' | &lt;style="vertical-align:
top;"&gt;DooMMasteR &lt;style="vertical-align: top;"&gt;10.14s
&lt;style="vertical-align: top;"&gt;6.19s &lt;style="vertical-align:
top;"&gt;DD-WRT &lt;style="vertical-align: top;"&gt;BCM94704
&lt;style="vertical-align: top;"&gt;OpenWrtDocs | 2008-06-30 | 4.9s |
10.3s | v0.6 | ''ASUS WL-500gP v1'' | ''O.C.300Mhz'' | | ''a9988cd'' |
11.9s | 7.5s | ''Tomato 1.19-ND''| ''BCM5354 rev 2'' | ''LinkToHwPage''
| 2009-02-19 | 3.8s | 4.8s | v0.6 | ''Litestation SR71'' | ''680Mhz'' |

TODO, which bench to use? Tester''' ''' Version of bench''' ''' CPU'''
''' Link to HW page''' ''' ||

== How to compile and run the benchmark ==

:   1.  you need to download it: <attachment:openwrt_cpu_bench_v06.c>
    2.  Compile it

{{{

:   cd &lt;...&gt;/OpenWrt-SDK-Linux-i686-1
    staging\_dir\_mipsel/bin/mipsel-linux-gcc -O0 -o openwrt\_cpu\_bench
    openwrt\_cpu\_bench\_vXX.c =&gt; This will produce a binary
    openwrt\_cpu\_bench =&gt; Be careful to add -O0, we don't want to
    check compiler optimization but CPU capabilities

}}}

:   1.  Or download it precompiled for mipsel here:
        <attachment:openwrt_cpu_bench_v06.bin>
    2.  Or download it precompiled for OpenDebianSlug here:
        \["<attachment:openwrt> cpu bench opendebianslug.bin"\]
    3.  Copy it to your device
    4.  Run it: ./openwrt\_cpu\_bench\_vXX.bin''' '''
    5.  Run it 2 more times and report the average of the 3 runs
    6.  Report in the table above the time it took to run

== Versions of the benchmark ==

:   -   v0.1 : Initial revision
    -   v0.2 : ???
    -   v0.3 : Various bug fixes
    -   v0.4 : Added floating point calculation
    -   v0.5 : Corrected pi benchamrk
    -   v0.6 : Initialization of variables for gcc on NSLU2

= Discussion = ---- . \[...continued from above\] No, this page is still
useless; we're not arguing over what a load average is or what a
benchmark is for. We're simply pointing out the absurdity of this
benchmark.

Almost all the boards in question are the exact same hardware, they're
even based off of the same schematics, otherwise known as a reference
design. It doesn't matter what the brand name on the box is; you're
comparing a board with a 200Mhz Broadcom mips chip to another board with
a 200Mhz Broadcom mips chip and expecting to see dramatic differences.
Worse is the fact that you're actually seeing differences between two
benchmarks being run on identical models -- this alone should tell you
that the testing conditions are flawed.

I'm also not a fan of the "many processors can be safely overclocked"
mentality. Yes you can easily change the clock frequency by setting an
nvram variable, and it may even work on some boards. The problem is what
happens when it doesn't work -- that's where the whole "safe" concept
fails, because when it doesn't work there's often no easy or safe way to
reset it, you actually have to pull apart the device and connect up a
JTAG cable to reset the configuration.

-   mbm

---- . Ah, I understand your argument now, and it does have merit.
However, not all entries in this table are comparing the same hardware.
It is slightly helpful to compare two different architectures or
processors to see which may perform best at CPU or memory intensive
tasks (at least, to the best this synthetic benchmark may tell us).

The differences in performance on identical models (with identical
clocks) is probably due to their load at the time of the testing, or
something else. Although the differences are not great, you are right
that this does indicate the benchmark isn't 100%. However, it's close
enough for now, until someone develops a better benchmark (or better
recommended testing procedures).

The author recommends averaging 3 tests, yet the program encourages
pasting the results of a single run. I think the benchmark could be
improved by running the tests multiple times and finding an average.
This change would probably yield closer results for identical models. I
may make this change myself if the author doesn't.

So, the page, while perhaps far from perfect, isn't useless IMHO. I'd
love to present your opinions here as well as my own. Please induldge us
and let this page live on with your noted, and quite valid, concerns.

-jcollake

--

= See also =

:   -   \["OpenWrtDocs/Benchmarks/OpenSSL"\]


