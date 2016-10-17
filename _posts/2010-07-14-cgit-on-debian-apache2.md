---
layout: post
title: cgit - on Debian Apache2
date: '2010-07-14T18:16:00.000Z'
author: tknv
tags:
- gitosis
- c
- cgit
- git
modified_time: '2010-08-28T16:06:01.966Z'
blogger_id: tag:blogger.com,1999:blog-11459759.post-7491066333318609062
blogger_orig_url: http://phichyudebow.blogspot.com/2010/07/cgit-on-debian-apache2.html
---

<h1>cgit - a very fast web frontend for git repositories</h1><h2>installation to Apache2 on debian lenny</h2><ul><li>Setup git server by gitosys. ref.-<a href="http://yet-another-problem.blogspot.com/2009/09/redmine-git-repo-on-debian.html">link1(lang_JP)</a>,<a href="http://scie.nti.st/2007/11/14/hosting-git-repositories-the-easy-and-secure-way">link2(lang_EN)</a></li><li>add permission by chomod 755 to git repo for cgit exec .git.(Maybe chmod 755 -R /home/git/repositories/)</li><li>build & install cgit.(I had error curl for getting git and extract tar.bz2 at make get-git.)</li><li>config file:/etc/cgitrc</li><pre name='code' class='ruby'>virtual-root=/cgi-bin/<br /><br /># Enable caching of up to 1000 output entriess<br />cache-size=1000<br /><br /># Show extra links for each repository on the index page<br />enable-index-links=1<br /><br /># Show number of affected files per commit on the log pages<br />enable-log-filecount=1<br /><br /># Show number of added/removed lines per commit on the log pages<br />enable-log-linecount=1<br /><br /># Enable statistics per week, month and quarter<br />max-stats=quarter<br /><br /># Set the title and heading of the repository index page<br />root-title=tknv git repositories<br /><br /># Set a subheading for the repository index page<br />root-desc=tracking the tknv development<br /><br /># Include some more info about foobar.com on the index page<br />root-readme=/var/www/htdocs/about.html<br /><br /># Allow download of tar.gz, tar.bz2 and zip-files<br />snapshots=tar.gz tar.bz2 zip<br /><br />##<br />## List of common mimetypes<br />##<br /><br />mimetype.gif=image/gif<br />mimetype.html=text/html<br />mimetype.jpg=image/jpeg<br />mimetype.jpeg=image/jpeg<br />mimetype.pdf=application/pdf<br />mimetype.png=image/png<br />mimetype.svg=image/svg+xml<br /><br /><br />##<br />## List of repositories.<br />## PS: Any repositories listed when section is unset will not be<br />##     displayed under a section heading<br />## PPS: This list could be kept in a different file (e.g. '/etc/cgitrepos')<br />##      and included like this:<br />##        include=/etc/cgitrepos<br />##<br /><br /><br />repo.url=SuperEngine<br />repo.path=/home/git/repositories/SuperEngine.git<br />repo.desc=the SUperEngine repository<br />repo.owner=foo@bar.com<br /><br />repo.url=OSGi-module<br />repo.path=/home/git/repositories/OSGi-module.git<br />repo.desc=the OSGi-module repository<br />repo.owner=foo@bar.com<br /></pre><li>config file:/etc/apache2/httpd.conf</li><pre name='code' class='ruby'><virtualhost *><br /> ServerName halryporya.co.foo<br /> DocumentRoot /var/www/htdocs/cgit/<br /> ScriptAlias /cgi-bin/ "/var/www/htdocs/cgit/cgit.cgi/"<br /> <directory "/var/www/htdocs/cgit/"><br />  AllowOverride None<br />  Options ExecCGI<br />  Order allow,deny<br />  Allow from all<br /> </Directory><br /> Alias /cgit.png /var/www/htdocs/cgit/cgit.png<br /> Alias /cgit.css /var/www/htdocs/cgit/cgit.css<br /> Alias / /var/www/htdocs/cgit/cgit.cgi/<br /></VirtualHost><br /></pre><li>check by w3m http://localhost/cgi-bin/</li></ul><h1>http://halryporya.co.foo/cgi-bin/<br /></h1><p>by browser</P><blockquote>watching CGIT site...</blockquote><div class="blogger-post-footer">/TKNV</div>