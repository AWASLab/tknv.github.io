---
layout: post
title: Brew Tool Suite Perl API
date: '2009-08-19T13:36:00.000+09:00'
author: tknv
comments: true
category:
- ruby
- brew
- windows
- test
- perl
- mobilephone
modified_time: '2009-08-19T13:58:34.256+09:00'
blogger_id: tag:blogger.com,1999:blog-2736766923155041598.post-8444188127697377996
blogger_orig_url: http://yet-another-problem.blogspot.com/2009/08/brew-tool-suite-perl-api.html
---

<blockquote>How to apply original keymap.ini</blockquote><br />オリジナルのキーマップ iniファイルを作成しテストケースを作るにあたって。</br><br />use enumでハマリました。</br><br />--keymap.ini<br /><pre name="code" class="ruby"><br />[KEYMAP]<br />AVK_SELECT=112<br />AVK_UP=134<br />AVK_DOWN=135<br />AVK_LEFT=88<br />AVK_RIGHT=89<br />AVK_SOFT1=34<br />AVK_SOFT2=35<br />AVK_SOFT3=36<br />AVK_SOFT4=36<br />AVK_SEND=69<br />AVK_CLR=70<br /></pre><br />だとしたら、<br />--Testcase.pl<br /><pre name="code" class="ruby"><br />.....<br />use enum qw( AVK_ASTERISK=1000 AVK_NUMBER_SIGN AVK_SELECT AVK_UP AVK_DOWN AVK_LEFT AVK_RIGHT AVK_SEND AVK_CLR AVK_END);<br />.....<br /></pre><br />のように、enumで定義するときに、順序を合わせないと想定通りの挙動をしなかった。</br><br />iniファイルの内容をnameでマッピングするのかと思っていたのが間違え立った模様。<br />足掛け、3日ハマリマシタ。Brewのデベロッパー(正式なアカウントがないと入れない)サイトでもぜんぜんこういう情報がなく。サイトも重いし。アカウント作成すれば入れるフォーラム(無料のサイト)でもあまり情報なしで、perl普段から使ってる人には常識なのかもしれないですが、、、</br><br />慣れていないperlコードだけだと、自動テストをまとめてコントロールするのが大変なんで、rakeでできないかなーと模索中。<br />Autoitも使用するので、rubyで全部コントロールできると楽。</br><br />だれか、Brew Tool Suite Perl APIでなく、RubyにてBTILをSwigした、Brew Tool Suite知ってますか？<br />Does anyone know Brew Tool Suite Ruby API that swiged BITL for ruby?
