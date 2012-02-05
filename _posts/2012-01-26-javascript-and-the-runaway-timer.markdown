---
layout: posts
title: JavaScript and the runaway timer
categories: [javascript]
---
One of the things that you probably will run into when creating a JavaScript application is the runaway timer. The runaway timer? Yes, the runaway timer. Ever seen this message?

![IE message](/images/runaway_ie.gif)

or this one?

![Chrome message](/images/runaway_chrome.png)

that is [the runaway timer][1] telling you that something is running out of control. It is the thing that keeps your browser from crashing or not responding because of a long running script. JavaScript in browsers is executed in a single thread. But, any DOM manipulation or event handling is also done on this single thread (yes I'm ignoring [web workers][2] in this post. When your script runs for seconds, no other thing can be done thus making your browser unresponsive. Clicks on buttons or hyperlinks will not be executed, hovers or other effects will do nothing. You should be timing your JavaScript code and split everything that takes more than 100 ms. Why 100 ms? The different runaway timers have enforce limits of multiple seconds. [Usability research][3] however shows that humans find a User Interface responsive when it reacts within 100 ms.

I myself ran into problems with the runaway timer when implementing the Mandelbrot figure using JavaScript and the Canvas element. The basis of the mandelbrot figure is an algorithm that calculates a color for every pixel on the canvas. A nested for-loop that is, visiting every pixel and doing some calculation. With a 400 by 400 pixel canvas, I was already running into problems.

    #!javascript
    var paint = function () {
        for (var screenX = startpoint_x; screenX &lt; endpoint_x ; screenX++) {
            for (var screenY = startpoint_y; screenY &lt; endpoint_y ; screenY++) {
                var xynumber = getNumber(screenX, screenY, center_x, center_y);
                var color = getColor(xynumber);
                setColor(color);
                canvascontext.fillRect(screenX + canvas_half_width, screenY + canvas_half_height, 1, 1);
            }
        }
    };

The solution was to split this up in parts that run within the 100 ms limit (actually I choose 50 ms blocks to stay on the safe side).

    #!javascript
    var paint = function () {
        paintPart(startpoint_x);
    };
    
    var paintPart = function(start_x) {
        var start = new Date();
        for (var screenX = start_x; screenX &lt; endpoint_x &amp;&amp; (new Date() - start) &lt; 50 ; screenX++) {
            for (var screenY = startpoint_y; screenY &lt; endpoint_y ; screenY++) {
                var xynumber = number.getNumber(screenX, screenY, center_x, center_y);
                var color = number.getColor(xynumber);
                setColor(color);
                canvascontext.fillRect(screenX + canvas_half_width, screenY + canvas_half_height, 1, 1);
            }
        }    
        if(screenX &lt; endpoint_x) {
            setTimeout(function() { paintPart(screenX); }, 25);
        }
    };

The effect of applying this pattern is that the browser stays responsive. Of course, the splitting also gives some overhead. But the site and the browser stay usable, and that is key in every good UI.

Want to more about creating responsive JavaScript applications? [This presentation][4] by Nicholas Zakas is a must see.

[1]: http://www.nczonline.net/blog/2009/01/05/what-determines-that-a-script-is-long-running/
[2]: http://en.wikipedia.org/wiki/Web_worker
[3]: http://www.useit.com/papers/responsetime.html
[4]: http://www.slideshare.net/nzakas/responsive-interfaces
