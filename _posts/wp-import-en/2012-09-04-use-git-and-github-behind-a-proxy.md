---
layout: post
title: "Use Git and GitHub behind a Proxy"
date: 2012-09-04 10:35
author: CI Team
comments: true
categories: [Uncategorized]
tags: []
language: en
---
{% include JB/setup %}
<p><i>Long time no see. Excuse us for this long brake. It’s because Robert moved to Switzerland while Antje moved to another city as well for her studying. There are still a lot thinks left to do but we hope that we could come back to post on a regular base soon. </i></p>  
  
  

<p>If you are situated behind a Proxy you might get connection problems while using GitHub. That’s what the problems looked like for me:</p>
<p><img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; border-top: 0px; border-right: 0px; padding-top: 0px" title="clip_image001" border="0" alt="clip_image001" src="{{BASE_PATH}}/assets/wp-images-de/clip_image001_thumb.jpg" width="585" height="334" /></p>
<p>The GitHub Client always stops at 9% before it quits complete: </p>
<p><img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; border-top: 0px; border-right: 0px; padding-top: 0px" title="clip_image001[5]" border="0" alt="clip_image001[5]" src="{{BASE_PATH}}/assets/wp-images-de/clip_image0015_thumb.jpg" width="589" height="232" /></p>
<p>The same thing happens when you work with Git Bash:</p>
<p><img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb741.png" width="593" height="269" /></p>
<p><b>Easy trick – Git Proxy Settings</b></p>  

<p>Reason: Git doesn’t know my Proxy-address. At least he doesn’t know the Proxy in IE.</p>  

<p><b>Solution</b></p>  

<p>The GitHub support leaded me to this <a href="http://www.lmxm.net/using-github-for-windows-behind-microsoft-isa-proxy/">Blog</a> who suggests this alternative:</p>
<p><u>Either you configure it like this:</u></p>
<p><u></u></p>  <p align="left">Git config – global http.proxy <a href="http://proxyuser:proxypwd@proxy.server.com:8080/">http://proxyuser:proxypwd@proxy.server.com:8080</a></p>
<p><u>Or you safe it directly into .gitconfig (although the first alternative will be safe into .gitconfig as well)</u></p>
<p><u></u></p>
<p>[http]   <br />proxy = <a href="http://proxyuser:proxypwd@proxy.server.com:8080/">http://proxyuser:proxypwd@proxy.server.com:8080</a></p>
<p>With this it works with the nice GitHub Client and with the Git Bash as well. If you use a Proxy without authentication: simply enter the adress.</p>
