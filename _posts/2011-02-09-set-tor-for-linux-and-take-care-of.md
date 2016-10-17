---
layout: post
title: Set Tor for linux and take care of internet censorship
date: '2011-02-09T15:24:00.000Z'
author: tknv
tags:
- linux
- tor
- censorship
- network
modified_time: '2011-02-09T15:24:43.706Z'
blogger_id: tag:blogger.com,1999:blog-11459759.post-5950408608987773442
blogger_orig_url: http://phichyudebow.blogspot.com/2011/02/set-tor-for-linux-and-take-care-of.html
---

<pre>aptitude install vidalia polipo</pre><pre>wget http://www.torproject.org/dist/tor-0.2.1.29.tar.gz</pre><pre>tar -xzvf tor-0.2.1.29.tar.gz</pre><pre>cd tor-xxx</pre><pre>./configure &amp;&amp; make &amp;&amp; make install</pre><pre>wget https://gitweb.torproject.org/torbrowser.git/blob_plain/HEAD:/build-scripts/config/polipo.conf</pre><pre>cp polipo.conf /etc/polipo/config</pre><pre></pre><pre>vidalia -&gt; settings -&gt; general</pre><pre>Tor</pre><pre>check:Start the Tor software when Vidalia starts</pre><pre>path:/usr/local/bin/tor</pre><pre></pre><pre>Proxy Application(optional)</pre><pre>check:Start a proxy applicaion when Tor starts</pre><pre>path:/usr/bin/polipo</pre><pre>ProxyApplicaion Arguments:</pre><pre>-c /etc/polipo/config</pre><pre></pre><pre>FireFox</pre><pre>install Tor addon</pre><pre>FireFox -&gt; Tools -&gt; Add-ons -&gt; TorButton -&gt; Preferences -&gt; Proxy Settings</pre><pre>check:Use custom proxy settings</pre><pre>HTTP Proxy:127.0.0.1 Port:8118</pre><pre>SSL Proxy:127.0.0.1 Port:8118</pre><pre>SOCKS Host:127.0.0.1 Port:9050</pre><pre>check:SOCKS v5</pre><pre>No Proxies for:127.0.0.1</pre><pre></pre><pre>better use Test Settings button before you up some leaks.</pre><pre>and double check your IP by access http://ipid.shat.net/ before it.</pre><pre></pre><pre><iframe align="left" frameborder="0" marginheight="0" marginwidth="0" scrolling="no" src="http://rcm.amazon.com/e/cm?t=4594025773&amp;o=1&amp;p=8&amp;l=bpl&amp;asins=B002KQ6PEW&amp;fc1=000000&amp;IS2=1&amp;lt1=_blank&amp;m=amazon&amp;lc1=0000FF&amp;bc1=000000&amp;bg1=FFFFFF&amp;f=ifr" style="align: left; height: 245px; padding-right: 10px; padding-top: 5px; width: 131px;"></iframe></pre><div class="blogger-post-footer">/TKNV</div>