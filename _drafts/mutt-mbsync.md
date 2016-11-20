---
layout: post
title: mutt-mbsync
date: 2016-11-20 00:03
update_date:
cover: /images/cover-mutt-mbsync.jpg
	width:
	height:
tags:
description:
location: local
locale:
---
davmail

https://outlook.office365.com/EWS/Exchange.asmx

davmail.url=https://outlook.office365.com/EWS/Exchange.asmx
davmail.enableEws=true

Setting up libswt-gtk-3-jni (3.8.2-3) ...
Setting up libswt-gtk-3-java (3.8.2-3) ...
Setting up libswt-cairo-gtk-3-jni (3.8.2-3) ...

mkdir mail/work
mkdir mail/temp


chmod 0600 .msmtprc 
.msmtprc
account work
host localhost
port 1025
protocol smtp
from me@me.com
user me@me.com
passwordeval pass mail/work-gpg
auth login

.mbsyncrc
IMAPAccount work
Host localhost
Port 1143
User me@me.com
PassCmd "pass mail/work-gpg"
RequireSSL no
CertificateFile /etc/ssl/certs/ca-certificates.crt

IMAPStore work-remote
Account work

MaildirStore work-local
Path ~/mail/work/
Inbox ~/mail/work/Inbox

Channel work
Master :work-remote:
Slave :work-local:
# Include everything
Patterns "INBOX" "*"
# Automatically create missing mailboxes, both locally and on the server
Create Both
Sync All
# Expunge Both
# Save the synchronization state files in the relevant directory
SyncState *

.muttrc
# vim: set filetype=muttrc:
# Paths ----------------------------------------------
set folder           = ~/mail               # mailbox location
set alias_file       = ~/.mutt/alias         # where to store aliases
set header_cache     = ~/.mutt/cache/headers # where to store headers
set message_cachedir = ~/.mutt/cache/bodies  # where to store bodies
set certificate_file = ~/.mutt/certificates  # where to store certs
set mailcap_path     = ~/.mutt/mailcap       # entries for filetypes
set tmpdir           = ~/.mutt/temp          # where to keep temp files
set signature        = ~/.mutt/sig           # my signature file
# Account Settings ----------------------------------
set mbox_type    = Maildir
set spoolfile    = "+work/Inbox" #Default inbox.
set mbox         = "+work/Archive"
set postponed    = "+work/Drafts"
set editor		   = "vim -c \"set spell spelllang=en\""
# How to use spell checker
# ]s — move to the next mispelled word
# [s — move to the previous mispelled word
# zg — add a word to the dictionary
# zug — undo the addition of a word to the dictionary
# z= — view spelling suggestions for a mispelled word
set trash		     = "+work/Deleted\ Items"
set index_format = "%4C %Z %D %-15.15L (%4l) %s"
set beep_new     = yes
set copy         = yes
set sort         = "threads"
set query_command	="lbdbq '%s'"# how to query the exchange ldap database bind editor "\t" complete-query #tab completion over ldap
# Mailboxes to show in the sidebar.
mailboxes +work/Inbox \
          +work/Inbox/\.Action \
          +work/Inbox/\.Hold \
          +work/Inbox/\.Done \
          +work/Conversation\ History \
          +work/Inbox/\.HR \
          +work/Inbox/\.HR\.training \
          +work/Inbox/\.HR\.transition \
          +work/Inbox/\.HR\.performance \
          +work/Inbox/\.IT-issue \
          +work/Inbox/\.ProductInfo \
          +work/Archive \
          +work/Sent\ Items \
          +work/Deleted\ Items \
          +work/Drafts
bind index 		"\Ca" 'imap-fetch-mail'
fcc-hook ~A "+work/Archive" # Saves copies of outgoing mail to "Archive" folder
# Notmuch setting ----------------------------------

macro index <F8> \
"<enter-command>set my_old_pipe_decode=\$pipe_decode my_old_wait_key=\$wait_key nopipe_decode nowait_key<enter>\
<shell-escape>notmuch-mutt -r --prompt search<enter>\
<change-folder-readonly>`echo ${XDG_CACHE_HOME:-$HOME/.cache}/notmuch/mutt/results`<enter>\
<enter-command>set pipe_decode=\$my_old_pipe_decode wait_key=\$my_old_wait_key<enter>" \
      "notmuch: search mail"

macro index <F9> \
"<enter-command>set my_old_pipe_decode=\$pipe_decode my_old_wait_key=\$wait_key nopipe_decode nowait_key<enter>\
<pipe-message>notmuch-mutt -r thread<enter>\
<change-folder-readonly>`echo ${XDG_CACHE_HOME:-$HOME/.cache}/notmuch/mutt/results`<enter>\
<enter-command>set pipe_decode=\$my_old_pipe_decode wait_key=\$my_old_wait_key<enter>" \
      "notmuch: reconstruct thread"

