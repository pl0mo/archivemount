What is it
==========

Archivemount is a piece of glue code between libarchive
(<http://people.freebsd.org/~kientzle/libarchive/>) and FUSE
(<http://fuse.sourceforge.net>). It can be used to mount a (possibly
compressed) archive (as in .tar.gz or .tar.bz2) and use it like an
ordinary filesystem.

Requirements
============

You will need autoconf, libarchive-dev and libfuse-dev for building
archivemont, as well as GNU make and gcc of course.

Installation
============

From version 0.6.0 on archivemount uses autoconf, so just do the normal

autoreconf -i ./configure && make && sudo make install

to build archivemount and install it to /usr/local. For building it,
libfuse is needed in version 2.6 or higher, including its headerfiles of
course (on most distributions those can be found in a package named
something like libfuse-dev or so).

Usage
=====

archivemount \<options\> \<archive\> \<mountpoint\> Options are the
normal fuse mount options, nothing special supported yet.

Write support
=============

Writing to an archive with libarchive is unfortunately not possible. In
order to provide write support thus the whole archive has to be
recreated; this requires two things: space and time. To optimize at
least the timely behaviour, archives are recreated only once: at the
time of unmount. If there are any problems creating the new archive -
bad luck, the changes are lost. Some checks are run when mounting the
archive to determine if it can be mounted writeable, but there is no
guarantee. Also note that unmounting a fuse filesystem is NOT
necessarily completed when the unmount command returns. Although
unmounting takes a long time already, fuse backgrounds the process and
lets the unmount command return early. You can check on the real state
of unmounting by checking the process list for archivemount.

THERE IS ALSO NO GUARANTEE THAT DATA IS WRITTEN CORRECTLY. DO NOT TRUST
THIS SOFTWARE! A backup is made of the original archive (with .orig
appended to the name), but please understand that I, the author of
archivemount, do not guarantee anything at all about the state of your
data and I am not responsible if you lose vital information by using
this software. YOU HAVE BEEN WARNED!

Archive formats
===============

A note about archive and compression formats: libarchive supports a lot
more formats for reading than it does for writing. Archivemount tries to
do a sensible conversion here when writing the changed archive because I
think most people do not care a lot about the exact version of tar used
inside the .tgz file as long as their favourite archive tool can still
cope with the file.
