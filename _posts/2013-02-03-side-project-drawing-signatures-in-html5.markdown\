---
layout: posts
title: Side project: drawing signatures in HTML5
categories: [javascript, html5, projects]
---

This weekend I worked on another little side project. This time it consisted of a little HTML5 and JavaScript to use the power of a small tablet ([iPad mini][1] in this case). And it amazes me again how easy it is to put some open source projects together and get something working. Of course the proof-of-concept is no big deal, and it is not production ready. But all these little machines that we have give us so many possibilities.

The requirements are easy: enter information about work done at a customer and get a signature of someone. This will then by e-mailed to both the company and the customer, forming the basis of the bill that needs to be payed at the end. The tablets where chosen easily: iPads where already available. The code will hopefully be build in such way that we can easily expand support to other platforms, but for now we will stick with iPads. They come with a modern browser and support most of the HTML5 features. Important is the touch of course, because we will use that to get a signature.

The input of information is easy: put some input elements in the body and write some PHP script to turn it into an e-mail. We need a signature. It will be the official approval of the customer, and we do not want to put it on paper. HTML5 comes with the canvas element. That allows us to get some drawing input. But this is a side project and I do not want to write my own drawing code if it is not necessary. So I turned to Google. There I found [Signature Pad][2], and let me tell you, using it is easy. With some configuration I managed to get a good size canvas displayed on the iPad. The PHP script transforms it to an image and with [swiftmailer][3] we build a beautiful HTML mail and send it off.

Follow progress on [github][4]!

[1]: http://www.apple.com/ipad-mini/overview/
[2]: http://thomasjbradley.ca/lab/signature-pad/
[3]: http://swiftmailer.org
[4]: https://github.com/overeemm/werkbon.overeemtelecom.nl