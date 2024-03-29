---
layout: posts
title: My photo library over the years
categories: [photo's]
---
After reading [Hanselmans blog][1] I became curious. How does my photo library look?
 
The journey, or better adventure, started with seeing if I could get those statistics from my library. I use iPhoto to manage all my photo's, but it doesn't come with these kind of statistics. I does have some .xml file, so I thought lets use XSLT. But no, the necessary information (like resolution) is not in this .xml file.
Next step was to do it myself. So after copying everything to a Windows environment (yes, I do use Windows and I like Visual Studio and C#) I started to do some gathering. The opensource library [jpegdata][2] came to the rescue.
 
But then you also want to view charts, right? Nobody wants to look at some numbers. So I dove into [google charts][3] to generate some nice pictures. And the result is:
 
![Photo's per year](/images/photosperyear.png)

![Photo's per resolution](/images/photosperresolution.png) 
 
![Average resolution per year](/images/averageresolutionperyear.png)  
 
So what does this tell me? 

+ I don't invest a lot in camera's (I often make photo's with my iPhone), so the resolution does not increase a lot.
+ I probably need to clean up my library, because there was a lot of garbage in it :-)
 
Again, not very interesting, but a fun exercise.

[1]: http://www.hanselman.com/blog/ReviewingADecadeOfDigitalLifeTheSizeAndTheDirectionOfPersonalMedia.aspx
[2]: http://jpegdata.codeplex.com/
[3]: http://code.google.com/apis/chart/