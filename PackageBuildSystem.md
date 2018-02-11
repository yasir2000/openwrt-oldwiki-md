= New Package Buildsystem =

== Basic Idea ==

:   -   package/: contains directories for each category
    -   package/&lt;category&gt;/: contains directories for each package
        in this category
    -   package/&lt;category&gt;/&lt;package&gt;/: all package specific
        information are in this directory:
        -   Makefile
        -   target.mk
        -   Config.in
        -   ipkg/&lt;package&gt;.control
        -   patches/ (optional)

== Build Files == package/Makefile {{{ \# Main makefile for the packages
include \$(TOPDIR)/rules.mk

include \$(TOPDIR)/package/*/*/target.mk

all: compile install clean: \$(patsubst %,%-clean,\$(package-)
\$(package-y) \$(package-m)) compile: \$(patsubst
%,%-compile,\$(package-y) \$(package-m)) install: \$(patsubst
%,%-install,\$(package-y))

%-prepare:

:   @\[ -f \$(STAMP\_DIR)/.\$@ \] || \$(MAKE) -C \*/\$(patsubst
    %-prepare,%,\$@) prepare @touch \$(STAMP\_DIR)/.\$@

%-compile: %-prepare

:   @\[ -f \$(STAMP\_DIR)/.\$@ \] || \$(MAKE) -C \*/\$(patsubst
    %-compile,%,\$@) compile @touch \$(STAMP\_DIR)/.\$@

%-install: %-compile

:   @\[ -f \$(STAMP\_DIR)/.\$@ \] || \$(MAKE) -C \*/\$(patsubst
    %-install,%,\$@) install @touch \$(STAMP\_DIR)/.\$@

%-rebuild:

:   @rm -f \$(STAMP\_DIR)/.\$(patsubst %-rebuild,%,\$@)-\* \$(MAKE) -C
    \*/\$(patsubst %-rebuild,%,\$@) rebuild

%-clean:

:   @\$(MAKE) -C */\$(patsubst %-clean,%,\$@) clean @rm -f
    \$(STAMP\_DIR)/.\$(patsubst %-clean,%,\$@)-*

}}}

package/Config.in {{{ menu "OpenWrt Package Selection" source
"package/\*/Config.in" endmenu

source "package/Sysconf.in" }}}

package/&lt;category&gt;/Config.in {{{ menu "&lt;category name&gt;"
comment "&lt;category description&gt;" source
"package/&lt;category&gt;/\*/Config.in" endmenu }}}

package/&lt;category&gt;/&lt;package&gt;/target.mk {{{
package-\$([BR2\_PACKAGE]()&lt;PACKAGE&gt;) += &lt;package&gt;
&lt;package&gt;-compile: &lt;package-depencies&gt; }}}

== config Patch == The menuconfig configuration files (\*.in) does
currently not support wildcards in the source-statement. This patch is
required to do this: \["PackageBuildSystemConfigPatch"\].

== Package Categories == \["PackageBuildSystemCategories"\]

== TODO ==

:   -   Write \["PackageCreation"\] for documentation on package
        creation.
    -   Move packages/config/ out of packages/.
    -   Create good categories and assign each package to one category.
    -   Move bootscripts/configuration files which are specific to a
        package or option of a package from
        target/default/target\_skeleton/ into the package?
    -   Shall Sysconf.in stay in package directory?
    -   Commit changes to cvs {{{:)}}}


