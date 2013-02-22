---
layout: posts
title: JavaScript architecture - some lessons learned
categories: [javascript, architecture]
---

> The secret to building large apps is NEVER build large apps. Break up your applications into small pieces. 
> Then, assemble those testable, bite-sized pieces into your big application. [Brian Moschel][1]

In the past year, I've been busy with researching architecture in JavaScript.
And there are a few things that I've found and learned during this year.
Now, I am nowhere near being an expert, I'm just someone that loves the language and the platform it offers.
I happen to like the fact that it opens up so much possibilities, because JavaScript is everywhere!

I would like to list some principles and observations that follow from my learning.

### Create little parts and not one big application

Now, JavaScript is a language that misses some features when you compare it with more mature and better designed language. There are no modules or namespaces, and there is only function-scope. When you do not look for a solution for this missing, you end up with spaghetti code. One of the things I've learned is that a module framework is one of the most important parts of your application.

I myself are only in depth familiar with [RequireJS][2], but there are more. A module framework forces you to break up your app in little parts that have their own scope and clearly export what others can use.

    #js
    define("moduleNam", [/* list of dependencies*/], 
      function ( /* dependencies */ ) {
      
        /* place for private code */
      
        return {
          /* your contract with the outside */
        }
      }
    );

You see, a module has clear definition of what it needs and what it delivers. This way, you are forced to let go of easy global variables.

### Create abstractions for changing things that you cannot control

One of the really hard points that I discovered is working with a quickly changing environment. The third party libraries are developing fast. And it makes it hard to plan for the future. Of course no one wants to right code that we do not need.
But last year, the [jQuery UI library][3] was widely used. Nowadays, it is [Twitter Bootstrap][4] everywhere. 
How do you deal with this? Do you choose and never look back? Or do you try to abstract it? Or do you write your own code?

Unfortunately there are no easy answers here. I found it to be possible to abstract some of these choices. But your situation might be different. But it is important to choose widely what you depend on. The JavaScript world is moving fast. Depending on something that is hard to change might be a mistake.

### Be careful with accessing the DOM

The DOM is what your environment looks like in JavaScript running in the browser. You will need it. But you do not want it to be everywhere. JavaScript code is not compiled. Unit tests are more important then ever. Nobody likes script errors that make a web application become useless. So testing is required. But using the DOM everywhere makes your code untestable. Writing code that is modular and testable is a must!

There are more lessons and more paths to follow. But these three items are key to success in my opinion.

[1]: http://bitovi.com/blog/2010/11/organizing-a-jquery-application.html
[2]: http://requirejs.org
[3]: http://jqueryui.com
[4]: http://twitter.github.com/bootstrap/