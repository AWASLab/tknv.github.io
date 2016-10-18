---
layout: post
title: SJIS and UTF8
date: '2009-09-28T18:13:00.000+09:00'
author: tknv
comments: true
category:
- ruby
- ie
- watir
- test
modified_time: '2009-09-30T14:07:46.384+09:00'
blogger_id: tag:blogger.com,1999:blog-2736766923155041598.post-7371925281481680570
blogger_orig_url: http://yet-another-problem.blogspot.com/2009/09/sjis-and-utf8.html
---

<h1>Stuck in chr encoding!</h1><br /><blockquote>Encode Situation.<br />target site : UTF-8<br />test data   : SJIS(CP932)</blockquote><br /><br />source file encode:sjis<br /><p><br />＃！　ｒｕｂｙ　－ｋｓ<br />~some code~<br />ＷＩＮ３２ＯＬＥ．ｃｏｄｅｐａｇｅ　＝　ＷＩＮ３２ＯＬＥ：：ＣＰ＿ＵＴＦ８　# -> for target site encode<br /># string for ie<br />some_str = 'SJIS str 日本語です。'<br />give_str = some_str.toutf8<br /></p><br />blogger sanitized my code that's why 2byte chr.<br />なぜ、こんなことになってしまったか？<br />読み込むCSVがCP932で書かれていて、これは変更できない制限がある。<br />テストするサイトはUTF8のエンコーディングで、これも変更できない制限がある。<br />で、CSVのデータを読み込むときに-kuだと、化ける。<br />-ksで読み込んで、toutf8でエンコードして、WIN32OLE::CP_UTF8で、<br />ieインスタンスからutf8でもらって$KCODE=UTF8でregexpすることに<br />必要だった。<br />他に良い、方法あるのか模索中
