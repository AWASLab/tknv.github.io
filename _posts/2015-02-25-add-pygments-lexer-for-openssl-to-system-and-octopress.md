---
layout: post
title: "Add pygments lexer for openssl to system and octopress"
date: 2015-02-25 20:53:40 +0700
author: w.tknv
cover: /images/oct-logo.png
comments: true
categories: 
- pygments
- octopress
- openssl
- lexer
- highlight
description: making more nice octopress.
---
##install to system(Debian)  
```
sudo apt-get install python-setuptools
sudo easy_install pygments-openssl
```   
##install to octopress
```  
cd opctopress/plugins
vim  pygments.rb
```
add below.
```diff
lang = 'objc' if lang == 'm'
lang = 'perl' if lang == 'pl'
lang = 'yaml' if lang == 'yml'
+ lang = 'openssl' if lang == 'cnf'
```