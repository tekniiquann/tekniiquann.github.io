---
layout: post
title: "Permanently Disable Network Manager On Debian Based OS"
categories_short_name: shell
meta: "shell_and_OS"
type: "publication"
---

There is a glitch caused by [Systemd](https://en.wikipedia.org/wiki/Systemd) if not many, on my [Debian](https://wiki.debian.org/) 11 desktop. 
The [Netwrk Manager](https://help.ubuntu.com/community/NetworkManager#Stopping_and_Disabling_NetworkManager) always waits for loading "remote" booting components e.g., drivers, whenever the operating system is booting . 
This is probably a design "feature" of UNIX-like system, which the idea could be tracked back to [Time-sharing](https://en.wikipedia.org/wiki/Time-sharing#Time-sharing) concept of system running on 
[UNIX](https://en.wikipedia.org/wiki/Unix).

This "feature" made sense at 1980s but turns to be bugg-ish on modern hardware. Nowadays, even a single board computer could hold all drivers on a SD card, and **operating system with desktop environment (DE) has been deigned to get online after DE login for obviously security reason**. As a result, many Debian/Ubuntu-based system frequently been noticed [stuck in Network-Manager-wait-online.service](https://askubuntu.com/questions/1018576/what-does-networkmanager-wait-online-service-do) stage during booting, here is the case for [Linux Mint](https://forums.linuxmint.com/viewtopic.php?t=282437).   

As the answer of [this StackExcange](https://askubuntu.com/questions/1091653/how-do-i-disable-network-manager-permanently) suggested, one should delete/disable Network Manager dependent on the DEs. For [Gnome](https://apps.gnome.org/), deletion is NOT a option. Instead, one should stop & disable Network Manager permanently following [this Ubuntu community doc page](https://help.ubuntu.com/community/NetworkManager#Stopping_and_Disabling_NetworkManager), run these shell commands
{% highlight console %}
~# systemctl stop NetworkManager.service
~# systemctl stop NetworkManager-wait-online.service
~# systemctl stop NetworkManager-dispatcher.service
~# systemctl stop network-manager.service
{% endhighlight %}
to stop the current service, and then run these commands
{% highlight console %}
~# systemctl disable NetworkManager.service
~# systemctl disable NetworkManager-wait-online.service
~# systemctl disable NetworkManager-dispatcher.service
~# systemctl disable network-manager.service
{% endhighlight %}
to permanently disable Network Manager from starting during boot process, then reboot system.

- last time edited @10th. Feb. 2024
