---
layout: post
title: How to down load file.
date: '2009-06-10T08:48:00.000Z'
author: tknv
tags:
- ruby
- test
- watir
modified_time: '2010-08-28T16:06:02.647Z'
blogger_id: tag:blogger.com,1999:blog-11459759.post-2224049067859822296
blogger_orig_url: http://phichyudebow.blogspot.com/2009/06/how-to-down-load-file.html
---

<pre name="code" class="ruby"><br />#!/usr/bin/env ruby<br /><br />require 'test/unit'<br />require 'rubygems'<br />require 'watir'<br />require 'win32ole'<br /><br /><br /><br />class FileUpLoadTest < Test::Unit::TestCase<br /> include Watir<br /><br />    def test_fileup<br />     ie = Watir::IE.new<br />     ie.goto('foo.com/fileup.html')<br />     assert(ie.file_field(:name,"register_data").exists?,message="error none file field")<br /><br />#Start down load<br />     ie.file_field(:id,'register_data').click_no_wait   <br /># should click_no_wait,otherwise wait forever behind modal window. <br /> <br />     windwName_dwn = "ファイルのダウンロード"<br />     windwName_sve = "名前を付けて保存"<br />     autoit=WIN32OLE.new("AutoItX3.Control")<br />     autoit.WinWait(windwName_dwn)<br />     autoit.WinActivate(windwName_dwn)<br />     autoit.ControlClick(windwName_dwn,"","[CLASS:Button;INSTANCE:2]")<br />     pathAndName = "var/foo.txt"<br />     autoit.WinWait(windwName_sv)<br />     autoit.WinActivate(windwName_sv)<br />     autoit.ControlSetText(windwName_sv, "", 1148, "#{pathAndName}")<br />     autoit.ControlClick(windwName_sv,"","[CLASS:Button;INSTANCE:2]")<br />     autoit.WinWait("ダウンロードの完了")<br />  end<br />end<br /></pre><div class="blogger-post-footer">/TKNV</div>