---
layout: post
title: dig log file
date: 2016-10-26 15:15
update_date: 2016-10-26 22:22
cover: /images/cover-dig-log-file.jpg
tags:
- log
- shell
- grep
- script
- tar
description: dig many log files by using grep and shell script 
location: local
locale:

---

## Extract many tarballs

some tarballs.

```shell
$> ls -1
logdump-host-a01
logdump-host-a02
logdump-host-a03
logdump-host-b01
logdump-host-c01
logdump-host-c02
```

make some directory for extracting.

```shell
$> mkdir all_logs
```

```shell
$> ls -1
all_logs
logdump-host-a01
logdump-host-a02
logdump-host-a03
logdump-host-b01
logdump-host-c01
logdump-host-c02
```

make some script to make directory and extract in all_logs directory. maybe the script name is extract.sh.

```bash
#!/bin/sh
ls -1 ../ >> list
while read f
do
  if [ "$f" != "all_logs" ]
	then
	mkdir files-$f
	cd files-$f
	tar -xzvf ../../$f
    cd ../
  fi
done < list
```

add execute permission.  

```shell
$> cd all_logs
$> ls
extract.sh
$> chmod +x extract.sh
```

execute.

```shell
$> ./extract.sh
```

## grep logs

```shell
$> grep -Ir -A 5 -B 3 --include=event.log -w 'not\ fun,\ but\ addict$'
```

recursively search and ignore binary files, search event.log file name, result show before 3 lines and after 5 lines from match line. search *something not fun, but addict*, match string should end at **addict**.

`--include='*.log'` search *any*.log files.

