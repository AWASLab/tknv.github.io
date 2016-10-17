---
layout: post
title: Bitbake how to rebuild easy
date: '2010-01-13T03:25:00.002+09:00'
author: tknv
comments: true
category:
- arm
- omap
- linux
- gumstix
- embedded
- bitbake
modified_time: '2010-01-13T03:44:25.889+09:00'
blogger_id: tag:blogger.com,1999:blog-2736766923155041598.post-1202732610363005946
blogger_orig_url: http://yet-another-problem.blogspot.com/2010/01/bitbake-how-to-rebuild-easy.html
---

<h2>My proposal is TAKE SNAP SHOT.</h2><br />When change foo.config for configure. <br />Bitbake does not rebuild include that reconfigure pkg.<br />For example, When add some recipes to ~/images/omap3-console-image.bb.<br />1st build,that added recipes include for deploy images.<br /><span style="font-weight:bold;"><h1>But</h1></span>,when need reconfigure again for that. 2nd times,never include what you want.(from my experience).<br />It is sure rebuild by rm -rf tmp, But it will make again soooooooooooooo long building time.<br />My Idea is use VM.(many people use it,when developing)<br />It is best use-case that TAKE SNAP SHOT since VM take snap shot history ever(maybe), Just build simple omap3-console-image then Take snap shot and after that easy to add recipes,reconfigure,rebuild... also faster.
