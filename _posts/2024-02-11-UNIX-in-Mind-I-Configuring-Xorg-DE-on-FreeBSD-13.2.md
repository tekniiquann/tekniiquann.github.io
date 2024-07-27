---
layout: post
title: "UNIX in Mind (I) -- Configuring Xorg DE on FreeBSD 13.2 "
categories_short_name: shell
meta: "shell_and_OS"
---

The historically impacts of [UNIX](https://en.wikipedia.org/wiki/Unix) on computer science and computer technologies are going forever along every hardware ruining UNIX-like system. 
Unfortunately, quite often, the original creators of revolutionary ideas finally are not those, who boost the ideas into absolutely market success. The UNIX went through same history pattern,
as passed by the early success of 1980s, [UNIX war](https://en.wikipedia.org/wiki/Unix_wars), then was substituted by [Linux](https://en.wikipedia.org/wiki/Linux). Except [Mac OS](https://en.wikipedia.org/wiki/MacOS) and [Sun OS/Solaris](https://en.wikipedia.org/wiki/Oracle_Solaris), generally people is not bothered to mention UNIX. Instead, [Berkeley Software Distribution](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution) family of operating system a.k.a BSD system, is the direct descendants of UNIX, which is mostly used as "BSD-UNIX" in modern time.

Though almost every Linux user knows about UNIX in some level, it actually NOT such straightforward and NOT such easy for a mainstream **desktop** Linux-distro user to go and pick up a BSD system without any documentations reading and searching. One of many significant reasons may be BSD is arguably "less bloated" than many mainstream Linux-distro. I don't think I exaggerate, because the popularity of modern Linux desktop distributions are actually based upon the huge mount of applications developed for Linux desktop. And they do not really care abut [UNIX philosophy](https://en.wikipedia.org/wiki/Unix_philosophy#Origin). These very useful and helpful applications significantly reduces thresholds of skills for using a shell-based operating system. Just like Python makes start-up-programming easier, and also makes many users "un-wish" or lacking of skill to handle low level developments and issues appearing in static languages, user-friendly Linux-desktop frequently makes UNIX/BSD looks brutal and tough for general Linux-desktop user.

This is the first episode of explore and using experiences of BSD as a ten years GNU/Linux user and HPC developer.
The BSD I am using is [FreeBSD 13](https://www.freebsd.org/), simply because it is mainstream BSD supported by [FreeBSD Foundation](https://freebsdfoundation.org/), with nice documentations and source code, and also a cool looking logo.

<img src="/pictures/FREEBSD_Logo.png" alt="centered image" width="500" height="auto"> 

The differences between FreeBSD and common GNU/Linux distros show themselves at the very beginning stage of running OS --- the installation. The filesystem which FreeBSD uses is [Z FileSystem/ZFS](https://en.wikipedia.org/wiki/ZFS) or [UFS](https://en.wikipedia.org/wiki/Unix_File_System), while the default filesystem of GNU/Linux distros is [ext4/3](https://en.wikipedia.org/wiki/Ext4). The advance technologies which ZFS offers such as [zpool](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-zpool-create) is not topic of this article, but one should at least know that the PC desktop FreeBSD system will be installed in one zpool if one just following the installation instructions from FreeBSD HandBook.

Moving forward, we can simply follow the [installation instructions of FreeBSD HandBook](https://docs.freebsd.org/en/books/handbook/bsdinstall/#bsdinstall-start). Before real installation kicked off, we should prepare the bootable Installation Media,
and download right version which matches your hardware is key e.g., don't try to put DVD image into USB driver. 
The download instruction can be found from at [here](https://docs.freebsd.org/en/books/handbook/bsdinstall/#bsdinstall-installation-media).

If you wish to use USB deriver/memstick image as I did, then plug USB driver into computer and **umount** it and run
```console
~$ dd if=FreeBSD-xx.x-RELEASE-amd64-memstick.img of=/dev/daX bs=1M conv=sync
```
to write bootable file on USB driver, here the `/dev/daX` is device name, and should be checked out by `lsblk` or `ls -tl /dev` command. After bootable image is written down, reboot the computer and get into boot option session, and choose USB driver. In this stage, many machines requires boot hierarchy change.

If the image is successfully loaded and no error pops up in boot process, we will get into installation session immediately. For processing correctly, one should follow the [installation instructions of FreeBSD HandBook](https://docs.freebsd.org/en/books/handbook/bsdinstall/#bsdinstall-start) and also the great demonstration provided by [DJ Ware](https://www.youtube.com/watch?v=O3G1v0BRjxs&list=PLWK00SLo2KcSf2X1DDZK6NS0dg_EJb9Ls). **Please don't forget add yourself as user with Video and Wheel privilege and pre-install `pkg`**!

After system reboot into a tty login screen, we can now install [display server](https://en.wikipedia.org/wiki/Windowing_system#Display_server_communications_protocols) and desktop environment. This stage is hardware-dependent. For a computer equipped with Nvidia or AMD graphic cards, the proper drivers must be installed and configured to be loaded when bootup. Here I use intel i915 driver, read [here](https://docs.freebsd.org/en/books/handbook/x11/#x-graphic-card-drivers) to see what's suitable for different cases.
```console
~# pkg install xorg 
~# pkg install drm-kmod
~# sysrc kld_list+=i915kms
```
The last command adds the graphic driver module to `/etc/rc.conf` file for boot-up auto loading. Next let's install the desktop environment (DE), here `Gnome` is used as example, for `KDE`, `Mate` and `xfce` etc, see [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/desktop/#desktop-environments).
```console
~# pkg install gnome
~# sysrc dbus_enable="YES"
~# sysrc gdm_enable="YES"
```
The last two commands turn on Data-Bus and Gnome Display management (GDB) during boot. After this succeeded, reboot and see the gnome login page. 

See you in UNIX in Mind (II)!

- last time edited @27th. July. 2024
