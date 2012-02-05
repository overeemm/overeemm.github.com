---
layout: posts
title: The rise of JavaScript
categories: [javascript]
---
_I'm digging deeper into JavaScript lately, and I like what I find, see and try._ 

I remember that when I first saw JavaScript I was building a website for our graduating class in high school, it was around 2001. It was the period that JavaScript was used in the marketing term [DHTML][1]. It was primarily used for dynamic menu's with overlays and for the snow effects during winter. And yes, I am guilty for doing that too. 

During college, fast forward three years, we used JavaScript for client-side validation. It was still advertised as a scripting language, useful for dynamic HTML.

Then during my first full-time job, the rise of [AJAX][2] happened. Microsoft added dynamic capabilities through various WebControls (like the UpdatePanel). Prototype.js was the first major library. Everything and everybody that used AJAX was hot. People started pointing out the downsides: JavaScript could be disabled, so websites should work without it. The battle of browser compatibility began, Internet Explorer 6 was still mainstream.

And then, slowly it all evolved from sprinkling extra's into an HTML environment to full web applications build in JavaScript. Great examples are of course gmail, google docs and [Cloud 9 IDE][3]. JavaScript has grown into a serious programming language, that people should know and care about. With [Node.js][4] it even became possible to write your web application's server-side in JavaScript. Microsoft is enabling you to use it for desktop applications on [Windows 8][5].

Some saw this evolution, thought about it and stated bold things:

&gt; Any application that can be written in JavaScript, will eventually be written in JavaScript. ([Atwood's Law][9]. _Yes, it was written in 2007, I know that I'm late to the game..._)

and

&gt; JavaScript is an assembly language. The JavaScript + HTML generate is like a .NET assembly. The browser can execute it, but no human should really care what&rsquo;s there.([Erik Meijer on hanselman.com][10])

Atwood's Law is getting confirmed every day: real life web applications and games are written in JavaScript, not only for fun! Hanselman's statement is seen in things like the Google Web Toolkit and the Cappuccino framework. But also in ClojureScript and CoffeeScript. They all try to lift some of the problems in JavaScript by using other languages or creating new languages that compile into JavaScript.

I am not sure if that is the way to go. I like JavaScript. I've read some great books by [Douglas Crockford (JavaScript The Good Parts)][6], [John Resig (Pro JavaScript Techniques)][7] and [David Flanagam (JavaScript The Definitive Guid)][8]. Still I think that I scratched nothing but the surface. There are so many tools, libraries and frameworks from which I can learn. However, I can also see where those abstraction are coming from. I never learned C or C++. It is great that we have C#, Java, Ruby and so on on top of those languages to make development easier, faster and less error prone. But is JavaScript already in that stage? I am not sure... only time will tell.

_I hope to digg into JavaScript and some libraries in the next couple blog posts._

[1]: http://nl.wikipedia.org/wiki/Dynamic_HTML
[2]: http://nl.wikipedia.org/wiki/Asynchronous_JavaScript_and_XML
[3]: http://c9.io/
[4]: http://nodejs.org/
[5]: http://msdn.microsoft.com/en-us/library/windows/apps/br229565%28v=VS.85%29.aspx
[6]: http://shop.oreilly.com/product/9780596517748.do
[7]: http://jspro.org/
[8]: http://shop.oreilly.com/product/9780596101992.do
[9]: http://www.codinghorror.com/blog/2007/07/the-principle-of-least-power.html
[10]: http://www.hanselman.com/blog/JavaScriptIsAssemblyLanguageForTheWebSematicMarkupIsDeadCleanVsMachinecodedHTML.aspx
