---
layout: posts
title: The Art of Computer Programming - part 2 - MMIX and fundamental programming techniques
categories: [taocp]
---

I set out to read [The Art of Computer Programming][1] this year. 
In [these blog posts][2] I will report on the progress.
Mostly for me, but maybe someone finds it fun to read :)

### sections 1.3' (Volume 1 Fascicle 1) - MMIX

The section on MMIX, the language that Knuth uses throughout the book for his code samples.
Although it seems a strange choice, a machine language for samples, I can appreciate the reasons.
It is not that hard, and it shows a lot of how the machine works.

Nice from a theoretical perspective, but to be honest, I skipped/skimmed these parts.
Although I typed [some][3] [programs][4], I didn't put to much effort in this part.

### sections 1.4' (Volume 1 Fascicle 1) and 1.4.4 and 1.4.5 (Volume 1 Third Edition) - Fundamental programming techniques

These sections are a bit more general, and I really liked the ideas in it. 

In 1.4.1' Knuth explains the steps for writing a program with subroutines as follows:

0. Start with the problem: what wil be the plan.
1. Write a top-down version of the solution. First the general flow, while putting details in subroutines (that are not yet implemented). Then the subroutines are filled in. In this step, any language can be used. The interesting idea is that this version is an exploration, and the result is not used except for knowledge discovery.
2. After that initial exploration, a second version can be written. This time in a real programming language, but bottom-up. Write the subroutines, and then the main routine that combines them.
3. The final step is the reexamination of the solution. Depending on the state, return to step 1 or 2 and refine.
4. After the reexamination, it is time to finalize the version by running and inspecting it. Or debugging as Knuth says. This is necessary to observe the behaviour at run-time.

Step 1 reminded my of [Alistair Cockburn's view: design as knowledge acquisition][5].
Choosing something else than a programming language, or maybe a programming language other than your normal language frees you from the feeling that you are writing production code.
It gives opportunity for knowledge acquisition without the burden of quality.

Step 3 is of course non-trivial: when to stop, when is enough enough.
This is something that one can only learn through experience, I guess.

Step 4 might be something that we often forget to do in a controlled fashion.
A run, not just to validate the functionality and correctness, but also a run to validate the performance and other aspects.
Maybe automated tests can improve this: a controlled fashion that is part of source control and can be ran on every build.
This prevents errors that slip with later changes.

Secion 1.4.2' was also interesting, coroutines versus multiple passes.
It reminded me of generators, iterators, reactive programming, etc.
Much to think about!
I might write another blog about that in the future.

[1]: http://www-cs-faculty.stanford.edu/~uno/taocp.html
[2]: http://www.movereem.nl/topic.html#taocp
[3]: https://github.com/overeemm/the-art-of-computer-programming/blob/master/Section1.3/Hello.mms
[4]: https://github.com/overeemm/the-art-of-computer-programming/blob/master/Section1.3/Prime.mms
[5]: http://www.movereem.nl/notes-from-the-class-advanced-agile-developme