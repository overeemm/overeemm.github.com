---
layout: posts
title: First steps in a new language
categories: [ruby, learning]
---
One of my all time favorite books on software development is The Pragmatic Programmer: From Journeyman To Master. I highly recommend it. You do not have it? You didn’t read it yet? Go buy it!

One of the tips given in the book is

> Learn at least one new language every year. Different languages solve the same 
> problems in different ways. By learning several different approaches, you can 
> help broaden your thinking and avoid getting stuck in a rut. Additionally, 
> learning many languages is far easier now, thanks to the wealth of freely 
> available software on the Internet.

Although I read through the seven languages in seven days book and the book of Ruby this year, I was still not comfortable enough with Ruby to use it when necessary. So one of my goals was to really learn Ruby. And the best way to learn is by using it. Like Knuth already said in his classic The Art Of Computer Programming:

> It is difficult, if not impossible, for anyone to learn a subject purely by
> reading about it, without applying the information to specific problems and
> thereby begin encouraged to think about what has been read. Furthermore, we all
> learn best the things that we have discovered for ourselves.

This write-up is a summary of my first (baby) steps in Ruby.

I started out with doing some katas from the pragmatic programmers. I quickly found out that although they are good exercises, they did not interest me. And because of that I was not really motivated in doing them.

The second exercise was a rewrite of some tool that I build in C# to encode, upload and publish mp3 files to a Joomla-website. The .NET tool is a WinForms executable, using lame to encode, using ftp to upload and using XML-RPC to publish the post. The Ruby version is one script, doing all those things in sequential steps. This exercise was more fun, because I needed it (the .NET tool didn’t work on my MacBook).

My scripts can be found on [github][1].

So far so good. I like the language, but I do realize that these are just scripts. And by no means they are building a real life system. So the next exercise should be bigger, forcing me to use more advanced features. Maybe I should rewrite the Joomla-website in a simple RoR version?

How do you learn a new language?

[1]: https://github.com/overeemm/LanguageExperiments/tree/master/ruby
