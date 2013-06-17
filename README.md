distroscript
============

Simple shell script to return os/distro info

Fixes/Updates: pull request or joe@rewt.org.uk - cheers

Output
======

Output is in the format of OS|KERNELVER|ARCH|DISTRO|DISTROVER

Where the info isn't available the field is left blank, there are some notable exceptions:

 * On FreeBSD and OpenBSD, where it is a generic install, the distro field is left blank and the distrover is populated with the kernel name.
 * AIX sucks, so may be lots of blank fields.
