---
layout: post
title: "When Should You Think?"
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="0; url=http://blog.8thlight.com/uncle-bob/2014/03/11/when-to-think.html" />
In the last few weeks there have been a spate of blogs, from various sources, that have suggested that TDD could be done better if people would just think before they code.  These blogs have suggested that some people leap into code too quickly, and would be better served by thinking about the problem first.  

In some ways these blogs remind me of Rich Hickey's now famous talk on [Hammock Driven Developmen](https://gibbon.co/wunki/clojures-best-presentations/hammock-driven-development-rich-hickey); which I enthusiastically support.  

Of course I completely agree that you should think before you code.  It is _never_ a bad idea to think through a problem or how to solve it.  Please, please, _think_.  Lay in the Hammock.  Take some time.  Think.  Think.  Think.

My problem with the recent blogs is that some readers may infer two things that are, in my view, incorrect.  

1. TDD means don't think ahead of time.  
2. Thinking ahead of time is better than thinking at any other time.

The first point is a meme that has made the rounds many times; and is often mentioned by both critics and fanatics of TDD.  It is, however, patently false.  Forethought is _in no way_ excluded by the rules of TDD.  I, as an avid TDDer, _strongly_ encourage you to think ahead.

Let me state that even more forcefully.  If you want to draw UML diagrams because they help you think, _then draw the diagrams_!  If you want to sketch out your thoughts about a problem-solution pair, you should _of course_ do so.  You should take _every_ opportunity to think.  Coding is not the only, nor always the best, way to think.

The second point is also false, and this is critically important to understand.  Your early thoughts are not better than your latter thoughts!  Indeed, quite to the contrary, your latter thoughts are almost always better.  

What this means is that no matter how much effort you put into thinking ahead, once you start to code, you'll have better thoughts.  While coding you'll very probably discover things your forethoughts missed.  Indeed, you may even discover that your some of your forethoughts were just plain wrong. 

It has happened to me more than once, while coding, that I've found my forethoughts to be _completely_ wrong.  In those instances I've had to throw away my forethoughts and start over.  

This happens because thinking without coding involves inadequate negative feedback. There are no reliable tests you can run that can tell you if your thinking is staying close to reality. Without that negative feedback, it's hard to know if you're thoughts are practical, or if they've gone off the rails into La La land.   

Having been to La La land a few times in my career, I've learned a healthy distrust of too much forethought.  So, nowadays, I bring my forethoughts back to ground by keeping the forethoughts relatively short, and driving them to code before they can get too crazy.

If I keep my fore-thinking episodes short I find that they usually send me in a direction that, while imperfect, still leads to a successful outcome.  The outcome doesn't often look a lot like the forethoughts; but the forethoughts are a step in the right direction.

So, should you think ahead?  Of course; but don't give those forethoughts special status or special trust.  In fact, I think healthy skepticism is the best way to treat them.  After all, those forethoughts are the _least_ reliable thoughts you'll have. The _most_ reliable thoughts you'll have will come long after you are done with the project.

So, yes, think before you code.  Then think _as_ you code.  Then think after you code. 

Or, to be terse, just:  *THINK!*  -- Because _there is no preferred time to think!_
