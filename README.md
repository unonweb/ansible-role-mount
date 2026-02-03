NOTES
=====

- controls active and configured mount points in /etc/fstab

MOUNT OPTIONS
=============

Links
-----

- https://wiki.ubuntuusers.de/fstab/
- https://www.freedesktop.org/software/systemd/man/latest/systemd.mount.html
- https://www.samba.org/~ab/output/htmldocs/manpages-3/mount.cifs.8.html


General Options
---------------

## defaults 
rw,suid,dev,exec,auto,nouser,async

## users   
Allow any user to mount and to unmount the filesystem, even when some other ordinary user mounted it. 
Implies: noexec, nosuid, nodev

## user
Allow an ordinary user to mount the filesystem. The name of the mounting user is written to the mtab file (or to the private libmount file in /run/mount on systems without a regular mtab) so that this same user can unmount the filesystem again. 

Implies: noexec, nosuid, nodev

## nofail
With nofail, this mount will be only wanted, not required, by local-fs.target or remote-fs.target. Moreover the mount unit is not ordered before these target units. This means that the boot will continue without waiting for the mount unit and regardless whether the mount point can be mounted successfully.

# x-systemd.device-timeout=
Configure how long systemd should wait for a device to show up before giving up on an entry from /etc/fstab. Specify a time in seconds 
* mount process won't start at all if the device is not present

# x-systemd.mount-timeout=
Configure how long systemd should wait for the mount command to finish before giving up on an entry from /etc/fstab. Specify a time in seconds
* mount process has already begun

## uid
Sets the uid that will own all files on the mounted filesystem. It may be specified as either a username or a numeric uid. For mounts to servers which do support the CIFS Unix extensions, such as a properly configured Samba server, the server provides the uid, gid and mode so this parameter should not be specified unless the server and client uid and gid numbering differ. 

For servers which do not support the CIFS Unix extensions, the default uid (and gid) returned on lookup of existing files will be the uid (gid) of the person who executed the mount (root, except when mount.cifs is configured setuid for user mounts) unless the "uid=" (gid) mount option is specified.

# _netdev
Normally the file system type is used to determine if a mount is a "network mount", i.e. if it should only be started after the network is available. Using this option overrides this detection and specifies that the mount requires network.

* Network mount units are ordered between **remote-fs-pre.target** and **remote-fs.target**, instead of **local-fs-pre.target** and **local-fs.target**.
They also pull in network-online.target and are ordered after it and network.target.

## cache=loose
Warnung im Stackexchange Beitrag  "cache=loose can cause data corruption when multiple readers and writers are working on the same files."
Findet sich aber nicht (mehr) im Link zu den man-pages

Davfs Options
-------------

- https://manpages.debian.org/trixie/davfs2/mount.davfs.8.en.html#OPTIONS