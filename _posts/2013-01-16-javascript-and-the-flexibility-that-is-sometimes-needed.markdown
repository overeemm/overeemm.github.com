---
layout: posts
title: JavaScript and the flexibility that is sometimes needed
categories: [javascript]
---

Integration of third part libraries. The promises that a library makes often force you to delete your own code right away and start integrating. Finally, ripping out code and replacing it with a tool, framework or library that is developed by a team instead of you. Developed over a couple of months, maybe even years. And all is good.

But is it? Because using a third party library comes at a price. What if integration is not that easy. Or what if some parts are not that stable. What if the demands from your customers are a bit different. Of course, as good developers we start with analyzing, prototyping, proofing that the library does what it needs. And of course, every manager, or stakeholder, approves and gives you the green light. Until a new requirement is found, that was not known at that time. Or maybe just not told to you. And now what to do?

There are a lot of discussions about JavaScript. About it being a language that is bad designed. About [the risks][1] in large enterprises. Microsoft started to design [a better version][2]. But there is great power in this language. At least, that is my experience. You can shoot yourself in the foot. Sure, you can also hit yourself with a hammer. In a perfect world, we might want to have static analysis that finds all the errors before we release stuff in the world. But that is not going to happen.

If you want to program for the web, there is only one option: JavaScript. And sure, you can use [CoffeeScript][4], [TypeScript][2] or [ClojureScript][3]. But still, at one point you will hit JavaScript. I do not believe that we are already arrived at the point that [JavaScript is only the assembly language][5] underneath it all. I think that you still need to have some knowledge of the underlying core. Of course you can use tools that compile into JavaScript. But at least learn some, I think you will need it when debugging.

Back to integrating third party libraries. One thing that JavaScript stands out in, is the flexibility. The fact that you can change any object after creation makes it super easy to alter behavior of libraries. And you can mimic any object, without a type-system that will not allow it.

Sometimes it feels wrong to do this kind of stuff. And my computer science background screams for type-systems, proves of correctness and a high level of maintainability. But the maintainability of an application does not need to suffer from JavaScripts flexibility. You will need discipline. Maybe some unit tests. Even a little documentation.

But most important, we as developers need to get the job done and deliver what our customers want. Because in the end, all they care about is working applications that do the job for them in the best possible way.

[1]: http://www.sig.eu/en/News_and_publications/TechnologyRiskReview/997/__JavaScript:_flexibility_can_come_at_a_price__.html
[2]: http://www.typescriptlang.org
[3]: https://github.com/clojure/clojurescript
[4]: http://coffeescript.org
[5]: http://www.hanselman.com/blog/JavaScriptIsAssemblyLanguageForTheWebSematicMarkupIsDeadCleanVsMachinecodedHTML.aspx