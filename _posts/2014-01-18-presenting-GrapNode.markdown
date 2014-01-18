---
layout: posts
title: Presenting GrapNode
categories: [javascript, node, rx]
---

I'm like [a couple of years late][1] to the party, but finally started a node.js project. The plan is to log my progress on the blog, by writing about different aspects and maybe do some explaining of the core concepts (mostly for me, but maybe some others see benefit from it). The project will combine a couple of things. First of all node.js. I love JavaScript, and hope to learn more about using it on the server. Along with node.js, the project will use [RxJS][2], the reactive programming library for JavaScript. After following the [Principles of Reactive Programming on coursera][3] a project to get some real life experience was needed! And of course, the project might catch up on other cool stuff as well.

In this series, I will try to take (baby)-steps in developing a system to take screenshots from given urls. [PhantomJS][4] will be used to take those screenshots, as I am in no way looking to build a browser environment myself. The website will allow you to enter an url, and maybe a screen size. The result will be a png of the website at the given url. The code will be online of course, at [github][5].

### Getting started

We start with npm, the Node Package Manager. The packages that are needed are [rx][6] and [node-phantom][7]. Installing is easy:

    npm install rx
    npm install node-phantom

After that we will want to create a package.json with:

    npm init

This will ask you lots of questions, but will give you a package.json that can be used to restore dependencies and tell lots of things about your application. I am not sure what the other uses are yet, but I hope to find out later on. So now we got some dependencies and an application definition.

### Write some code

Let's get a node.js server up and running. We create an index.js file and put the following lines of code in it:

    #js
    require('http').createServer(function handleRequest(req, res) {
      res.writeHead(200, { 'content-type': 'text/plain'});
      res.end('Hello World!');
    }).listen(8080);

This will give us a nice "Hello World!", but in the plain node.js way. We want to rx-ify it. For now we will install a new dependency, [rx-httpserver][8]:

    npm install rx-http-server --save

This will allow us to change the code to

    #js
    var RxServer = require("rx-http-server");
    
    var server = new RxServer();
    
    server.requests.subscribe(function(data) {
        data.response.writeHead(200, {"Content-Type": "text/plain"});
        data.response.end("Hello from Rx!");
    });
    
    server.listen(8080);

Much nicer, right? I might change that dependency later on to a custom written library, so that I can learn more about the internals.

That's all for now. Lets hook up PhantomJS the next time. Feedback is welcome at anytime, just send a message through twitter or google+!

[1]: http://devopsangle.com/2013/04/01/the-birth-of-node-where-did-it-come-from-creator-ryan-dahl-shares-the-history/
[2]: https://github.com/Reactive-Extensions/RxJS
[3]: https://www.coursera.org/course/reactive
[4]: http://phantomjs.org
[5]: https://github.com/overeemm/GrapNode
[6]: https://npmjs.org/package/rx
[7]: https://npmjs.org/package/node-phantom
[8]: https://npmjs.org/package/rx-http-server