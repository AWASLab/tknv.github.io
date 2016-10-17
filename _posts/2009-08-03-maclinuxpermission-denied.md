---
layout: post
title: MacのファイルがLinuxでPermission denied
date: '2009-08-03T13:10:00.000+09:00'
author: tknv
comments: true
category:
- linux
- mac
modified_time: '2009-08-03T13:23:23.103+09:00'
blogger_id: tag:blogger.com,1999:blog-2736766923155041598.post-42120064453366459
blogger_orig_url: http://yet-another-problem.blogspot.com/2009/08/maclinuxpermission-denied.html
---

Macのデータを外付けHDにバックアップして、Linuxでファイルを見ようとしたら、Permission denied.</br><br />だいたい、画像(iphotoの画像)、音楽(itunesのファイル)などのフォルダが見られない。</br><br />Linuxにてrootになって、chown +R ユーザ /そのフォルダ/　</br><br />すると、ファイルを参照することができる。<br />もともとマックでどのユーザが作ったフォルダかどうかによります。
