---
layout: post
title: "Push and pull Github by SGit on Android."
date: 2015-02-24 21:59:49 +0700
comments: true
categories: 
- git
- SGit
- Android
- Github
---
## All android(rooted) needs SGit and Terminal Emulator.
**On [Terminal Emulator](https://f-droid.org/repository/browse/?fdfilter=terminal&fdid=jackpal.androidterm)**

```bash
su
ssh-keygen -t rsa -C "yo_mail@foo.com"
# /data/.ssh is ok, just enter. no need change path.
# no password is ok, just enter. maybe work even set password. But need more password? Phone-lock is enough.
cp /data/.ssh/id_rsa /your-sdcard-path
cp /data/.ssh/id_rsa.pub /your-sdcard-path
```
Add /your-sdcard-path/id_rsa.pub at https://github.com/settings/ssh .  
**On [SGit](https://f-droid.org/repository/browse/?fdfilter=sgit&fdid=me.sheimi.sgit)**  
From menu *Private Keys*, add /your-sdcard-path/id_rsa .  
Push **+**, then  
- git@github.com:you/you.github.io.git at *Remote URL*  
- your-repo-name-as-you-like at *Local Path*  
- empty at *Username*  
- empty at *Password*  
SGit uses root for user by default and when ssh-keygen, did not make password on terminal emulator.  
Less, but ok. I tested android 4.4.4.  

 
