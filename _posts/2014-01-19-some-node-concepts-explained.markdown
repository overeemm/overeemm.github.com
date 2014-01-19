---
layout: posts
title: Some node concepts explained
categories: [javascript, node, rx]
---

The project GrapNode is my attempt to get some experience with node.js and understand the key concepts and benefits. This short post will allow me to touch on these a little. By no means is this to be seen as the best explanation of these concepts. These are just my thoughts, and summaries of other explanations.

### Modules

Node.js uses a simple module system, files are modules and modules are files. A module can be loaded through the function require. In the first post this function was used to require the http module:

    #js
    var http = require('http');

This call will look for the module (or file) _http_ and load it. Where will it look? The [documentation][2] explains that it will do a couple of things:

- Is there a core module with this name (which there is in the case of _http_).
- If the argument starts with '/', it is an absolute path and the file is searched for on disc.
- If the argument starts with './', the path is relative to the current folder.
- If the argument has no leading './' or '/', node.js will look in the node_modules folder. This is the folder that is created by npm when a dependency is added.

This last step has some really smart rules which allow you to create a hierarchy of folders and dependencies. I suggest that you read the docs for more info!

### The event loop

The code that is written in node.js, runs single threaded. You do not need any lock or synchronize constructs to prevent concurrency problems. Better yet, there are no locks! However, the runtime itself offers asynchronousity. When we do a http request, or a database call, or some IO, the node.js runtime will execute that task on another thread. You cannot interact with that thread, but you are allowed to give a callback function, that is executed when the work is done. How does this work in a single threaded runtime? When will that callback be executed? The _event loop_ takes care of that.

Basically, the event loop is an ever running while loop that executes code. When a callback should be executed, the runtime puts it in a queue (the following code snippets are copied from [this stackoverflow answer][4]):

    #js
    lock (queue) {
      queue.push(event);
    }

The event loop itself will take this queue and execute the functions on every _tick_:

    #js
    while (true) {
      // this is the beginning of a tick
    
      lock (queue) {
        // copy the current queue items into thread-local memory
        var tickEvents = copy(queue); 
        // ..and empty out the shared queue
        queue.empty(); 
      }
    
      for (var i = 0; i < tickEvents.length; i++) {
        InvokeJSFunction(tickEvents[i]);
      }
    
      // this the end of the tick
    }

So on every thick, the queue is emptied and executed. All callbacks triggered while executing those functions are put in the queue, and executed on the next tick.

Scheduling stuff with setTimeout or setInterval actually means that the given callback is _queued_ after the given timeout or interval, not executed. That is why you should never block the event loop with cpu intensive or long running tasks! This will prevent the event loop of executing queued callbacks and thus firing events.

### Other posts in this serie

- [Presenting GrapNode][1]

[1]: /presenting-GrapNode/
[2]: http://nodejs.org/api/modules.html#loading_from_node_modules_Folders
[3]: http://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/
[4]: http://stackoverflow.com/a/19823583/5043