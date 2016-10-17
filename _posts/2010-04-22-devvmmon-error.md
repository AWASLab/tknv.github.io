---
layout: post
title: "/dev/vmmon error"
date: '2010-04-22T18:50:00.000Z'
author: tknv
tags:
- windows
- linux
- module
- VMware
modified_time: '2010-08-28T16:06:01.980Z'
blogger_id: tag:blogger.com,1999:blog-11459759.post-3845894326147697166
blogger_orig_url: http://phichyudebow.blogspot.com/2010/04/devvmmon-error.html
---

When I start VMwarePlayer 2, error occur could not find /dev/vmmon.<br />In google, many people say "use vmware-config.pl", But mostly they do it as same as casting spell.<br />Also my VMware has not that perl file.<br />Maybe depend on user that boot VMwarePlayer and load module list.(need more debug myself)<br />Anyway,load vmmon.ko then ok.<br /><blockquote>1.check where is my vmmon.ko<br />     # vmplayer<br />then it show vmmon.ko path.<br />2.load it<br />     # sudo insmod _that_path_vmmon.ko<br />3.run VMwarePlayer</blockquote><div class="blogger-post-footer">/TKNV</div>