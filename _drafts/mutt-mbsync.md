---
layout: post
title: mutt and office365 on windows/Linux
date: 2016-12-03 00:03
update_date:
cover: /images/cover-mutt-mbsync.jpg
	width: 4192
	height: 3104
tags:
- mutt
- linux
- putty
- windows
- davmail
- lbdb
- office365
- exchange
- mbsync
- isync
- msmtp
- GPG
- pass
- debian
- notmuch
- Xming
description: Using mutt for office365 account email on windows(linux) putty.
location: local
locale:
---

> My outlook 2016 is too slow to do somthing.
> Also alas port 993 is closed due to ...

## using packages  

- mutt - MUI
- notmuch - search engine
- lbdb - address book
- putty - ssh GUI client
- davmail - service/client to connect office365
- mbsync/isync - mailbox(imap) sync client
- msmtp - sending email
- pass - manage password

#### install all packages

```
apt-get install libswt-gtk-3-jni libswt-gtk-3-java libswt-cairo-gtk-3-jni davmail mbsync pass notmuch notmuch-mutt msmtp mutt-patched lbdb fortune cowsay pinentry-curses
```

## davmail

default config file `.davmail.properties` in user home folder.  

`vim .davmail.properties`

```
...
davmail.url=https://outlook.office365.com/EWS/Exchange.asmx
davmail.enableEws=true
davmail.server=true
davmail.disableGuiNotifications=true
...
```

## mbsync

 `mkdir -p mail/work`

### setting pass

make gpg key at Linux if no gpg key.

#### set gpg-agent

`vim ~/.gnupg/gpg.conf`

```
use-agent
```

`vim ~/.gnupg/gpg-agent.conf`

```
default-cache-ttl 28800
pinentry-program /usr/bin/pinentry-curses
```

#### set gpg module on for zsh ( preztorc )

```
zstyle ':prezto:load' pmodule \
  ...
  'gpg' \
  'prompt'
```

#### initialize pass and set

`pass init <your fingerprint>` - your fingerprint = gpg2 --fingerprint

encrypt password by pass with gpg. `pass insert mail/worg-gpg`. 

### config mbsync

`vim ~/.mbsyncrc`

```
IMAPAccount work
Host localhost
Port 1143
User me@me.com
PassCmd "pass mail/work-gpg"
RequireSSL no

IMAPStore work-remote
Account work

MaildirStore work-local
Path ~/mail/work/
Inbox ~/mail/work/Inbox

Channel work
Master :work-remote:
Slave :work-local:
Patterns "INBOX" "*"
Create Both
Sync All
SyncState *
```

## msmtp

### config msmtp

`vim ~/.msmtprc`

```
account work
host localhost
port 1025
protocol smtp
from me@me.com
user me@me.com
passwordeval pass mail/work-gpg
auth login
```

`chmod 0600 .msmtprc`

## mutt

`mkdir -p .mutt/temp`
`vim ~/.muttrc`

