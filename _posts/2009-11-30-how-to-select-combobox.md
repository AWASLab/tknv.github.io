---
layout: post
title: How to select ComboBox
date: '2009-11-30T16:29:00.000+09:00'
author: tknv
comments: true
category:
- ruby
- win32ole
- autoit
- windows
modified_time: '2009-11-30T16:40:47.287+09:00'
blogger_id: tag:blogger.com,1999:blog-2736766923155041598.post-6847828236295766271
blogger_orig_url: http://yet-another-problem.blogspot.com/2009/11/how-to-select-combobox.html
---

How to select ComboBox by AutoIt.exe from ruby<br />Give parameter to select combobox exe.<br /><br /><pre name='code' class='ruby'><br />### select_year_combobox.au3 ###<br />WinActivate("Configuration")<br />Global Const $CB_SETCURSEL = 0x14E<br />$h_combobox = ControlGetHandle("Configuration", "", "[CLASS:WindowsForms10.COMBOBOX.app.0.378734a; INSTANCE:2]")<br />$i_index = $CMDLINE[1]<br />DllCall("user32.dll", "int", "SendMessage", "hwnd", $h_combobox, "int", $CB_SETCURSEL, "int", $i_index, "int", 0)<br /></pre><br /><br />ControlGetHandle("Configuration", "", "[CLASS:WindowsForms10.COMBOBOX.app.0.378734a; INSTANCE:2]")<br />ControlGetHandle(＜app title＞, "", ＜select combobox instance＞).<br />＜select combobox instance＞ is easy to know by AutoIt Window Info app.<br /><br /><pre name='code' class='ruby'><br />### select_combobox.rb ###<br />select_year_exe = File.expand_path(File.dirname(__FILE__) + "/select_year_combobox.exe")<br />year = 1 # ex. index 1 = 2005<br />system("#{select_year_exe} #{year}")<br /></pre><br /><br />>ruby select_combobox.rb # app will choose index 1 from combobox.<br />>select_year_combobox.exe 1 # app will choose index 1 from combobox.
