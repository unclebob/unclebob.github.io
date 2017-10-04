---
layout: post
title: Tools are not the Answer
tags: ["Software"]
---
I just finished reading an extremely depressing [article](https://www.theatlantic.com/technology/archive/2017/09/saving-the-world-from-code/540393/) in _The Atlantic_ entitled: _The Coming Software Apocalypse_.  The article does a good job, at first, of describing several terrible software bugs that have harmed, maimed, and killed people.  But then the article veers off in a direction that I found disheartening.

The author of the article interviewed many thought leaders in the industry, but chose only those thought leaders who were inventing new technologies.  Those technologies were things like [Light Table](http://lighttable.com/), [Model Driven Engineering](https://en.wikipedia.org/wiki/Model-driven_engineering), and [TLA+](https://lamport.azurewebsites.net/tla/tla.html).  

I have nothing against tools like this.  I've even contributed money to the Light Table project.  I think that good software tools make it easier to write good software. However, tools are not the answer to the "Apocalypse".  

Nowhere in the article did the author examine the possibility that programmers are generally undisciplined.  The author did not interview software experts like Kent Beck, or Ward Cunningham, or Martin Fowler.  The author completely avoided the notion that the software apocalypse might be avoided if programmers just did a better job.  Instead, the author seemed convinced that the solution to the software apocalypse -- the solution to bad code -- is more code.

I disagree.  Tools are fine; but the solution to the software apocalypse is not more tools. The solution is better programming discipline.

Last night I watched the pilot episode of [_The Wisdom of the Crowd_](http://www.cbs.com/shows/wisdom-of-the-crowd/).  It's a fun new series on CBS.  The hero of the story is the inventor of a huge social network platform whose daughter is murdered.  He leaves everything to invent a new social network called Sophie which uses crowd sourcing to solve crimes.  

They must have a few programmers consulting on the show because there was one scene that made me hang my head in shame.  Someone posts a video of a crime scene on the platform.  It leads the investigators in a certain direction that doesn't pan out.  Later they find that the video was actually much longer and contained clear evidence; but that the Sophie platform had truncated it to 30 seconds causing the investiators to miss that evidence.  

The hero was furious.  Murderers might have escaped.  More people might have died.  So he confronted the lead developer and demanded to know how this could have happened.  He was told that one of the programmers had reused some code from a different platform and had not realized that it had a built-in 30 second truncation.  The hero was livid.  He demanded to know which programmer had been so sloppy.  The lead developer refused to say.  She told him that if he wanted to fire someone, he should fire her.  So the hero backed down.

Later, the guilty programmer thanked the lead developer for protecting him.  He said: 
"I knew I shouldn't have reused that code, but we were in a rush."  She smiled at him and told him not to worry about it.

And right there, ladies and gentlemen, you can see both the cause of the apocalypse, and the obvious solution.  

The cause:

1. Too many programmer take sloppy short-cuts under schedule pressure.
2. Too many other programmers think it's fine, and provide cover.

The obvious solution:

 1. Raise the level of software discipline and professionalism.
 2. Never make excuses for sloppy work.

This is the point that the author of the Atlantic article missed entirely.  The one thing he failed to consider was that the reason we are facing bugs that kill people and lose fortunes, the reason that we are facing a software apocalypse, is that too many programmers think that schedule pressure makes it OK to do a half-assed job.

I found it astounding that in that entire long article, the notion of testing was never examined as a solution -- it was only presented as a forlorn and foolish hope.  At one point he said of bugs:

>_You could do all the testing you wanted and you’d never find them all._

True as this may be, it is not a reason to discard, or reduce, or fail to increase testing as a discipline.

Several of the tools that the author presented were simply glorified REPLs.  They allow the programmers to immediately visualize the results of their code.  

I think that such rapid feedback is wonderful.  I think that rapid feedback of that kind is extraordinarily valuable.  But REPLs don't replace tests.

Whenever I hear that a programmer "tested it in the REPL" I cringe.  You don't _test_ things in the REPL; you _try_ things in the REPL.  A test is much more formal than a trial.  A test can be repeated.  A test can be enhanced.  You can add to a test.  You can review a test.  You can add a test to a larger suite of tests. A test is a document.  A test is a _program_.

Better REPLs are not the answer.  Model Driven Engineering is not the answer.  Tools and platforms are not the answer.  Better languages are not the answer.  Better frameworks are not the answer.

Yes, those things are shiny, and they sparkle and glisten; but...

>_"The fault, dear Brutus, is not in our stars, But in ourselves..."_

I stood before a sea of programmers a few days ago.  I asked them the question I always ask:  _"How many of you write unit tests on a regular basis?"_  Not one in twenty raised their hands.

