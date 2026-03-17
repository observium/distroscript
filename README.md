distroscript
============

~~Simple~~ shell script to return os/distro/version/virt info

Wanted
======

Obviously to implement good support for all platforms I require at least uname outputs and/or any OS specific checks I can do, currently I need info for:

* HP-UX
* VxWorks
* Hurd
* Minix

Known supported
===============

* **Linux**
* **Free/Open/Net/Dragonfly BSD**
* **Cygwin/MingW**
* **QNX**
* **AIX**
* **Solaris/SunOS**
* **macOS/IOS**

Usage
=====

You can either include the script in your own application or execute it directly.

No dependencies, should work in any sh compatible environment (ksh, bash etc)

CLI options:
 * **-f \<format\>** - Output format: *pipe* (default), *twopipe*, *json*, *ini*, *export*
 * **-o \<out\>**    - Show specific parameter only: *os*, *kernel*, *arch*, *distro*, *version*, *virt*, *cont*
 * **-h**            - Show help
 * **-v**            - Show version

Examples:
```
./distro                      # default pipe output
./distro -f json              # JSON output
./distro -o virt              # show only virtualisation
DISTROFORMAT=export ./distro  # set format via environment variable
```

Sourcing
--------

To source the script in your own application, set `$DISTROEXEC` before sourcing to prevent automatic execution:

```sh
DISTROEXEC=1
. /path/to/distro
getos
getdistro
getversion
echo "$DISTRO $VERSION"
```

Variables:
 * **$OS**           - Operating System (eg Linux, Solaris, FreeBSD)
 * **$KERNEL**       - Kernel version/name (eg 3.13.0-68-generic, GENERIC#50)
 * **$ARCH**         - platform/bitness (eg amd64 or i386)
 * **$DISTRO**       - Distribution name (eg Debian, OpenIndiana, pfSense), note for some OSes this is empty (eg for FreeBSD, OpenBSD, AIX)
 * **$VERSION**      - Distribution (or OS if $DISTRO empty) version (eg 22.04.5, 6.3)
 * **$VIRT**         - Virtualisation tech, if any (or the OS is useful enough to tell us)
 * **$CONT**         - Container tech, if any (or the OS is useful enough to tell us)
 * **$DISTROSCRIPT** - Version of the script

**Note:** The variables above are no longer exported just by sourcing the script, you need to explicitly run one or more of the functions below.

If information isn't available or applicable for any of the variables, they are left blank.

Functions:
 * **getos**         - exports OS as the Operating System
 * **getkernel**     - exports KERNEL as the kernel version/name
 * **getarch**       - exports ARCH as the arch/platform
 * **getdistro**     - exports DISTRO as the distribution name
 * **getversion**    - exports VERSION as the distribution/os version
 * **getvirt**       - exports VIRT as the virtualisation technology
 * **getcont**       - exports CONT as the container technology

Notes:
 * On FreeBSD and OpenBSD, where it is a generic install, the distro field is left blank and the version is populated with the kernel name.
 * **x86_64** is normalised to **amd64**, **ix86** is normalised to **i386**.
 * Values for **VIRT** and **CONT** use the naming convention from **[systemd-detect-virt](https://www.freedesktop.org/software/systemd/man/systemd-detect-virt.html "systemd-detect-virt")** with the exception of FreeBSD jails.
 * If the DMI strings or virtual cpu names have been changed, or running on a particularly old OS there is a likely chance that the detected virtualisation will be wrong.

Output:
 * Default output is pipe delimited: `OS|KERNEL|ARCH|DISTRO|VERSION|VIRT|CONT`
 * Possible output options are: *pipe*, *twopipe*, *ini*, *export*, *json*
 * Set `$DISTROFORMAT` environment variable or use `-f` flag to change output

Installation
============

```sh
curl -o /usr/local/bin/distro https://raw.githubusercontent.com/observium/distroscript/master/distro
chmod +x /usr/local/bin/distro
```

Verify:
```
/usr/local/bin/distro
```

Expected output (example):
```
Linux|5.15.0-91-generic|amd64|Ubuntu|22.04.5||
```

Net-SNMP integration
--------------------

Add to `/etc/snmp/snmpd.conf`:
```
extend .1.3.6.1.4.1.2021.7890.1 distro /usr/local/bin/distro
```

If `extend` is not supported (older net-snmp), use `exec` instead:
```
exec .1.3.6.1.4.1.2021.7890.1 distro /usr/local/bin/distro
```

Restart snmpd:
```sh
service snmpd restart
```

Test via SNMP:
```sh
snmpwalk -v2c -c <community> <hostname> .1.3.6.1.4.1.2021.7890.1
```

See the [Observium documentation][1] for more details.

[1]: https://docs.observium.org/distro_script/ "Distro Script - Observium Docs"
