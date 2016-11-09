---
layout: posts
title: Things learned from the Game of Life
categories: [javascript]
---
_I mentioned in the [previous blog post][6] that I am digging deeper into JavaScript lately. This post is the second in a series about JavaScript._

I love mathematical stuff. Things like [fractals][1], but also [Conway's Game of Life][2] are amazing. So I started implementing the Game of Life using JavaScript ([github][5] and [a running example][4]). While implementing, I used a number of patterns and techniques that are worth explaining. These patterns are useful because they let you work more effective with the language. 

But let me first explain two differences between JavaScript and traditional object-oriented languages.

### Scope

JavaScript only knows two scopes: the global scope and the function scope. This is different then for example C#. In C# block structures also start a new scope. So when you use a conditional or a loop structure, you know that you enter a new scope. This gives advantages in reuse variable names, but it also means that the garbage collector can free up memory when your done. The next example shows how this works in C#.

    #java
    if(condition) {
        int i = 9 + 8;
        // do something with i
    }
    return i; // error, because i is out of scope
  
If we would try the same in JavaScript, it would lead to different results.

    #js
    if(condition) {
        var i = 9 + 8; 
        // do something with i
    }
    return i; // fine, i is in scope
  
This can really fool you. Because these are the only two scopes we have, libraries that define global variables (like jQuery does) can conflict with each other easily. So you want and should be careful to not pollute the global scope. That is also why you should always use the **var** keyword to declare your variables. Declarations without this keyword cause global variables, even if you only use them inside of a function.

### Access modifiers

One of the first things that we learn in object-oriented languages is the usage of **private** and **public** modifiers. With those modifiers it possible to hide information inside of object instances. It is one of the core principles of good code design: information hiding.

In JavaScript there is no such thing as access modifiers. 
So we are left with two choices: give away our implementation details, or come up with some clever way to hide them anyway.

These two key differences between JavaScript and other object-oriented languages make that we should come up with some clever way's to structure and design our code. There are three things that I would like to point out.

### Namespaces

The first thing that I choose to do is adding namespaces to my code. Namespaces are used to create unique names within a certain naming scope. C# has them, and so does XML. Unfortunately, JavaScript doesn't know namespaces. But you can still simulate them by using object properties.

    #js
    // create the first level
    var overeemm = window.overeemm || {}; 
    // create a subnamespace
    overeemm.conway = overeemm.conway || {}; 
    // continue creating as many levels as you would like

What happens is actually very simple: we just create empty objects to simulate a namespace level. By using the OR notation I am sure not to redefine another namespace and thus clearing functions and objects created on that level. By putting this into every file, I know that the namespace that I will use is available.

With this simple technique we prevent naming conflicts with other libraries.

### Hiding your private stuff 

We want to prevent all of our internal implementation details to leak out of our objects. _At least I would._ So we need some clever way to do this. There are a couple of things that you could do, I have been using closures to achieve this.

A [closure][3] is a function stored together with his environment (the variable bindings). When you execute this function, it has access to the variable bindings from the stored environment. It is said that the function is closed over the free variables. Because the function keeps access to all those variables, it is possible to create an hidden environment. This hidden environment is only accessible by the functions defined in this environment. 

    #js
    var f, g;
    function doSomething(x, y) {
        var z = x + y;
        f = function () { return z++; };
        g = function () { return z--; };
    }
    doSomething(1, 4);
    // f and g are free to use

After calling **doSomething** we can use the functions **f** and **g**. But they are defined within the scope of **doSomething**! Because they are defined in the same scope, they both have access to the same variable **z**. This variable is not available outside of **doSomething**, making it a private variable.

If we change the code snippet a little bit, we can create an object with his own private scope.
            
    #js
    function createObject(x, y) {
        var z = x + y;
        return {
            f : function () { return z++; },
            g : function () { return z--; }
        }
    }
    var o = createObject(1, 4);
    // o.f and o.g are free to use

With this technique we actually achieved two things:

+ we created an object **o** with a private state.
+ we did not pollute the global namespace with **f** and **g**, but only with **o**.

### Singletons through self executing functions

But what if we have a library with utilities. Or we want to create a singleton object? We can use the same technique, but place it in a different context.

    #js
    var singleton = (function(x, y) {
        var z = x + y;
        return {
            f : function () { return z++; },
            g : function () { return z--; }
        }
    })();
    // singleton.f and singleton.g are free to use

We change the create-function to an anonymous function that is directly executed. Because of the object that we return, we have created a private scope. And by executing the anonymous function, we make sure that nobody can create a second instance.

For me, these examples show a nice and elegant side of JavaScript. One could argue that they are solution around problems in a bad designed language. However, we have to deal with our inheritance even if we do not like it. And to me, these techniques show creativity and possibilities.

### Singleton revisited

After a twitter discussion I wanted to revisite the singleton pattern a bit. One of the aspects of the singleton pattern is the fact that you can ask for the thing you want and get the same instance back, no matter how often you ask for it. That is something that was not captured in my code example.

On stackoverflow [a nice example][7] is given. The principle is that you redefine the creation function and let it return the same instance. I thought it was a creative solution, and again I am amazed by the things you can do :-)

    #js
    function singleton() {
      var instance = (function() {
        var privateVar;
    
        function privateMethod () {
          // ...
        }
    
        return { // public interface
          publicMethod1: function () {
              // private members can be accessed here
           },
          publicMethod2: function () {
            // ...
          }
        };
      })();
    
      singleton = function () { // re-define the function for subsequent calls
        return instance;
      };
    
      return singleton(); // call the new function
    }


[1]: http://en.wikipedia.org/wiki/Fractal
[2]: http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life
[3]: http://en.wikipedia.org/wiki/Closure_(computer_science)
[4]: http://www.movereem.nl/conwaysscriptoflife/
[5]: https://github.com/overeemm/ConwaysScriptOfLife
[6]: http://www.movereem.nl/evolving-technology-javascript
[7]: http://stackoverflow.com/a/1895669
