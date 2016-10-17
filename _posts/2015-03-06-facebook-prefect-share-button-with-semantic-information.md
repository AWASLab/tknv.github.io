---
layout: post
title: "FACEBOOK - Prefect share button with semantic information."
date: 2015-03-06 00:04:34 +0900
author: tknv
cover:  /images/facebook-btn/cover.png
comments: true
categories: 
- Facebook
- Web
- SEO
- semantic
- share button
description: making looks nice share contents for share button.
---
When share ![share button](/images/facebook-btn/share-btn.png) , going to see ![share image](/images/facebook-btn/sharing-sample.png)   
Just add meta tags in html file, like below.  

## Obtain facebook app_id  
[Get facebook app id](https://developers.facebook.com/).  
In menu bar My Apps -> Add a New App -> Website -> ...

## meta contents, it is good for SEO too.  

```html
<head>
...
  <meta property="og:url" content="http://YOUR SITE URL/" />
  <meta property="og:type" content="website" />
  <meta property="og:title" content="YOUT SITE TITLE" />
  <meta property="og:description" content="YOUT SITE DESCRIPTION." />
  <meta property="og:image" content="http://YOUT SITE IMAGE URL/IMAGE.png" />
  <meta property="fb:app_id" content="YOUR facebook app id" />
...
</head>
```

## Share button  

```html
<div id="fb-root"></div>
<script>
  window.fbAsyncInit = function() {
    FB.init({
      appId      : 'YOUR facebook app id',
      xfbml      : true,
      version    : 'v2.2'
    });
  };

  (function(d, s, id){
     var js, fjs = d.getElementsByTagName(s)[0];
     if (d.getElementById(id)) {return;}
     js = d.createElement(s); js.id = id;
     js.src = "//connect.facebook.net/en_US/sdk.js";
     fjs.parentNode.insertBefore(js, fjs);
   }(document, 'script', 'facebook-jssdk'));
</script>
```