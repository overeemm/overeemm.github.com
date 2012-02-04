---
layout: posts
title: Blogging in the cloud through gdocerous
tags: [blogging, gdocerous]
---

I’m trying to find more time and topics to blog about. While doing that, I reviewed the work flow that I have for blogging. My blog is hosted on posterous.com, a great service (although they do have some downtime-issues). It makes posting a blog easy: just send a mail to posterous@posterous.com and your done. The sender e-mail is recognized and the e-mail is converted to a blog post. They automatically convert images to galleries, embed youtube links and so on. If you are working on multiple blog posts at a time, you could store your mail as a draft or store it somewhere else. Somewhere else is no fun, because it means that you will have to have some other intermediate form. Storing it as a draft means that you do not have history on your blog post.

Lately I’ve started to use google docs more and more. And it looked like a great place to store my draft-blog posts. But about posting the final version? In that case you had to copy-paste it over to your e-mail and reformat it. Not that easy and fun anymore.

And then I got this great idea: wouldn’t it be great if you could choose your google document and post it to posterous? Of course it would be :) So that became my weekend-project: gdocerous. (the domain is not yet active, it should be this week).

I have a working version, but unfortunately it is not open for public yet. I still have some things to finish before opening it up:

+ I want to use the OAuth API instead of the AuthSub version
+ I still need to figure out how to use gmail’s smtp through OAuth

The whole thing is pretty simple, but I think useful. One of the things is that I do not want to store sensitive data or user accounts. That’s why the posterous API is not usable. Another thing is that I only want to select and post, no mutation of articles and such. Last is that the complete source code is open for everyone to look at.