```
# Paths ----------------------------------------------
set folder           = ~/mail               # mailbox location
set alias_file       = ~/.mutt/alias         # where to store aliases
set header_cache     = ~/.mutt/cache/headers # where to store headers
set message_cachedir = ~/.mutt/cache/bodies  # where to store bodies
set certificate_file = ~/.mutt/certificates  # where to store certs
set mailcap_path     = ~/.mutt/mailcap       # entries for filetypes
set tmpdir           = ~/.mutt/temp          # where to keep temp files
set sig_on_top       = yes
set signature        = ~/.mutt/sig           # my signature file
# Account Settings ----------------------------------
set mbox_type    = Maildir
set spoolfile    = "+work/Inbox" #Default inbox.
set mbox         = "+work/Archive"
set postponed    = "+work/Drafts"
set recall      = yes
# Editor Settings -------------------------------------
set editor	     = "vim -c \"set spell spelllang=en\""
# How to use spell checker
# ]s — move to the next mispelled word
# [s — move to the previous mispelled word
# zg — add a word to the dictionary
# zug — undo the addition of a word to the dictionary
# z= — view spelling suggestions for a mispelled word
set trash		     = "+work/Deleted\ Items"
set index_format = "%4C %Z %d %-15.15L (%2e) %s"
set beep_new     = yes
set copy         = yes
set query_command	="lbdbq '%s'" # how to query the exchange ldap database bind editor "\t"
# Mailboxes to show in the sidebar ------------------
mailboxes +work/Inbox \
          +work/Inbox/\.Action \
          +work/Inbox/\.Hold \
          +work/Inbox/\.Done \
          +work/Archive \
          +work/Sent \
		  +work/Junk \
		  +work/Deleted\ Items \
          +work/Drafts
fcc-hook ~A "+work/Archive" # Saves copies of outgoing mail to "Archive" folder
# Notmuch setting ----------------------------------
macro index,Pager <F8> \
"<enter-command>set my_old_pipe_decode=\$pipe_decode my_old_wait_key=\$wait_key nopipe_decode nowait_key<enter>\
<shell-escape>notmuch-mutt -r --prompt search<enter>\
<change-folder-readonly>`echo ${XDG_CACHE_HOME:-$HOME/.cache}/notmuch/mutt/results`<enter>\
<enter-command>set pipe_decode=\$my_old_pipe_decode wait_key=\$my_old_wait_key<enter>" \
      "notmuch: search mail"

# Sidebar Patch --------------------------------------
set sidebar_visible 	= yes
set sidebar_width   	= 30
# Sidebar Navigation ---------------------------------
bind index,pager K  sidebar-prev
bind index,pager J  sidebar-next
bind index,pager L  sidebar-open
# Status Bar -----------------------------------------
set status_chars  = " *%A"
set status_format = "───[ Folder: %f ]───[%r%m messages%?n? (%n new)?%?d? (%d to delete)?%?t? (%t tagged)? ]───%>─%?p?( %p postponed )?───"

# Sending mail ---------------------------------------
set from		="me@me.com"
set realname		="me"
set use_from		="yes"
set envelope_from	="yes"
set sendmail		="/usr/bin/msmtp -a work"

# Header Options -------------------------------------
ignore *                                # ignore all headers
unignore from: to: cc: date: subject:   # show only these
unhdr_order *                           # some distros order things by default
hdr_order from: to: cc: date: subject:  # and in this order

# Set Header -----------------------------------------
set hdrs = yes
set edit_headers = yes
my_hdr X-Priority: 1 (Highest)          # or not
my_hdr X-MSMail-Priority: High          # or not  

# Index View Options ---------------------------------
set date_format = "%m/%d %I:%M"
set sort = reverse-threads
set sort_aux = date-received
set uncollapse_jump                        # don't collapse on an         unread message
set sort_re                                # thread based on regex

# Index Key Bindings ---------------------------------
bind index gg       first-entry
bind index G        last-entry
bind index R        group-reply
bind index <space>  collapse-thread
# toggle flagged messages ----------------------------
macro index .i  "<limit>(~N|~F)<Enter>"  "view new/flag" # .i will show all new/flagged
macro index .a "<limit>~A<Enter>" "view all" # .a will show all again
bind index   \t          next-unread
bind pager   \t          next-unread
bind index  ,\t      previous-unread
bind pager  ,\t      previous-unread
bind pager <Return> next-line
bind index <Return> display-message 

# Sync email ----------------------------------------
macro index O "<shell-escape>mbsync work<enter>" "run mbsync to sync all mail"

# Saner copy/move dialogs ---------------------------
macro index,pager C "<copy-message>?<toggle-mailboxes>" "copy a message to a mailbox"
macro index,pager \em ":set confirmappend=no delete=yes<enter><tag-thread><tag-prefix-cond><save-message>?<toggle-mailboxes>" "move a all thread message to a mailbox"
macro index,Pager \eCA ":set confirmappend=no delete=yes<enter><tag-thread><tag-prefix-cond><save-message>+work/Archive<enter><sync-mailbox>:set confirmappend=yes delete=ask-yes<enter>"  "Archive all in thread"
macro index,Pager \eca ":set confirmappend=no delete=yes<enter><save-message>+work/Archive<enter><sync-mailbox>:set confirmappend=yes delete=ask-yes<enter>" "Archive message"
# Filter by hand --------------------------------------
# move to action
macro index,Pager \eAC ":set confirmappend=no delete=yes<enter><tag-thread><tag-prefix-cond><save-message>+work/Inbox/\.Action<enter><sync-mailbox>:set confirmappend=yes delete=ask-yes<enter>"  "move a all thread message to Action"
macro index,Pager \eac ":set confirmappend=no delete=yes<enter><save-message>+work/Inbox/\.Action<enter><sync-mailbox>:set confirmappend=yes delete=ask-yes<enter>" "move to Action"
# move to hold
macro index,Pager \eHO ":set confirmappend=no delete=yes<enter><tag-thread><tag-prefix-cond><save-message>+work/Inbox/\.Hold<enter><sync-mailbox>:set confirmappend=yes delete=ask-yes<enter>"  "move a all thread message to Hold"
macro index,Pager \eho ":set confirmappend=no delete=yes<enter><save-message>+work/Inbox/\.Hold<enter><sync-mailbox>:set confirmappend=yes delete=ask-yes<enter>" "move to Hold"
# move to done
macro index,Pager \eDO ":set confirmappend=no delete=yes<enter><tag-thread><tag-prefix-cond><save-message>+work/Inbox/\.Done<enter><sync-mailbox>:set confirmappend=yes delete=ask-yes<enter>"  "move a all thread message to Done"
macro index,Pager \edo ":set confirmappend=no delete=yes<enter><save-message>+work/Inbox/\.Done<enter><sync-mailbox>:set confirmappend=yes delete=ask-yes<enter>" "move to Done"
# to view
macro index       -   "<limit>all\n" "show all messages (undo limit)"
macro index       \C_  "<tag-pattern>~N<enter><tag-prefix-cond>N<untag-pattern>~T<enter>" "mark all as read"

# Pager View Options ---------------------------------
set pager_index_lines = 10 # number of index lines to show
set pager_context = 3      # number of context lines to show
set pager_stop             # don't go to next message automatically
set menu_scroll            # scroll in menus
set tilde                  # show tildes like in vim
unset markers              # no ugly plus signs
set quote_regexp = "^( {0,4}[>|:#%]| {0,4}[a-z0-9]+[>|]+)+"

# Pager Key Bindings ---------------------------------
bind pager k  previous-line
bind pager j  next-line
bind pager gg top
bind pager G  bottom
bind pager R  group-reply

# Compose View Options -------------------------------
set realname = "me"                  # who am i?
set envelope_from                    # which from?
set sig_dashes                       # dashes before sig
set askcc                            # ask for CC:
set fcc_attach                       # save attachments with the body
unset mime_forward                   # forward attachments as part of body
set forward_format = "Fwd: %s"       # format of subject when forwarding
set forward_decode                   # decode when forwarding
set reply_to                         # reply to Reply to: field
set reverse_name                     # reply as whomever it was to
set include                          # include message in replies
set forward_quote                    # include message in forwards
set indent_str="> "
set check_new=yes
set timeout=15
set mail_check=5
set crypt_use_gpgme=no

# Automatically open evil HTML ---------------------
auto_view text/html

# Color quoted set
color quoted0   blue        default
color quoted1   green       default
color quoted2   red         default
color quoted3   blue        default
color quoted4   green       default
color quoted5   red         default
color quoted6   blue        default

# Color scheme -------------------------------------
color index      brightyellow  default ~N # New
color attachment magenta          white
color error      brightwhite   red
color hdrdefault brightwhite   magenta
color header     red           white   ^Subject        # except for `Subject:'.
color header     black         red     ^(X-Spam-Status:\ Yes|X-Virus-Report:)
color header     black         red     ^Newsgroups:.*, # cross-posted
color header     black         green   ^Followup-To:   # followup header
mono  header     bold ^(From|Subject):
color indicator  brightwhite   magenta
color markers    brightcyan    default
color message    white         default
color normal     white         default
color search     magenta          default
color signature  magenta         default
color status     brightwhite   magenta
color tilde      brightmagenta default
color tree       brightmagenta default

