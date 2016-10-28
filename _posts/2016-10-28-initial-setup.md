---
layout: post
title: initial setup
date: 2016-10-28 16:20
update_date: 
cover: /images/cover-initial-setup.jpg
width: 2048
height: 1365
tags:
- debian
- linux
- initial
- python
- ruby
- node
- git
description: VM storage full, cannot wait merging lengthy time. start rebuild env. 
location: local
locale:

---

## initial setup

*at debian 8 64 VM*

install minimum program

```shell
apt-get install vim visudo git rcconf build-essential p7zip-full
```

set default editor

```shell
sudo update-alternatives --config editor
```

 set visudo

```shell
sudo visudo
```

```
god ALL=(ALL) NOPASSWD: ALL
```

interface

```
# if1 Nat primary
allow-hotplug eth0
iface eth0 inet dhcp

# if2 my-lab
allow-hotplug eth1
iface eth1 inet static
        address 169.254.111.111
        netmask 255.255.0.0
        network 169.254.0.0
        broadcast 169.254.255.255
        gateway 169.254.156.196

# if3 co-lab
allow-hotplug eth2
iface eth2 inet dhcp

# if4 Host-Only adopter for VM
allow-hotplug eth3
iface eth3 inet static
        address 192.168.56.101
        netmask 255.255.255.0
```

ruby ( rvm )

```shell
curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
\curl -sSL https://get.rvm.io | bash -s stable --ruby
```

git ( under co net )

```shell
git config --global user.name "me"
git config --global user.email me@example.com
git config --global core.editor vim
git config --global url.https://github.com/.insteadOf git://github.com/
# or
git config --global url."https://".insteadOf git://
git config --global user.signingkey EF4539D6276B2BFB
```

[node](https://github.com/nodesource/distributions)

```shell
# Using Debian, as root
curl -sL https://deb.nodesource.com/setup_6.x | bash -
apt-get install -y nodejs
```

[zsh](https://github.com/sorin-ionescu/prezto)

https://github.com/sorin-ionescu/prezto

python ( virtual env )

```shell
sudo apt-get install python-pip
sudo pip install virtualenv
```

cryptography

```shell
sudo apt-get install libssl-dev libffi-dev python-dev
pip install cryptography
```



