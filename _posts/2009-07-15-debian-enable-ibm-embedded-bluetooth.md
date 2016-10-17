---
layout: post
title: Debian Enable IBM embedded bluetooth
date: '2009-07-15T21:51:00.000+09:00'
author: tknv
comments: true
category:
- bluetooth
- linux
- debian
- ibm
modified_time: '2009-07-15T22:16:02.187+09:00'
blogger_id: tag:blogger.com,1999:blog-2736766923155041598.post-5575750783321192798
blogger_orig_url: http://yet-another-problem.blogspot.com/2009/07/debian-enable-ibm-embedded-bluetooth.html
---

Bluetooth<br /><br />Works out of the box using the bluetooth and hci_usb driver. The laptop's Bluetooth device is USB-attached internally and shows up in lsusb as:<br /><br /> $ lsusb<br /> Bus 003 Device 004: ID 1668:0441 Actiontec Electronics, Inc. [hex] IBM Integrated Bluetooth II<br /><br />The device is not enabled per default though (which is a good thing), you can enable it like this:<br /><br /> $ echo enable > /proc/acpi/ibm/bluetooth<br /><br />Disabling is equally simple:<br /><br /> $ echo disable > /proc/acpi/ibm/bluetooth<br /><br />After that, you can use hcitool / hciconfig etc. as usual, and/or enable more related stuff with /etc/init.d/bluetooth restart.<br /><a href="http://www.linuxjournal.com/article/9466">also</a>
