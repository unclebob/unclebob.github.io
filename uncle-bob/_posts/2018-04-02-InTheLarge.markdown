---
layout: post
title: In The Large
tags: ["Software"]
---
From the very first moments of the Agile revolution we pondered the question of _Agile in the Large_.  How could we take the principles of light-weight, iterative, incremental, high-feedback development and apply them to truly _huge_ projects?

At first the answers were things like _Scrum of Scrums_.  The idea was to recursively apply the principles of Agile development at ever higher levels of scale.  A project that required more than one team of 5-12 developers could be built by two such teams with a higher level team to "oversee?" them.

Note the question mark.  As we start to consider large projects, we can't avoid hierarchy; but hierarchy seems anathema to Agile.  After all, Agile is all about egalitarianism.  It is a rejection of command and control.  It is a rejection of plans and schedules and...

>_Oh Bollocks!  It is not!_

Agile was a revolution in the sense of "a turning of the wheel".  In the earliest days of software we wrote code in an agile way.  We wrote little bits, tested them, built them into bigger bits, tested those bits, in a never ending cycle.  If you went back to the late 1960s and watched the way code was written, you'd see bits of Agile peeking out.  

Of course we were greatly hampered by the hardware.  Compiles took hours.   Editing was done on teletypes.  Most programmers back then didn't know how to use a keyboard at all; so they had keypunch operators type in their code for them.  In that kind of environment quick feedback loops were difficult to achieve.  

Even so, we did what we could to shorten those feedback loops.  We wrote in assembler so we could sit at the console and debug by patching our software in octal or hex.  We'd test our code by executing it in a debugger, or even by single stepping the computer.[1]  We got pretty good at this.  It was the only way to get things done in anything like a reasonable amount of time.

But the wheel turned.  We started using languages that were not easy to debug at the console.  We started to write bigger and bigger programs.  To make that work in an environment with such long feedback loops we needed _plans_.  

This is the environment from which waterfall emerged.  When it takes a full day to go around the edit/compile/test loop, lots of planning and checking are necessary.  You can't to TDD and Refactoring in a 24 hour loop!

But the wheel kept turning.  Moore's law put us on an exponential curve that most of today's programmers have no inkling of.  We went from 24 hour turnarounds in 1970, to 60 minute turnarounds in 1980, to ten minute turnarounds in 1990, to ten second turnarounds in 2000.  And by 2005 the turnaround time for most programmers was sub second.

This is the environment in which Agile emerged.  Agile was a return to the quick turnaround, high feedback, development strategies of the 1960s but with much more powerful machines, much better languages and tools, and much larger projects. 

But Agile also emerged from the flames.  Waterfall, though necessary in the 70s and 80s, was painful in the extreme.  We learned a lot, during those decades, about what _not_ to do.  So when Agile emerged in the late 90s it carried with it the lessons learned through those dark times.

Agile, was not simply a return to short feedback cycles.  Agile imposed _disciplines_ on top of those short feedback cycles.  Disciplines like testing, refactoring, pairing, and intense automation.  It's true that Agile was a turning of the wheel; but when wheels turn, they drag the vehicle forward.  Agile definitely moved us forward from the strategies of the '60s.

But forward in what?  What was it that the Agile revolution improved?

Agile was a revolution in how relatively _small_ teams can develop relatively _small_ software projects.  Note the emphasis on the word _small_.  

An Agile team is great at creating a software system of 100,000 lines or so.  And 100,000 lines can do a hell of a lot.  So for many companies one or two Agile teams is sufficient for just about anything they want to do.  

On the other hand, if you need to create a system of ten million lines, a single Agile team isn't going to cut it.  You need about a hundred teams to build a ten million line system.

But how do you manage a hundred agile teams?  How do you feed stories to them.  How do you coordinate the interfaces between them?  How do you segregate those ten million lines of code so that the teams can work independently of each other?  

And how (and this was the real question) do you do that in an "Agile" way?

>_Answer:  You don't!_

Here's the thing.  We, humans, are actually very good at building big projects.  We've known how to do this for a very long time.

<img src="/assets/pyramids.jpg"/>

Just think of the _truly huge_ projects that we, humans, have accomplished.  

 * Apollo: We put men on the moon!
 * D-Day: We invaded Normandy with 156,000 troops along a 50 mile, heavily fortified, border.
 * We have a world economy that supports 8 billion people.
 * The globe is ensconced in a massive digital network allowing you to read this blog on your phone while hiking in the woods!
 * You want a thingamajig?  You can get it delivered tomorrow, or even today, with a few taps on your phone.
 
I don't think I need to go on.  We, humans, are really quite good at doing big things.  It's _who_ we are.  It's _what_ we do.  We put red sports cars into solar orbit.  We do big things.

So why are we worried about big software?  We already know how to build big software.  We've been doing it for 50 years or more.  The "big" part was never actually the problem.  The problem that we solved with Agile was the _small_ part.  What we _didn't_ know how to do well, was build _small_ projects.  

We've always known how to do big projects.  You do big projects by breaking them up into a bunch of _small_ projects.  Agile solved the _small_ part of that.  Agile really has nothing to do with the big part.

But, but, but, but...  Egalitarianism!  The rejection of plans and of Command and Control!  Agile!

>_Bollocks!_

Agile was never about egalitarianism.  Agile was never about the rejection of plans, or  the rejection of command and control.  Indeed, Agile was the _embodiment_ of the lowest level unit of command and control: _The squad_.

Yes, at some level down the hierarchy command and control stops being effective.  A small squad of individuals can work together in short cycles with lots of feedback and intense communication to achieve an objective.  That's an Agile team.  At that level strict command and control is intensely harmful.  But above that level command and control begin to become necessary.  The higher you go, the more concrete and obvious that becomes.  You don't design, build, produce and sell hundreds of millions of iPhones without an awful lot of command and control.

There are quite a few Agile in the Large strategies out there.  Books and blogs have been written about the topic.  Whole consulting companies have been created to transition companies to use Agile in the Large approaches.  There is nothing wrong with this. 

There is nothing wrong with the strategies and techniques described by these Agile in the Large approaches.  Except one thing.  They aren't Agile.  They have nothing to do with Agile.  Rather, they are Agile "flavored" variations on the strategies and techniques that humans have been using for millennia to get big things done.

The flavor comes from the use vocabulary and concepts from Agile.  There's nothing wrong with that flavor -- it's fine.  If you find that it helps to use the words and concepts from Agile, go for the flavor.  But don't be overly concerned with the actual "Agile-ness" of it.  Once you are doing something big, you have left the realm of Agile.  Hopefully your development teams are using Agile disciplines; but the overall project is not Agile.

Because Agile is about the _small_ things.  







----
[1] In those days, computers had switches on the front panel that allowed you to stop execution, and then step through a program one instruction at a time.  The front panel had lights that showed the contents of the various registers.  This allowed you to watch what was happening as your program executed.  

Look at the switches at the bottom right in the image below.  You'll see _Sing Step_ and _Sing Inst_.  A single _step_ was one cycle of the clock.  An instruction often took several clock cycles.  So you could literally watch the internals of how the instructions were executed.

<img src="/assets/PDP8I_FrontPanel.jpg"/>








