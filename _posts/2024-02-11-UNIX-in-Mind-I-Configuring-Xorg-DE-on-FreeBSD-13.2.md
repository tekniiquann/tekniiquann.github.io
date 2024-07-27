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

The differences between FreeBSD and common GNU/Linux distro show themselves at the very begining stage of running the OS --- the installation 

- last time edited @27th. July. 2024
