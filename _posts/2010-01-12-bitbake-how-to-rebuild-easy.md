---
layout: post
title: Bitbake how to rebuild easy
date: '2010-01-12T18:25:00.000Z'
author: tknv
tags:
- bitbake
- arm
- gumstix
- linux
- embedded
- omap
modified_time: '2010-08-28T16:06:02.047Z'
blogger_id: tag:blogger.com,1999:blog-11459759.post-3650370970286667404
blogger_orig_url: http://phichyudebow.blogspot.com/2010/01/bitbake-how-to-rebuild-easy.html
---

<h2>My proposal is TAKE SNAP SHOT.</h2><br />When change foo.config for configure. <br />Bitbake does not rebuild include that reconfigure pkg.<br />For example, When add some recipes to ~/images/omap3-console-image.bb.<br />1st build,that added recipes include for deploy images.<br /><span style="font-weight:bold;"><h1>But</h1></span>,when need reconfigure again for that. 2nd times,never include what you want.(from my experience).<br />It is sure rebuild by rm -rf tmp, But it will make again soooooooooooooo long building time.<br />My Idea is use VM.(many people use it,when developing)<br />It is best use-case that TAKE SNAP SHOT since VM take snap shot history ever(maybe), Just build simple omap3-console-image then Take snap shot and after that easy to add recipes,reconfigure,rebuild... also faster.<div class="blogger-post-footer">/TKNV</div>