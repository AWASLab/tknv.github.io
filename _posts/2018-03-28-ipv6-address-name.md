---
layout: post
title: IPv6 address name
date: 2018-03-28 15:47
update_date:
cover: /images/cover-IPv6-address-name.jpg
width: 3120
height: 4160
tags: 
- ethernet 
- ipv6 
- subnet 
- naming 
- design 
- diagram 
- python 
- regex 
- argv 
description: Assign IPv6 network address is bit faff.  
location: in electric signal  
locale: en_GB
---
# It's so faff to think about address

We can use only a to f, 0 to 9 and four letters combination. And also better add meanings to remember why assign this IPv6 address. I usually am using **beef** and **cafe** to type easy. Alas poor vocabulary.  

Here is usable word list for IPv6 address with meaning(maybe, I did not check it all).

```
Abba
Adad
Adda
Caca
Dada
Dade
EBCD
Ecca
Edda
Faba
abac
abba
abbe
abed
acca
aced
adad
adda
affa
baba
babe
bade
baff
bead
bede
beef
caba
cade
cafe
ceca
cede
dabb
dace
dada
dade
daff
dead
deaf
deed
ebcd
ecad
edda
edea
face
fade
faff
feed
``` 

It's inspired by [Alec's Web Log](http://www.alecjacobson.com/weblog/?p=475)

## code

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import re
import sys
from io import open

context = sys.argv[1]
fha = re.compile(r"^[a-fA-F0-9]{4}\n")
with open(context, encoding='utf8', errors='ignore') as lines:
  for line in lines:
    if fha.search(line):
      print(line),
```


### Execute

`foo> python this-program.py words-file`

[words-file](http://www.alecjacobson.com/weblog/media/list-of-english-words.txt)	

