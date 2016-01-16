distroscript
============

<<<<<<< HEAD
Simple shell script to return os/distro info
=======
Simple shell script to return os/distro/version info
>>>>>>> 7b3790895dc37ee8b3dae2d58db95bbad7881162

Pull requests welcome!

Wanted
======

Obviously to implement good support for all platforms I require at least uname outputs and/or any OS specific checks I can do, currently I need info for:

* HP-UX
* VxWorks
* RTOS/QNX

Versioning
========

Generally after every commit, or at least after a couple of moderate commits, I increment the version.

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
 * **$DISTROSCRIPT** - Version of the script

If information isn't available or applicable for any of the variables, they are left blank.

Functions:
 * **getos**         - exports OS as the Operating System
 * **getkernel**     - exports KERNEL as the kernel version/name
 * **getarch**       - exports ARCH as the arch/platform
 * **getdistro**     - exports DISTRO as the distribution name
 * **getversion**    - exports VERSION as the distribution/os version

Notes:
 * On FreeBSD and OpenBSD, where it is a generic install, the distro field is left blank and the distrover is populated with the kernel name.
 * **x86_64** is normalised to **amd64** on Linux.

Output:
 * Default output is pipe deliminated: `OS|KERNEL|ARCH|DISTRO|VERSION`
 * Possible output options are: *pipe*, *twopipe*, *ini*, *export*
 * Set `$DISTROFORMAT` environment variable to change output. Example: `DISTROFORMAT=export ./distro`
