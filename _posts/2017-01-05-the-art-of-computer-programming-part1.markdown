---
layout: posts
title: The Art of Computer Programming - part 1
categories: [taocp]
---

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Goal for 2017: read through this massive work. <a href="https://twitter.com/hashtag/knuth?src=hash">#knuth</a> <a href="https://twitter.com/hashtag/art?src=hash">#art</a> <a href="https://twitter.com/hashtag/computerprogramming?src=hash">#computerprogramming</a> <a href="https://twitter.com/hashtag/taocp?src=hash">#taocp</a> <a href="https://t.co/DGh5XnVg8X">pic.twitter.com/DGh5XnVg8X</a></p>&mdash; Michiel Overeem (@michielovereem) <a href="https://twitter.com/michielovereem/status/811228070740893699">December 20, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I have these books on my shelve since graduation (2007), and a couple of weeks ago I finally set the goal to actually read them. 
It is a challenge for myself to see if I can read all volumes, comprehend them (more or less), and get something useful out them.
So in this serie of blogs I will summarize, share notes, and tell the world about my progress.
I have not set a deadline or a schedule, I am just going with the flow.
And in the spirit of [The Pragmatic Programmer][4] I will use this challenge as an opportunity to learn some F#.
Code snippets and what more can be found in the [github repo][5].

These books are impressive. 
And to be honest a little intimidating. 
I like reading and learning, but I'm not sure if I can follow all this stuff. 
But we will see.

### sections 1.1 and 1.2 (Volume 1 Third Edition)

Section 1.1 is labeled __Algorithms__, and this first section was fun. 
It contained history (which I like), a little background, and some general explanation.
An example of the background: the origin of the word _algorithm_. 
I sure didn't know that its origins are in the name of the Persian author [al-Khwārizmī][1].
An algorithm is a receipe, procedure, a routine. But how to know when something is an algorithm?
Knuth gives the following list:

* Finiteness. An algorithm should terminate in a finite number of steps. 
A finite number of steps could mean a large number of steps, but it should stay reasonable.
* Definiteness. Every step should be precisely defined. 
The definition should have rigour and no ambiguity. 
This is where formally defined languages like programming languages are useful.
* Input. Algorithms have input: zero or more inputs.
* Output. And they have one more outputs.
* Effectiveness. Effective here is defined as using basic operations such that they can be done exactly and in finite time.

What is a good algorithm?
We have to look at elegance, the number of steps, simplicity, and more.

The book gives an example by lookinmg at [Euclid's algorithm][6]: find the greatest common divisor of two integers.

Section 1.2 is __Mathematical Preliminaries__ and for this section I followed the advise of Knuth: skimming these sections and familiarizing myself with the math.
I did not do any exercises.
To be honest, I am more interested in the computer programming parts and less in the math part, so I will regard this as reference material.
The subsections of 1.2 covered among other things numbers, powers, logarithms, sums, products, permutations, factorials, fibonacci, the analysis of algorithms, and asymptotic representations.

As an example of the analysis of an algorithm, the [algorithm for finding the maximum][7] is given.

The last subsection was most interesting as it covered the [big-O notation][2].
And it made my think about another book in my digital shelve: [The Imposter’s Handbook][3].
In this book Rob Conery dives into different computer science topics for those who never studied computer science.
And to be honest, his explanations also make sense for the ones that did study computer science (yep, that's me).
In this case I read Knuth's subsection and after that I read what Rob Conery said about it.
Knuth is pure and comprehensive with all the math, while Rob gives a rather short overview, but they reinforce each other and I am good to go :)

[1]: https://en.wikipedia.org/wiki/Muhammad_ibn_Musa_al-Khwarizmi
[2]: https://en.wikipedia.org/wiki/Big_O_notation
[3]: https://bigmachine.io/products/the-imposters-handbook/
[4]: https://www.amazon.com/Pragmatic-Programmer-Journeyman-Master/dp/020161622X
[5]: https://github.com/overeemm/the-art-of-computer-programming
[6]: https://github.com/overeemm/the-art-of-computer-programming/blob/master/Algorithm1.1E/Program.fs
[7]: https://github.com/overeemm/the-art-of-computer-programming/blob/master/Algorithm1.2.10M/Program.fs