---
layout: post
title: "AMAZON Kindle - daily news! send to kindle"
date: 2015-03-08 00:31:33 +0700
author: tknv
cover: /images/deliver-to-kindle/zews.png
comments: true
tags: 
- kindle
- amazon
- script
- google
- news
description: delivery daily news, send to kindle device.
---
## Make gmail account and subscribe for news letter  
From here, [create google script](https://script.google.com/)   
![create-script](/images/deliver-to-kindle/cerate-script.png)     
select **Gmail**.    
![script-view](/images/deliver-to-kindle/script-view.png)   
copy below code and paste above.   
sendToKindle.js  

```javascript  
var subjectToSend = "LikeADailyToKindle";
// send to kindle then move to File label
// part of anxious subjects
var targetSubject = new Array();
targetSubject[1] = "[  Daily green farmer";
targetSubject[2] = 'International bean market';
targetSubject[3] = '■ Start Now! Sports and Linux';
// even more
// targetSubject[i++] = 'Default FX market news diagnosis';
// Your kindle address
var kindleMailAddr = "your-kindle@kindle.com";
// check INBOX
function checkInbox() {
    // to check as the number of anxious subjects
    for (var i =1;i<targetSubject.length; i++) {
        var sanitizedTargetSub = targetSubject[i].replace(/[-[\]{}()*+?.,\\^$|#\s]/g, "\\$&");
        // check FRESH message in INBOX, possible moer than 50, but can be time out
        for (var j = 1;j<51; j++) {
        var threads = GmailApp.getInboxThreads();
        try {
          var subject = threads[j].getFirstMessageSubject();
        }
        catch(e) {
          break;  
        }
          // to RegExp match as the number of anxious subjects
          if (subject.match(new RegExp(sanitizedTargetSub))) {
          // obtain message in thread and strip some useless.
          var message = threads[j].getMessages()[0];
          var bodyStr = message.getBody();
          var topLess = bodyStr.replace(/<br\ \/>|&#x3000\;|&nbsp\;/gm, '');
          var title = targetSubject[i] + message.getDate() + ".html"
          var bodyDocHtml = DocsList.createFile(title, topLess, "text/html");
          var news = message.getDate() + ".zip"
          // packing!
          var zip = Utilities.zip([bodyDocHtml], news);
          // send to kindle!
          MailApp.sendEmail(kindleMailAddr, subjectToSend, 'see attachment', {attachments:zip});
          // labeling to message and even two messages, but send one by one. so it is morning and evening editions
		            var labelFile = GmailApp.getUserLabelByName("File");
          labelFile.addToThread(threads[j]);
          threads[j].moveToArchive();
          break;
          }
        }
      }
};
```
click clock button and make trigeer timing.  
![clock](/images/deliver-to-kindle/make-schedule.png)    
click *Click here to add one now.*    
![make-trigger](/images/deliver-to-kindle/make-triggers.png)     
assign trigger timing.    
![assign-timing](/images/deliver-to-kindle/make-event-timing.png)     


 