# Various smilies and the like ---------------------
color body brightgreen black <[Ee]?[Bb]?[Gg]>
color body brightgreen black <[Bb][Gg]>
color body brightgreen black ' [;:]-*[)>(<|]'

# URLs ---------------------------------------------
# color body brightblue black (http|ftp|news|telnet|finger):\/\/([^[:space:]]+)
color body magenta black (http|ftp|news|telnet|finger):\/\/([^[:space:]]+)
#color body brightblue black mailto:[-a-z_0-9.]+@[-a-z_0-9.]+
color body magenta black mailto:[-a-z_0-9.]+@[-a-z_0-9.]+
mono body bold (http|ftp|news|telnet|finger):\/\/([^[:space:]]+)
mono body bold mailto:[-a-z_0-9.]+@[-a-z_0-9.]+

# email addresses ----------------------------------
# color body brightblue black [-a-z_0-9.%$]+@[-a-z_0-9.]+\\.[-a-z][-a-z]+
color body magenta black [-a-z_0-9.%$]+@[-a-z_0-9.]+\\.[-a-z][-a-z]+
mono body bold [-a-z_0-9.%$]+@[-a-z_0-9.]+\\.[-a-z][-a-z]+
```

## lbdb

obtain email address by [nice script: http://serendipity.ruwenzori.net/index.php/2006/02/25/feeding-whole-maildirs-to-lbdb-for-mutts-address-completion](http://serendipity.ruwenzori.net/index.php/2006/02/25/feeding-whole-maildirs-to-lbdb-for-mutts-address-completion)

for two byte, add `-c urf-8`, otherwise garble.
`vim ~/src/Script/lbdb-fetchalladdresses-firsttime.sh`

```sh
#!/bin/bash
#
# lbdb-fetchalladdresses-firsttime
# Release 0.3
# Latest version is always available at <a href="http://www.ruwenzori.net/code/lbdb-fetchalladdresses-firsttime/">http://www.ruwenzori.net/code/lbdb-fetchalladdresses-firsttime/</a>

