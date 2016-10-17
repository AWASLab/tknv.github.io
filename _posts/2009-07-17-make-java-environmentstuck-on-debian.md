---
layout: post
title: Make Java environment,Stuck on Debian
date: '2009-07-18T00:32:00.000+09:00'
author: tknv
comments: true
category:
- java
- jruby
- linux
- debian
modified_time: '2009-07-18T01:38:51.700+09:00'
blogger_id: tag:blogger.com,1999:blog-2736766923155041598.post-6655171837905458716
blogger_orig_url: http://yet-another-problem.blogspot.com/2009/07/make-java-environmentstuck-on-debian.html
---

<blockquote><h1>Make Java environment on Debian Lenny</h1>(non free)</blockquote><br />I stuck to ant for jruby.<br /><br />1st.add it your /etc/apt/sources.list <br /><pre code='ruby'><br />deb http://mirrors.kernel.org/debian/ lenny main non-free<br /># this is your main mirrors when you install<br />deb-src http://mirrors.kernel.org/debian/ lenny main non-free<br /># this is your main mirrors when you install<br /><br />deb http://security.debian.org/ lenny/updates main non-free<br />deb-src http://security.debian.org/ lenny/updates main non-free<br /></pre><br />2nd.<br /><pre code='bash'><br />aptitude update<br />aptitude install sun-java6-jdk<br /></pre><br />3rd.if you need.<br />update-java-alternatives -s java-6-sun<br />echo 'JAVA_HOME="/usr/lib/jvm/java-6-sun"' | tee -a /etc/environment
