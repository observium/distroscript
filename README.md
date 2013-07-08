distroscript
============

Simple shell script to return os/distro info

Fixes/Updates: pull request or joe@rewt.org.uk - cheers

Versioning
========

Generally after every commit, or at least after a couple of moderate commits, I increment the version.

When there are significant milestones I will likely create "releases" where the script as of release is bug free.

 * master - use if you are brave, potential for disruptive changes.
 * stable - only changes that I think are worthwhile and won't break compatibility.


Usage
=====

You can either include the script in your own application or execute it directly.
Before sourcing the script, set $DISTROEXEC.

No dependencies, should work on anything ash compatible (ksh, bash etc)

Variables:
 * $OS - Operating System (eg Linux, Solaris, FreeBSD)
 * $KERNEL - Kernel version (eg 3.8.1 or 9.1-RELEASE)
 * $ARCH - platform/bitness (eg amd64 or i386)
 * $DISTRO - Distribution if any (eg Debian, OpenIndiana, pfSense)
 * $DISTROVER - Distribution version if any (eg 12.04, 6.3)
 * $DISTROSCRIPT - Version of the script

If information isn't available or applicable for any of the variables, they are left blank.

Functions:
 * getos - exports OS as the Operating System
 * getkernel - exports KERNEL as the kernel version
 * getarch - exports ARCH as the arch/platform
 * getdistro - exports DISTRO as the distribution
 * getversion - exports VERSION as the distribution version

Notes:
 * On FreeBSD and OpenBSD, where it is a generic install, the distro field is left blank and the distrover is populated with the kernel name.
 * x86_64 is normalised to amd64 on Linux.

Output if executing directly is in the format of OS|KERNEL|ARCH|DISTRO|DISTROVER
