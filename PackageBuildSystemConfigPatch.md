This patch adds wildcard for source statements in \*.in files used by
{{{config}}}. {{{ diff -Nur config.orig/lex.zconf.c\_shipped
config/lex.zconf.c\_shipped --- config.orig/lex.zconf.c\_shipped
2005-07-10 13:43:30.000000000 -0400 +++ config/lex.zconf.c\_shipped
2005-07-10 13:43:36.000000000 -0400 @@ -20,6 +20,7 @@ \#include
&lt;string.h&gt; \#include &lt;errno.h&gt; \#include &lt;stdlib.h&gt;
+\#include &lt;glob.h&gt;

> /\* end standard C headers. \*/

@@ -3622,32 +3623,52 @@

> void zconf\_nextfile(const char \*name) {

-   struct file \*file = file\_lookup(name);
-   struct buffer *buf = malloc(sizeof(*buf));

- memset(buf, 0, sizeof(*buf)); -- current\_buf-&gt;state =
YY\_CURRENT\_BUFFER; - zconfin = zconf\_fopen(name); - if (!zconfin) { -
printf("%s:%d: can't open file "%s"n", zconf\_curname(),
zconf\_lineno(), name); + size\_t i; + int retval; + glob\_t files; +
char*filename; + struct file *file; + struct buffer*buf; + + retval =
glob(name, GLOB\_ERR | GLOB\_MARK, NULL, &files); + if (retval ==
GLOB\_NOSPACE | retval == GLOB\_NOMATCH) { + printf("%s:%d: glob failed:
%s "%s"n", zconf\_curname(), zconf\_lineno(), + retval == GLOB\_NOSPACE
? "failed to allocate memory" : + retval == GLOB\_ABORTED ? "read error"
: "no match", + name); exit(1); } -
zconf\_switch\_to\_buffer(zconf\_create\_buffer(zconfin,YY\_BUF\_SIZE));
- buf-&gt;parent = current\_buf; - current\_buf = buf;

-   if (file-&gt;flags & FILE\_BUSY) {
-   printf("recursive scan (%s)?n", name);
-   exit(1);
-   }
-   if (file-&gt;flags & FILE\_SCANNED) {
-   printf("file %s already scanned?n", name);
-   exit(1);
-   for (i = files.gl\_pathc-1; i != (size\_t)-1; --i) {

+ filename = files.gl\_pathv\[i\]; + + file = file\_lookup(filename); +
buf = malloc(sizeof(*buf)); + memset(buf, 0, sizeof(*buf)); +
current\_buf-&gt;state = YY\_CURRENT\_BUFFER; + zconfin =
zconf\_fopen(filename); + if (!zconfin) { + printf("%s:%d: can't open
file "%s"n", + zconf\_curname(), zconf\_lineno(), filename); + exit(1);
+ } +
zconf\_switch\_to\_buffer(zconf\_create\_buffer(zconfin,YY\_BUF\_SIZE));
+ buf-&gt;parent = current\_buf; + current\_buf = buf; + + if
(file-&gt;flags & FILE\_BUSY) { + printf("recursive scan (%s)?n",
filename); + exit(1); + } + if (file-&gt;flags & FILE\_SCANNED) { +
printf("file %s already scanned?n", filename); + exit(1); + } +
file-&gt;flags = FILE\_BUSY; - file-&gt;lineno = 1; - file-&gt;parent =
current\_file; - current\_file = file; }

> static struct buffer \*zconf\_endfile(void)

diff -Nur config.orig/zconf.l config/zconf.l --- config.orig/zconf.l
2005-07-10 13:43:30.000000000 -0400 +++ config/zconf.l 2005-07-10
13:43:36.000000000 -0400 @@ -12,6 +12,7 @@ \#include &lt;stdlib.h&gt;
\#include &lt;string.h&gt; \#include &lt;unistd.h&gt; +\#include
&lt;glob.h&gt;

> \#define LKC\_DIRECT\_LINK \#include "lkc.h"

@@ -301,32 +302,52 @@

> void zconf\_nextfile(const char \*name) {

-   struct file \*file = file\_lookup(name);
-   struct buffer *buf = malloc(sizeof(*buf));

- memset(buf, 0, sizeof(*buf)); -- current\_buf-&gt;state =
YY\_CURRENT\_BUFFER; - yyin = zconf\_fopen(name); - if (!yyin) { -
printf("%s:%d: can't open file "%s"n", zconf\_curname(),
zconf\_lineno(), name); + size\_t i; + int retval; + glob\_t files; +
char*filename; + struct file *file; + struct buffer*buf; + + retval =
glob(name, GLOB\_ERR | GLOB\_MARK, NULL, &files); + if (retval ==
GLOB\_NOSPACE | retval == GLOB\_NOMATCH) { + printf("%s:%d: glob failed:
%s "%s"n", zconf\_curname(), zconf\_lineno(), + retval == GLOB\_NOSPACE
? "failed to allocate memory" : + retval == GLOB\_ABORTED ? "read error"
: "no match", + name); exit(1); } -
yy\_switch\_to\_buffer(yy\_create\_buffer(yyin, YY\_BUF\_SIZE)); -
buf-&gt;parent = current\_buf; - current\_buf = buf;

-   if (file-&gt;flags & FILE\_BUSY) {
-   printf("recursive scan (%s)?n", name);
-   exit(1);
-   }
-   if (file-&gt;flags & FILE\_SCANNED) {
-   printf("file %s already scanned?n", name);
-   exit(1);
-   for (i = files.gl\_pathc-1; i != (size\_t)-1; --i) {

+ filename = files.gl\_pathv\[i\]; + + file = file\_lookup(filename); +
buf = malloc(sizeof(*buf)); + memset(buf, 0, sizeof(*buf)); +
current\_buf-&gt;state = YY\_CURRENT\_BUFFER; + zconfin =
zconf\_fopen(filename); + if (!zconfin) { + printf("%s:%d: can't open
file "%s"n", + zconf\_curname(), zconf\_lineno(), filename); + exit(1);
+ } +
zconf\_switch\_to\_buffer(zconf\_create\_buffer(zconfin,YY\_BUF\_SIZE));
+ buf-&gt;parent = current\_buf; + current\_buf = buf; + + if
(file-&gt;flags & FILE\_BUSY) { + printf("recursive scan (%s)?n",
filename); + exit(1); + } + if (file-&gt;flags & FILE\_SCANNED) { +
printf("file %s already scanned?n", filename); + exit(1); + } +
file-&gt;flags = FILE\_BUSY; - file-&gt;lineno = 1; - file-&gt;parent =
current\_file; - current\_file = file; }

> static struct buffer \*zconf\_endfile(void)

}}}
