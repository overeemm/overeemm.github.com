---
layout: posts
title: Fall 2012 update on projects
categories: [projects]
---
I thought it would be nice to give an update on some of the side projects that I'm working on. Reflecting on those I see that I currently am focussed on design, CSS3, responsiveness and less on core programming.

The first side project is very simple and plain informative website. I've started using [Zurb Foundation][1] as a CSS bootstrap and worked with [Jekyll][2] as a templating engine. There is no server-side stuff in this website but a simple mail form. I could really focus on (responsive) design on this one and I think it worked out nice.

The second project is a larger website build using ASP.NET MVC. Originally it was a website build on [Joomla][3] but I got fed up with it, because it feld limitting. Any CMS will force you in setting up pages a certain way. And styling or plugins are also limited in a way. I wanted more flexibility, so choose to build something from scratch.

I started with the design of the frontpage, again using [Zurb Foundation][1]. After the frontpage I started to add dynamic pages to move the current content. As back-end storage it used a mix of plain text files: csv, xml and markdown files. This allows me to manage the content through FTP and also to easily backup the content. There will be an administrative part, but it can wait because I am the sole maintainer.

Another reason to rebuilt this one is that I want to build a [Phonegap/Cordova][4] app that builds on stuff in the website. By designing the website in a mobile-ready way, I hope that the app will be easier.

This project is on the edge of ready for release, but I need to get approval from certain people first.

The last project is not really started yet, but I am researching the design currently. Again a simple website, but with a larger backend. Multiple roles will need to login and do their stuff. This one is constrained by the hosting platform. Therefor it will be written in PHP, and of course HTML5 and CSS3. I haven't written PHP in years, so I am curious on the current state. Hopefully some blog posts will come from that project.

The last project is of course this blog. I hope to get to writting some posts soon. There is enough to write about, but time is limited.

[1]: http://foundation.zurb.com/
[2]: https://github.com/mojombo/jekyll/
[3]: http://www.joomla.org/
[4]: http://phonegap.com/