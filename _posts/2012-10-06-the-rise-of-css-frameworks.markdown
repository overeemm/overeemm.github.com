---
layout: posts
title: The rise of CSS Frameworks and some thoughts on how to use them
categories: [css,frameworks]
---

The advance of HTML5, CSS3 and responsive design has given rise to a whole new type of (open source) projects: [CSS frameworks][1]. The first time I saw these kind of projects I found it rather strange. I am used to frameworks in terms of reusable libraries giving some functionality. Like ASP.NET MVC, helping you serve content over HTTP. CSS however consists of rules and style definitions, there is no code, there is no functionality. These definitions can be reused of course, but it often becomes a pain to maintain large CSS files. The language itself does not have constructs to deal with reuse in a way like programming languages.

My first initial reaction was to view these frameworks not as ordinary frameworks, but rather as bootstraps: a basic starting point for your CSS design. Much like [CSS resets][2]. Such a bootstrap file gives you a certain amount of basic rules to build your layout. This approach does comes with some warnings. 

### Bootstrapping your project

Let us look at the relation between CSS and HTML and then how a bootstrap file can go wrong. CSS stylesheets consists of style definitions. These style definitions are annotated with selectors. These selectors link the style to your HTML file. In this way you get a clean separation: the HTML tells you what you are presenting and the CSS tells you how to present it. But they do have a stricter relationship then simple a separation of concerns. This relationship can go two ways. The selectors used in CSS can use some basic constructs: HTML tag names, CSS classes, ID attributes and some special cases (like hover, when a cursor is over an element). These constructs can be combined to match the structure of your HTML. The CSS file can use your nesting of HTML tags to link style definitions, but then the relationship becomes very fragile: a change in nesting can mess up your style. The other way around is also possible: you can annotate the HTML with class names and id attributes, but then you can wound up with to much knowledge about your CSS file in your HTML.

The two ways to use a CSS bootstrap are to use only the style definitions and not the selectors. This gives you freedom in both your HTML and CSS selector structure. However, it is lots of work and to be honest: you would only use the knowledge of the CSS in the bootstrap file and not the file itself. Another approach is to use the bootstrap file as is and change your HTML structure to match the file.

In the last approach, you get the extra benefit that it is possible to update the bootstrap file to incorporate bug fixes. But the usage of class names from the CSS file does violate the principle of separation: HTML tells what, and CSS tells how. These class names do not carry semantics for your project. They carry knowledge about what the rule definition does (for instance how much columns wide an element is presented).

### Extending CSS to gain reuse

This is where two popular languages that extend CSS come into play: [LESS][3] and [SASS][4]. These languages add extra features on top of CSS to make the language more flexible; they add constructs for re-usability. They both add variables and [mixins][7] to the language.

Variables allow you to reuse certain key values, like colors or sizes. This makes it easy to change the color palette in one place, instead of throughout your CSS rules. Mixins on the other hand allow you to reuse a set of style definitions inside other definitions. This allows you to reuse for instance a set of prefix specific definitions without writing them out every time.

These language allow you to distribute a CSS framework in terms of mixins and variables. That way you can set up your CSS file and HTML file in the cleanest way, but use the already defined style definitions and gain development speed.

### Reflection

So far I've used two CSS frameworks: [Skeleton][5] and [Zurb Foundation][6]. I like the Zurb framework because of the easy responsiveness that it adds. My approach is to add the Zurb class names throughout my HTML. Yes, this is not really clean, but the projects sizes are small and the likeliness of switching a framework or updating to a newer version are small. I am not sure of a larger project would succeed with such an approach. In such a project I would switch to just using the knowledge and style definitions, but not the actual code.

I am not sure if the LESS/SASS approach is any better. In my opinion, CSS and HTML are in a very strict relation, enforced by the language capabilities. Positioning and sizing within style definitions are depended on context of parent elements, and often you cannot view the CSS separate from the HTML. No language extension or framework will fix that problem.

But without the CSS frameworks, LESS/SASS do add better maintainability to your stylesheets. There are multiple tools to integrate it with your IDE. LESS even comes with a client-side library that transforms it on the fly to CSS. Performance would keep me from going that way, but it adds speed to the development process.

Depending on the size of a project, I would choose to learn from the CSS frameworks and write my own code or integrate them into the projects like any third party library.

[1]: http://en.wikipedia.org/wiki/CSS_frameworks
[2]: http://stackoverflow.com/questions/116754/best-css-reset
[3]: http://lesscss.org
[4]: http://sass-lang.com
[5]: http://www.getskeleton.com
[6]: http://foundation.zurb.com
[7]: http://en.wikipedia.org/wiki/Mixin
