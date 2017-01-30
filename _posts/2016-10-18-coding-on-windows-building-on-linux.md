---
layout: post
title: "Coding on Windows, building on VirtualBox Linux. No workgroup no problem"
date: 2016-10-18 01:20:33 +0700
update_date: 2017-01-30 12:48:33 +0700
author: tknv
cover: /images/cover-coding-on-win.jpg
width: 1280
height: 947
comments: true
tags: 
- linux
- windows
- samba
- Host-only
- virtualbox 
description: mount VM Linux files on Windows.
---

## Anyway mount Linux files on Windows without workgroup.   

### Setup VirtualBox Host-only network  

#### Add VirtualBox Host-Only Ethernet Adapter  

*set host machine IP i.e. Windows machine IP.*

from VirtualMox Manager (File > Preferences...)

![Oracle VM VirtualBox Manager](/images/2016-10-18/VM-sys-pref.PNG)

add Host-only Networks [^1] ( Network > Host-only Networks **TAB**). Click **+** icon.

![add Host-only Networks](/images/2016-10-18/VM-sys-pref-add-hoif.PNG)

set static IP to Host Adapter [^2] . Static is easy for mounting point. This IP assign to Windows machine i.e. Host machine IP.

![set static IP](/images/2016-10-18/VM-sys-pref-add-hoif-host-ip-st.PNG)

disable DHCP for static IP.

![disable DHCP](/images/2016-10-18/VM-sys-pref-add-hoif-host-no-dhcp.PNG)

#### Enable VirtualBox Host-Only Ethernet Adapter

*set virtual machine ethernet adapter. i.e. virtual machine Linux interface.*

enable network adapter(virtual machine > Settings > Network). It is Adapter 4(i.e. assigned to eth3 at virtual machine).

![enable host only adapter](/images/2016-10-18/VM-client-net-pref.PNG)

configure virtual machine eth3 IP to static.

```shell
sudo vim /etc/network/interfaces
```

```sh
# 4th network interface
auto eth3
allow-hotplug eth3
iface eth3 inet static
        address 192.168.56.101
        netmask 255.255.255.0
        network 192.168.56.0
```

apply configuration.

```sh
sudo ifdown eth3
sudo ifup eth3
```

good if test ping OK.

### Setup samba

get and install samba

```shell
sudo apt-get install samba
```

add user password (user is me, then one less trouble)

```shell
smbpasswd -a me
```

configure samba (by any editor)

```shell
vim /etc/samba/smb.conf
```

```sh
[src]
path = /home/me/src
available = yes
valid users = me
read only = no
browsable = yes
public = yes
writable = yes
```

!!! warning 'This is very important for host is windows 7, otherwise file transfer is very slow.'

```
[global]
max protocol = NT1
```

apply configuration

```sh
sudo /etc/init.d/samba restart
```

### Mount Linux(virtual machine) files on Windows(host)

*mount Linux file by Map network drive.*

mapping network drive(Start > Computer > Map network drive). **Click** Map network drive.

![Map network drive](/images/2016-10-18/map-net-dev.PNG)

assign Drive and folder.

- Drive: select which is not used. (at figure Y: is fine)

- Folder: \\\virtual machine host only interface IP\samba directory name

  at following this contents, Folder:\\\192.168.56.101\src

Check **"Connect using different credentials"**

![network drive](/images/2016-10-18/map-net-dev-popup.PNG)

if mounted, show below figure. 

![network drive](/images/2016-10-18/net-dev-fig.PNG)

Reference:  

[^1]: [6.7. Host-only networking](https://www.virtualbox.org/manual/ch06.html)
[^2]: [8.37. VBoxManage hostonlyif](https://www.virtualbox.org/manual/ch08.html#vboxmanage-hostonlyif)




