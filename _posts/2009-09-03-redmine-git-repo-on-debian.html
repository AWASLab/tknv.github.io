---
layout: post
title: redmine & git repo on debian
date: '2009-09-03T16:16:00.001+09:00'
author: tknv
comments: true
category:
- ruby
- redmine
- linux
- debian
- git
- gitosis
- apache
modified_time: '2010-08-10T20:17:19.039+09:00'
blogger_id: tag:blogger.com,1999:blog-2736766923155041598.post-962526293519404600
blogger_orig_url: http://yet-another-problem.blogspot.com/2009/09/redmine-git-repo-on-debian.html
---

<blockquote>1.setup redmine</blockquote><br />普通にrubyの環境があって、passengerが入ってて、redmineが/var/wwwの下に展開されています。<br />サーバのホスト名:base08。<br />セットアップしているマシンのユーザがtknv。<br /><pre name='code' class='ruby'>#/etc/apache2/httpd.conf<br />LoadModule passenger_module /home/tknv/.gem/ruby/1.8/gems/passenger-2.2.4/ext/apache2/mod_passenger.so<br />PassengerRoot /home/tknv/.gem/ruby/1.8/gems/passenger-2.2.4<br />PassengerRuby /usr/bin/ruby1.8<br /><br />PassengerDefaultUser www<br /><virtualhost *:80><br />ServerName base08<br />DocumentRoot /var/www/redmine/public<br /></VirtualHost><br /></pre><br />で、redmineで使用するdbの準備が完了していれば、http://base08でredmineが表示されるはず。<br />表示されないようだったら<a href="http://redmine.jp/install/">サイトで確認</a>。<br />サーバで/var/log/apache2のログを見てもおかしいところがなければ、redmine/tmpのパーミッションが強すぎだったり、666では起動する。または、/etc/apache2/.htaccessファイルがあっていないとか、、、<br /><blockquote>2.git リポジトリーを立てる。</blockquote><br /><a href="http://scie.nti.st/2007/11/14/hosting-git-repositories-the-easy-and-secure-way">このサイトが参考になります。</a><br />概要:ここで作成したgitユーザがこのbase08ホストで、gitのリポジトリーを立てたり、アップデートしたりします。<br />注意は説明で行われている、gitリポジトリーを使用するユーザ、gitユーザもbase08ホストで使用しますが、通常業務(趣味ではやらない)で使用するほうで、gitosisの設定を行います。<br />下記ユーザのマシンでの設定<br /><pre name='code' class='ruby'>#~/gitosis-admin/gitosis.conf<br />[gitosis]<br /><br />[group gitosis-admin]<br />writable = gitosis-admin<br />members = aaa@TKNV bbb@bbc usr1@base08 root@base08<br /><br />[group dev-Ateam]<br />writable = project1 project2 project-foo project-wow<br />members = aaa@TKNV tuka@REP USER1@thinkpad-x300 usr1@base08 root@base08 git@base08<br /><br /># git@base08が今回の肝です。<br /><br />[repo project1]<br />gitweb = yes<br />owner = tknv@amateras<br />description = module for UI and support Ajax<br /></pre><br /><blockquote>大事なのはgit@base08</blockquote>アカウントをredmineで登録したいリポジトリーのグループに入れておくこと。<br />あとは、リンクにあるように、/gitosis-admin/keydirに、git@base.pubのように、sshの公開鍵を入れて、gitでサーバにプッシュ。必要なユーザ分、全部おこなう。<br /><blockquote>3.サーバ上でどうやって、gitリポジトリーを作成しpushのたびに更新するか。</blockquote><br /><a href="http://petersteinberger.com/2009/04/redmine-automatic-synchronization-of-your-git-repo/">ここ参考になります。</a>しかしながら、ネットにある大抵のHowToは簡単には最近、うまくゆかない。こちらのスキルレベルもあるが、こんだけユーザが多いと、ゴミ情報がたくさん、また、最近は新しいこうゆうテクノロジーが出てくると、とにかくやってみて(やればいいのだが、未検証もあり)サイトにアップする人が多く、間違えもそのままコピペしていたり、、、で<br /><pre name='code' class='ruby'>#~/home/git/repositories/<your-repo>/hooks/post-update<br />export GIT_DIR=/home/git/checkout/<your-repo>/.git<br /><br />pushd /home/git/checkout/<your-repo> > /dev/null<br />git reset --hard<br />git pull<br />popd > /dev/null<br /><br />WORKDIR="/home/git/checkout/<your-repo>"<br /><br />/usr/bin/git-update-server-info<br /><br />unset GIT_DIR<br /></pre><br />で、先のサイトにもあるが、Don’t forget to create a _local git copy_ (via git clone git@localhost:<yourrepo>.git in /home/git/checkout (or whatever folder you prefer)<br />しかし、このままでも、まだ、無理で、もう2つやることが必要で、まずは、<br /><s><h2><br />1.先の今回の肝、git@base08がこの、cloneした、ソースのユーザになっている必要がある。<br />2.<a href="http://www.atmarkit.co.jp/flinux/rensai/linuxtips/447nonpassh.html">ssh-agent</a>でこのgit@base08ユーザの秘密鍵の方を登録して、パスワード認証を無しにする必要がある。<br /></h2><br />チェックリスト<br /><ul><li>git@~ ユーザがredmineでブラウジングしたいgitリポジトリーのアクセス許可グループに入っているか？(gitosis.conf)</li> <li>git@~ ユーザのsshの秘密鍵がサーバ(redmineとgitリポジトリのある)にてssh-agentで追加され、パスワード認証が不要になっているか？</li> <li>サーバ(redmineとgitリポジトリのある)の/home/git/checkout/<your-repo>のユーザがgit@~ であるか？<br /></li> </ul></s><br />勘違いでした。<br />git clone <repo-directory>で問題ない、、、<br /><h2>How to create repo from remote(not at server)</h2><script src="http://gist.github.com/517100.js"> </script>
