---
layout: post
title: linux wifi AP, ホテルでインターネット共有, ics
date: '2012-01-29T12:21:00.008Z'
author: tknv
tags:
- 11abgn
- windows
- linux
- ad-hoc
- AccessPoint
- iwl4965
modified_time: '2012-02-01T22:25:30.699Z'
thumbnail: http://3.bp.blogspot.com/-54tDHwzGB-4/TylPSyNfM0I/AAAAAAAABB8/aqw5P2xsrt4/s72-c/ad-hoc-network.png
blogger_id: tag:blogger.com,1999:blog-11459759.post-7503190983031242783
blogger_orig_url: http://phichyudebow.blogspot.com/2012/01/linux-wifi-ap-ics.html
---

<h1>linux wifi AP, ホテルでインターネット共有, ics</h1>At a Hotel, two laptops or laptop, ipad, iphone and etc want to connect internet even one line ethernet.  <br />If you are rich like a easy to buy wifi router and also like to bring many goods, please use router. <br />ホテルでイーサーネットケーブルが壁からだらんと一本垂れてる。でも２人でネットに繋ぎたい、片方はwindowsXPのノート、片方はlinuxのノート。linuxノートをad-hocのホストとして２台ともネットに繋ぐことに成功。この方法なら、かなりのノートPCがwifiルータ(ad-hoc)になれると思う。<br />ノート２台でなく、ノートPCにiphone, ipad, androidの組み合わせでもいけるはず。windwos側もホストになるらしいが、試していないしあまり興味もない。<br /><br /><br /><br /><br /><br /><center><a href="http://www.amazon.co.jp/gp/product/1932624449/ref=as_li_tf_il?ie=UTF8&amp;tag=pdb0c-22&amp;linkCode=as2&amp;camp=247&amp;creative=1211&amp;creativeASIN=1932624449"><img border="0" src="http://ws.assoc-amazon.jp/widgets/q?_encoding=UTF8&amp;Format=_SL160_&amp;ASIN=1932624449&amp;MarketPlace=JP&amp;ID=AsinImage&amp;WS=1&amp;tag=pdb0c-22&amp;ServiceVersion=20070822" /></a><img alt="" border="0" height="1" src="http://www.assoc-amazon.jp/e/ir?t=pdb0c-22&amp;l=as2&amp;o=9&amp;a=1932624449" style="border: none !important; margin: 0px !important;" width="1" /></center><br /><blockquote>*This access point is not Master mode, networks are <strong>ad-hoc</strong> connect. Because many wifi devices embedded in Laptop PC can not be Master mode. So impossible to use hostapd.  <br />If a wifi device embedded in Laptop able to be Master mode, it can be Access Point with hostapd. <br /><code>sudo iwconfig wlan0(depend on your wifi device) mode Master</code><br />then you will know whether able to be Access Point or not. <br />This article is tested by iwl4965 device.(iwl4965 can not be Master mode)<br />ほとんどのノートPCで行うことができるはずです。</blockquote><br /><h2>linux host</h2><ul><li>networks</li><a href="http://www.amazon.co.jp/gp/product/B003DQHROU/ref=as_li_tf_il?ie=UTF8&amp;tag=pdb0c-22&amp;linkCode=as2&amp;camp=247&amp;creative=1211&amp;creativeASIN=B003DQHROU"><img border="0" src="http://ws.assoc-amazon.jp/widgets/q?_encoding=UTF8&amp;Format=_SL160_&amp;ASIN=B003DQHROU&amp;MarketPlace=JP&amp;ID=AsinImage&amp;WS=1&amp;tag=pdb0c-22&amp;ServiceVersion=20070822" /></a><img alt="" border="0" height="1" src="http://www.assoc-amazon.jp/e/ir?t=pdb0c-22&amp;l=as2&amp;o=9&amp;a=B003DQHROU" style="border: none !important; margin: 0px !important;" width="1" />  <blockquote>INTERNET--wan--HOTEL ROOM--LAN--(eth0:ethernet cable)LinuxLaptopPC(wlan0:wifi)--(wifi)ipad,--(wifi)iphone,--(wifi)android and etc.&nbsp;</blockquote><div class="separator" style="clear: both; text-align: center;"><a href="http://3.bp.blogspot.com/-54tDHwzGB-4/TylPSyNfM0I/AAAAAAAABB8/aqw5P2xsrt4/s1600/ad-hoc-network.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="http://3.bp.blogspot.com/-54tDHwzGB-4/TylPSyNfM0I/AAAAAAAABB8/aqw5P2xsrt4/s1600/ad-hoc-network.png" /></a></div></ul>もし、不明な事があれば気軽に質問して下さい。Do not be hesitate to ask me about it.<br /><a href="http://yet-another-problem.blogspot.com/2012/01/linux-wifi-ap-ics.html">詳しいやり方、How to be Laptop PC as wifi AP.</a><div class="blogger-post-footer">/TKNV</div>