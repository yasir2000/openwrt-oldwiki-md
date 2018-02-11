= Wiki system on OpenWrt device = Is there any wiki systems that can be
\[<http://forum.openwrt.org/viewtopic.php?id=6324> installed on OpenWrt
device\]?

----Small table pulling a few small wiki systems. There are many more
though: \[wiki:WikiPedia:List\_of\_wiki\_software see this list\] and to
make detailed comparisons visit \[<http://www.wikimatrix.org>
WikiMatrix\].

| | | | | C libgcc packageYes Very small \~25kB and fast. See DidiWiki
and BuildingPackagesHowTo for binary link. Runs only on port 8000. No
user authentication.| | PHP PHP 4.1.x &gt;= Yes | | | | | | Awk None |
Gawk/CGawk| | Please update! :)

----DidiWiki fits happily on an OpenWrt system's flash memory. You may
need to add a symbolic link in /lib using ''ln -s /lib/libc.so.0
/lib/libgcc\_s.so.1'' Start with: env DIDIWIKIHOME=/opt/share/.didiwiki
didiwiki Where didiwiki is the binary file's name and .didiwiki is a
directory created for storing data files. Warning: If you don't start
DidiWiki with the env command your pages will be saved in the home
(/tmp) directory and wil be lost on rebooting!
