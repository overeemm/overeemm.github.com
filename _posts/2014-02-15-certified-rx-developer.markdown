---
layout: posts
title: Certified Rx Developer
categories: [rx]
---

<p style="float:right; width: 200px; margin-left: 20px;">
  <img src="/images/certified-rx-sticker.jpg" alt="Certified Rx Developer" 
       title="Certified Rx Developer" style="width:200px;">
</p>

My current company gives every employee a training budget. This budget can be spend on every training that you like, there is no need to ask permission up front (but how you spend it is part of the evaluation). So when [Erik Meijer][3] asked on [twitter][4] if people wanted to follow a _certified Rx developer_ training, I had a training to spend my money on.

I had already taken the coursera course [Principles of Reactive Programming][5] (which is highly recommended!), but the Rx stuff had not really sunk in yet. That is why I started [a new project][6]. But still, the opportunity to learn from Erik, the one that started the whole Rx movement (years ago)? I had to be there :-)

This blog post is a combination of a summary of Rx and my experience following the _certified Rx developer_ training. Let me start with the last, my experience. Erik is a great teacher. Not only is he super smart, he is also funny, entertaining and a nice person to chat with. But more, he is a real hacker, not some scientist that cannot write code. He honestly ended our lunch to start hacking on some idea (with _goto_'s for crying out loud)!

So, what is Rx anyway? I could point you to [this great introduction][1], or to [the code][2]. But it might be fun to see if I can explain some things. I like the path that Erik took, which started not with Rx, but with monads. Those things always sound awful. In the Haskell community, there is a quote saying that they should have called those things _warm fuzzy things_, because that sounds so much more fun. But monad it is. But before we get to monads, let us first look at exceptions.

### Exceptions

When programming C# and JavaScript (my primarly languages), exceptions are used to abort the execution when something happends that you did not expect. Which exceptions a method might throw is not visible from the outside. The method signature does not tell you about those cases (in Java you have checked exceptions, not in C#). So when we have a method like this, the signature is lying to us!

    #c#
    private int DoSomething(int number)
    {
      if(number < 0) {
        throw new Exception("No negative numbers allowed.");
      }
    
      return number * number;
    }

It tells us that this method takes a integer as argument, and returns an integer. There is however, another possible outcome: the exception. So the effect of failing is hidden from us. To make this more explicit, we introduce the _Try_ type. Using this will give us the following method (implementing the Try type is left as an exercise to the reader).

    #c#
    private Try&lt;int&gt; DoSomething(int number)
    {
      if(number < 0) {
        return Try { Failure = new Exception("No negative numbers allowed.") };
      }
     
      return Try { Success = number * number };
    }

This signature shows us that the method has two possible outputs: an integer, or an error. We have made the possibility of failure explicit.

### The four essential effects in programming

<p style="float:right; width: 400px; margin-left: 20px;">
  <img src="/images/rx-erik.jpg" alt="Erik Meijer" title="Erik Meijer" style="width:400px;">
</p>

The Try type is essential for programming, because we use it for synchronous, single values. If we move from single values to many values we need to use another type, the Iterable type. In C#, this is the _Enumerable_ type. But this type is still synchronous. This means that asking values from an Iterable blocks the execution till all values are given. You are pulling values from the iterable and that blocks the execution. When you want to develop a scalable, responsive system, blocking is not done: asynchronous is the way to go. 

If we move from a synchronous single value to an asynchronous single value, we need yet another type, the _Future_ type. This type makes it explicit that we have latency. It might take a while for a value to be available. In C#, this is done using the _Task_ type, which corresponds with Futures. In JavaScript we have the Future/Promise objects. 

The last step is from a asynchronous single value to multiple values, asychronously. That is where the _Observable_ type comes into play. And that, that is the heart of Rx. To summarise these four effects:

<table class="content">
  <tr>
    <td></td><th>Single</th><th>Many</th>
  </tr>
  <tr>
    <th>Synchronous</th><td>Try&lt;T&gt;</td><td>Iterable&lt;T&gt;</td>
  </tr>
  <tr>
    <th>Asynchronous</th><td>Future&lt;T&gtl;</td><td>Observable&lt;T&gt;</td>
  </tr>
</table>

But what about monads? And why is Rx so special? Untill now I only explained four effects and the corresponding types. Let's move on to monads now.

### Monads

So what is a monad? I do not pretend to understand it better then others, so I will not try to write a formal (or informal for that matter) explanation. Search google if you want the real stuff, or [read this one][8]! For now, all you need to grasp is that a monad is a datastructure that comes with a set of functions, of which I will give you three examples in C#:

    #c#
    public static Something&lt;U&gt; FlatMap(this Something&lt;T&gt; source, Func&lt;T, Something&lt;U&gt;&gt; func)
    
    public static Something&lt;U&gt; Map(this Something&lt;T&gt; source, Func&lt;T, U&gt; func)   
    
    public static Something&lt;U&gt; Filter(this Something&lt;T&gt; source, Func&lt;T, bool&gt; func)   

Now, these types are very general. But you could substitute the generic _Something_ type with a couple of familiar types. For instance with _Try_. Or with _Future_, _Iterable_. You might even substite it with _Observable_. 

A monad is a general data structure that gives you a set of common operations (others are _zip_ and _flatten_). From them, you can build lots of stuff. You can filter, combine and project your data. And that is the power of monads. They give you a basic set of functions that work on all four effects: synchronous, asynchronous, single or many. 

_By the way, the names FlatMap, Map and Filter seem new. But just put them next to the [LINQ][7] functions SelectMany, Select and Where. They do look familiar now, don't they?_

### Rx

Rx thus fills in the last quadrant, the missing piece: many asynchronous values. Now that I write it down, it all seems very simple and nothing new in general. But the Rx library gives you an implementation of the _Observable_ type, with all the monad functions implemented. It allows you to turn any stream of data (from a database, from a webservice, from an input device, from an user) into an Observable that you can then filter, combine and project. And all of this is asynchronous by default. You are forced to think about latency, which is a good thing if you want your app to stay responsive.

In the training, Erik went on the give some example use cases. Projecting earthquakes on a map for instance. Or updating the average earthquake strength per region while earthquake data comes in. The guys at github used Rx to build a [reactive UI][9]. Netflix [uses][10] Rx. [Square][11] (could not find any source but Erik told us) and [SoundCloud][12] as well.

One thing you have to keep in mind, is that Rx is not about concurrency, or parallelism. It is about asynchronous computations. However, you could use it for concurrency and parallelism, using the schedulers that they implemented. But that is not the key point.

### Take aways

So what are the take aways from this training and Rx in general? Let me offer a small list of things that I can think of:

- Programming is fun, seeing Erik enjoy coding and talking about it was such an inspiration.
- Rx is not hard, it makes stuff easier because you can use the same toolset that you use with synchronous or single value sources.
- Asynchronousity and latency are effects that we need to make explicit.
- Futures and promises are also not that hard. They are cleaner then callbacks as a pattern.


[1]: http://www.introtorx.com
[2]: https://github.com/Reactive-Extensions
[3]: http://en.wikipedia.org/wiki/Erik_Meijer_(computer_scientist)
[4]: https://twitter.com/headinthebox/status/428851131003830272
[5]: https://www.coursera.org/course/reactive
[6]: /presenting-GrapNode/
[7]: http://msdn.microsoft.com/en-us/library/bb397926.aspx
[8]: http://stackoverflow.com/questions/2704652/monad-in-plain-english-for-the-oop-programmer-with-no-fp-background/2704795#2704795
[9]: http://www.reactiveui.net
[10]: https://github.com/Netflix/RxJava
[11]: https://squareup.com
[12]: https://github.com/soundcloud/RxJava