macro index <F6> \
"<enter-command>set my_old_pipe_decode=\$pipe_decode my_old_wait_key=\$wait_key nopipe_decode nowait_key<enter>\
<pipe-message>notmuch-mutt tag -- -inbox<enter>\
<enter-command>set pipe_decode=\$my_old_pipe_decode wait_key=\$my_old_wait_key<enter>" \
      "notmuch: remove message from inbox"

# Sidebar Patch --------------------------------------
# set sidebar_delim   	= '  │'
# color sidebar_new color221 color233
set sidebar_visible 	= yes
set sidebar_width   	= 30
# Bind sidebar navigation to CTRL-n, CTRL-p, and CTRL-o
bind index \CP sidebar-prev
bind index \CN sidebar-next
bind index \CO sidebar-open
bind pager \CP sidebar-prev
bind pager \CN sidebar-next
bind pager \CO sidebar-open
#Status Bar
set status_chars  = " *%A"
set status_format = "───[ Folder: %f ]───[%r%m messages%?n? (%n new)?%?d? (%d to delete)?%?t? (%t tagged)? ]───%>─%?p?( %p postponed )?───"

# Sending mail ---------------------------------------
set from		="me@me.com"
set realname		="Takanobu Watanabe"
set use_from		="yes"
set envelope_from	="yes"
set sendmail		="/usr/bin/msmtp"

# Header Options -------------------------------------
ignore *                                # ignore all headers
unignore from: to: cc: date: subject:   # show only these
unhdr_order *                           # some distros order things by default
hdr_order from: to: cc: date: subject:  # and in this order

# Index View Options ---------------------------------
set date_format = "%m/%d %I:%M"
set sort = threads                         # like gmail
set sort_aux = reverse-last-date-received  # like gmail
set uncollapse_jump                        # don't collapse on an         unread message
set sort_re                                # thread based on regex
#set reply_regexp = "^(([Rr][Ee]?(\[[0-9]+\])?: *)?(\[[^]]+\] *)?)*"

# Index Key Bindings ---------------------------------
bind index gg       first-entry
bind index G        last-entry
bind index R        group-reply
bind index <space>  collapse-thread
# toggle flagged messages
macro index .i  "<limit>(~N|~F)<Enter>"  "view new/flag" # .i will show all new/flagged
macro index .a "<limit>~A<Enter>" "view all" # .a will show all again
bind index \t next-unread
bind pager   \t          next-unread
bind index  ,\t      previous-unread
bind pager  ,\t      previous-unread

# Sync email
macro index O "<shell-escape>mbsync work<enter>" "run mbsync to sync all mail"
macro index o "<shell-escape>mbsync work<enter>" "run mbsync to sync inbox"

# Saner copy/move dialogs
macro index C "<copy-message>?<toggle-mailboxes>" "copy a message to a mailbox"
macro index M "<save-message>?<toggle-mailboxes>" "move a message to a mailbox"
macro index,Pager A   "<tag-thread><tag-prefix-cond><save-message>+work/Archive<enter>"  "Archive all in thread"
macro index,Pager a   "<save-message>+work/Archive<enter>"                               "Archive message"
macro index       -   "<limit>all\n"                                                     "show all messages (undo limit)"
macro index       \C_ "<tag-pattern>~N<enter><tag-prefix-cond>N<untag-pattern>~T<enter>" "mark all as read"

# Sidebar Navigation ---------------------------------
bind index,Pager <down>   sidebar-next
bind index,Pager <up>     sidebar-prev
bind index,Pager <right>  sidebar-open

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
set realname = "Takanobu Watanabe"   # who am i?
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
color attachment blue          white
color error      brightwhite   red
color hdrdefault brightwhite   blue
color header     red           white   ^Subject        # except for `Subject:'.
color header     black         red     ^(X-Spam-Status:\ Yes|X-Virus-Report:)
color header     black         red     ^Newsgroups:.*, # cross-posted
color header     black         green   ^Followup-To:   # followup header
mono  header     bold ^(From|Subject):
color indicator  brightwhite   blue
color markers    brightcyan    default
color message    white         default
color normal     white         default
color search     blue          default
color signature  green         default
color status     brightwhite   blue
color tilde      brightmagenta default
color tree       brightmagenta default

# Various smilies and the like ---------------------
color body brightgreen black <[Ee]?[Bb]?[Gg]>
color body brightgreen black <[Bb][Gg]>
color body brightgreen black ' [;:]-*[)>(<|]'

# URLs ---------------------------------------------
color body brightblue black (http|ftp|news|telnet|finger):\/\/([^[:space:]]+)
color body brightblue black mailto:[-a-z_0-9.]+@[-a-z_0-9.]+
mono body bold (http|ftp|news|telnet|finger):\/\/([^[:space:]]+)
mono body bold mailto:[-a-z_0-9.]+@[-a-z_0-9.]+

# email addresses ----------------------------------
color body brightblue black [-a-z_0-9.%$]+@[-a-z_0-9.]+\\.[-a-z][-a-z]+
mono body bold [-a-z_0-9.%$]+@[-a-z_0-9.]+\\.[-a-z][-a-z]+
