---
layout: posts
title: Testing if the eventloop is blocked
categories: [javascript, node, eventloop]
---

In the previous post, we talked about some core concepts of node.js, one of them being the event loop.
In the first version of the GrapNode code, we did some asynchronous phantomjs stuff. That looked good, but I wanted to know for sure that the eventloop is not blocked during the 1.5 second that it takes to make a screenshot.

So I added a couple of other requests handlers to my code. See how we can do this with rx? We can assign the stream of requests to a variable, filter them and subscribe to the filtered stream:

    #js
    var server = new RxServer();
    var requests = server.requests;

    requests
      .filter(function (data) { return data.request.url === "/"; })
      .subscribe(function(data) {
        /* The root, returns a simple HTML page */
      });

    requests
      .filter(function (data) { return data.request.url === "/capture"; })
      .subscribe(function(data) {
        /* Here we will perform our screencapture stuff */
      });

    requests
      .filter(function (data) { return data.request.url.indexOf("/assets") === 0; })
      .subscribe(function(data) {
        /* Simple handler for static content */
      });

    requests
      .filter(function (data) { return data.request.url === "/time"; })
      .subscribe(function(data) {
        /* Returns the server time */
      });

    server.listen(8080);

By writing four different filters and four different subscribe handlers, we have created a basic routing mechanism.

Requests for the root URL will get back a basic HTML page that include a (clientside) JavaScript file with the following code:

    #js
    (function () {
      var requestTime = function () {
        var req = new XMLHttpRequest();
        req.open("GET", "/time");
        req.onreadystatechange = function (oEvent) {
          if (req.readyState === 4) {
            if (req.status === 200) {
              document.getElementsByTagName("span")[0].innerText = req.responseText;
            } else {
              document.getElementsByTagName("span")[0].innerText = req.statusText;
            }
            requestTime();
          }
        };
        req.send(null);
      };
      requestTime();
    })();

Basically, this fetches the time of the server and shows it in an element. This fetching is done continuously, so when we block the event loop, the time will fall behind and start to lag. Along with this time polling, I will start the [ab executable][1] to do some concurrent requests for the screenshot (/capture).

<iframe width="560" height="315" src="//www.youtube.com/embed/cubOIq-ak_0" frameborder="0" allowfullscreen="allowfullscreen">
</iframe>

As one can see in the video, the timer keeps running and is not blocked by the concurrent requests for the screencapture. This basically shows that our screencapture does not block the event loop. 

_There is another problem: the phantomjs bridge spawns new processes and those are limited. So we need to build a request queue and allow people to poll for their screencapture (or use socket.io to apply some realtime web)._

    $ ab -n 10 -c 1 http://127.0.01:8080/capture
      Time taken for tests:   15.673 seconds

    $ ab -n 10 -c 2 http://127.0.01:8080/capture
      Time taken for tests:   9.555 seconds
    
    $ ab -n 10 -c 5 http://127.0.01:8080/capture
      Time taken for tests:   7.379 seconds

The results from ab show fine numbers, I guess. I am not sure yet how to read this, but hopefully, that will come later on.

[1]: http://httpd.apache.org/docs/2.2/programs/ab.html