#
# What :
#
# Fetches all addresses contained in the messages in a maildir
# hierarchy and stores them in the lbdb m_inmail database

#
# Dependancies :
#
# Developped and tested on a Debian 'Testing' system.
# Should work anywhere that has Unix-ish paths, Bash or a bash like shell,
# lbdb and the dependancies thereof.

#
# Author : Jean-Marc Liotier
#

# Changes :
#
# 0.3 - 20060523
# - Unicity is now handled by 'sort' instead of 'uniq'.
# - Better sort criterias (-u -d -f -i -k 1,1).
#
# 0.2 - 20060321
# - Extraneous 'ls' and nested loop removed for faster performance
# - Now works with Maildir names containing spaces
# - Supressed bad root folder path in subfolder loop
# - Does not try to cat directories
#
# 0.1 - 20060222
# - Initial release
#

# License :
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or (at
# your option) any later version.

#############################################
#
# User editable parameters
#
home=/home/me
lbdbdir=$home/.lbdb
maildir=$home/mail/work/Inbox
# excluded_maildirs must be a regular expression understood by 'grep'
# and enclosed in quotes
excluded_maildirs="Junk\|Deleted\ Items"
#
# No user editable parameters below this line
#
##############################################


# Creates the lbdb directory if it does not exists
if [ -d "$home/.lbdb" ]
   then
      sleep 0
   else
      mkdir $home/.lbdb
   fi

chmod 700 $home/.lbdb &


find $maildir/cur | while read zemessage
   do
      if [ -f "$zemessage" ]
         then
            cat "$zemessage" | lbdb-fetchaddr -a -c utf-8
      fi
   done

find $maildir/.*/cur | grep -v "$excluded_maildirs" | sed s/\ /\\"\ "/g | grep -v /./ | while read zemessage
   do
      if [ -f "$zemessage" ]
         then
            cat "$zemessage" | lbdb-fetchaddr -a -c utf-8
      fi
   done

rm -f $lbdbdir/m_inmail.list.tmp

mv $lbdbdir/m_inmail.list $lbdbdir/m_inmail.list.tmp
sort -u -d -f -i -k 1,1 $lbdbdir/m_inmail.list.tmp > $lbdbdir/m_inmail.list

rm -f $lbdbdir/m_inmail.list.tmp
chmod 600 $lbdbdir/m_inmail.list

rm -f $lbdbdir/m_inmail.list.dirty

```

`vim ~/src/Script/lbdb-fetchalladdresses-daily.sh`

```sh
#!/bin/bash
#
# lbdb-fetchalladdresses-daily
# Release 0.3
#
# Latest version is always available at <a href="http://www.ruwenzori.net/code/lbdb-fetchalladdresses-daily/">http://www.ruwenzori.net/code/lbdb-fetchalladdresses-daily/</a>

#
# What :
#
# Fetches all addresses contained in the messages in a maildir
# hierarchy not older than a certain age and stores them in
# the lbdb m_inmail database
#
# If you do not have an existing lbdb database, you should
# run lbdb-fetchalladdresses-firsttime.sh first.
# 
# lbdb-fetchalladdresses-firsttime.sh is available at
# <a href="http://www.ruwenzori.net/code/lbdb-fetchalladdresses-firsttime/">http://www.ruwenzori.net/code/lbdb-fetchalladdresses-firsttime/</a>

#
# Dependancies :
#
# Developped and tested on a Debian 'Testing' system.
# Should work anywhere that has Unix-ish paths, Bash or a bash like shell,
# lbdb and the dependancies thereof.

#
# Author : Jean-Marc Liotier
#

# Changes :
#
# 0.3 - 20060523
# - Unicity is now handled by 'sort' instead of 'uniq'.
# - Better sort criterias (-u -d -f -i -k 1,1).
#
# 0.2 - 20060321
# - Extraneous 'ls' and nested loop removed for faster performance
# - Now works with Maildir names containing spaces
# - Supressed bad root folder path in subfolder loop
# - Does not try to cat directories
#
# 0.1 - 20060222
# - Initial release
#

#
# License :
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or (at
# your option) any later version.

#############################################
#
# User editable parameters
#
home=/home/me
lbdbdir=$home/.lbdb
maildir=$home/mail/extreme/Inbox
# Excluded_maildirs must be a regular expression understood by 'grep'
# and enclosed in quotes
excluded_maildirs="Drafts\|Junk\|Deleted\ Items"
# The value of max_age (the maximum age of the messages parsed)
# is expressed in days
max_age=1
#
# No user editable parameters below this line
#
##############################################


find $maildir/cur -mtime -$max_age | while read zemessage
   do
      if [ -f "$zemessage" ]
         then
            cat "$zemessage" | lbdb-fetchaddr -a -c utf-8
      fi 
   done

find $maildir/.*/cur -mtime -$max_age | grep -v "$excluded_maildirs" | sed s/\ /\\"\ "/g | grep -v /./ | while read zemessage
   do
      if [ -f "$zemessage" ]
         then
            cat "$zemessage" | lbdb-fetchaddr -a -c utf-8
      fi 
   done

rm -f $lbdbdir/m_inmail.list.tmp

mv $lbdbdir/m_inmail.list $lbdbdir/m_inmail.list.tmp
sort -u -d -f -i -k 1,1 $lbdbdir/m_inmail.list.tmp > $lbdbdir/m_inmail.list

rm -f $lbdbdir/m_inmail.list.tmp
chmod 600 $lbdbdir/m_inmail.list

rm -f $lbdbdir/m_inmail.list.dirty

```

## putty

use mutt with davmail at windows.

#### Windows putty setting

Category > Window > Set the size of the window > Columns 180, Rows 55
Category > Window > Controle the scrollback in the window > Lines of scrollback 0
Category > Connection > SSH > X11 > X11 forwarding check Enable X11 forwarding, X display location `localhost:0`. Remote X11 authentication protocol check MIT-Magic-Cookie-1, X authority file for local display `C:\Users\me\Xauthority`

#### Linux screen setting

`vim ~/src/Script/a`

```sh
#!/bin/bash
fortune | cowsay

```

`vim ~/.screenrc`

```
vbell off
bind w windowlist -b
startup_message off
caption always "%{= kw}%-w%{= mw}%n %t%{-}%+w %-= @%H - %LD %d %LM - %c"
termcapinfo xterm ti@:te@
altscreen on

screen -t mutt
select 0
stuff "mutt\n"
screen -t davmail
select 1
stuff "a\n"
# screen -t task
# select 2
# stuff "task\n"
```

## First run

- run Xming on windows.
- run putty on windows.

at putty, login Linux(virtual machine etc.)

```shell
screen
# control + a, then 1
davmail &
# control + a, then 0
notmuch config
# set params. directory is mail
mbsync work
notmuch new
lbdb-fetchalladdresses-firsttime.sh
mutt
```

## Appendix
- connect ldap from mutt: http://www.bsdconsulting.no/tools/
- html email by markdown with mutt: https://dgl.cx/2009/03/html-mail-with-mutt-using-markdown


## Referenced sites

- [Take Control of your Email with Mutt, OfflineIMAP and Notmuch | hobo.house](https://hobo.house/2015/09/09/take-control-of-your-email-with-mutt-offlineimap-notmuch/)
- [Mutt Offline Outlook365 With mbsync](http://www.benfrancom.com/2014/11/24/mutt-offline-with-mbsync.html)

