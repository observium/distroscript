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
Before sourcing the script, set `$DISTROEXEC`.

No dependencies, should work on anything sh compatible environment (ksh, bash etc)

Variables:
 * **$OS**           - Operating System (eg Linux, Solaris, FreeBSD)
 * **$KERNEL**       - Kernel version/name (eg 3.13.0-68-generic, GENERIC#50)
 * **$ARCH**         - platform/bitness (eg amd64 or i386)
 * **$DISTRO**       - Distribution name (eg Debian, OpenIndiana, pfSense), note for some OSes this empty (eg for FreeBSD, OpenBSD, AIX)
 * **$VERSION**      - Distribution (or OS if $DISTRO empty) version (eg 12.04, 6.3)
 * **$VIRT**         - Virtualisation tech, if any (or the OS is useful enough to tell us)
 * **$CONT**         - Container tech, if any (or the OS is useful enough to tell us)
 * **$DISTROSCRIPT** - Version of the script

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
 * On FreeBSD and OpenBSD, where it is a generic install, the distro field is left blank and the distrover is populated with the kernel name.
 * **x86_64** is normalised to **amd64**, **ix86** is normalised to **i386**.
 * Values for **VIRT** and **CONT** use the naming convention from **[systemd-detect-virt](https://www.freedesktop.org/software/systemd/man/systemd-detect-virt.html "systemd-detect-virt")** with the exception of FreeBSD jails.
 * Where we can identify the container, it is presented colon seperated (in the case of pipe), e.g **kvm:jail**.
 * If the DMI strings or virtual cpu names have been changed, or running on a particularly old OS there is a likely chance that the detected virtualisation will be wrong.

Output:
 * Default output is pipe deliminated: `OS|KERNEL|ARCH|DISTRO|VERSION|VIRT[:CONT]`
 * Possible output options are: *pipe*, *twopipe*, *ini*, *export*, *json*
 * Set `$DISTROFORMAT` environment variable to change output. Example: `DISTROFORMAT=export ./distro`
