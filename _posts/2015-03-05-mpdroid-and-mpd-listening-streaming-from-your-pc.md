---
layout: post
title: "MPDroid and mpd, listening streaming from your PC"
date: Thursday, 05. March 2015 12:04AM
author: tknv
cover:  /images/MPDoid-and-mpd.png
comments: true
tags:
- Android
- MPD
- streaming
description: Listening and control music on Android by MPDroid from your PC streaming.
---
Listening music on Android by [MPDroid-MPD (Music Player Daemon) client](MPDroid) from your PC streaming.  

## Install mpd ##  

```bash
sudo apt-get install mpd mpc lame ncmpcpp  
```

**prepare configs**

```bash
mkdir -p .config/mpd  
touch .config/mpd/log
```
mpd.conf

```config
music_directory		"~/music"
playlist_directory		"~/.config/mpd/playlists"
db_file			"~/.config/mpd/database"
log_file			"~/.config/mpd/log"
pid_file			"~/.config/mpd/pid"
state_file			"~/.config/mpd/state"
sticker_file			"~/.config/mpd/sticker.sql"
bind_to_address		"any"
port				"6600"
metadata_to_use	"artist,album,title,track,name,genre,date,composer,performer,disc"
auto_update	"yes"
auto_update_depth "5"
follow_outside_symlinks	"yes"
follow_inside_symlinks		"yes"
input {
        plugin "curl"
}
audio_output {
	type		"alsa"
	name		"My ALSA Device"
	mixer_type      "software"	# optional
    tags    "yes"
    replay_gain_handler  "software"
#    replay_gain_handler  "mixer"
#	mixer_device	"default"	# optional
#    mixer_control	"PCM"		# optional
#    mixer_control	"MASTER"		# optional
#	mixer_index	"0"		# optional
}
audio_output {
	type		"httpd"
	name		"My HTTP Stream"
#	encoder		"vorbis"		# optional, vorbis or lame
	encoder		"lame"		# optional, vorbis or lame
	port		"8000"
#	bind_to_address	"0.0.0.0"		# optional, IPv4 or IPv6
	quality		"5.0"			# do not define if bitrate is defined
#	bitrate		"128"			# do not define if quality is defined
	format		"44100:16:1"
	max_clients	"0"			# optional 0=no limit
}
```

```bash
mkdir music
cd music
ln -s /YOUR/MUSIC/FOLDER/FILES MY-MUSIC 
mpd .config/mpd/mpd.conf
mpc update
```
When something wrong with streaming.  

```bash
ncmpcpp
# push 8 -> Outputs -> select *** Stream, toggle enable <-> disable
```



















