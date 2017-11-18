---
layout: post
title: Living on the Plateau
tags: ["Software"]
---
Languages have evolved quite a bit over the years.  The early evolution from machine language to assembler was necessary and obvious.  The evolution from assembler to Fortran and Basic was also necessary and obvious.  We might even say that the evolution of COBOL was predictable, if not entirely necessary. 

Let's look at just one thread through that evolution:

     1946    1949     1958       1960     1966    1967  1970  1972 1980   2009
    Binary -> Asm -> ALGOL58 -> ALGOL60 -> CPL -> BCPL -> B -> C -> C++ -> Go

What drove all these steps Why was Algol58 not good enough?  Or BCPL?  Or B?

One factor, of course, is that we were just starting to learn about computer languages at that time.  It was difficult to separate the language from the architecture of the machine.  You can see the architecture of the hardware peeking out ever more clearly as you go back in time through these languages.  C, as abstract and portable as it was, remained very close to "the metal".  Even Go admits that below all the turtles there's some hardware.  

Another factor -- and probably a much more important factor -- is that the machines were getting exponentially more powerful.  In 1958 computers were slow, fragile, had 16K or less memory, and cost many millions of dollars.  By 1972 you could buy a fast 64K PDP-7 for ~$100,000.  

C held sway for a good long time.  Not because computers weren't getting more powerful; but because they were shrinking.  They were shrinking in both size and cost.  In 1980 you could buy an 8085 microcomputer with 64K of RAM for a few thousand dollars.  And so even though the top end computers were getting more and more powerful; the bottom end machines remained the perfect size for C.  

But through the 80s the power and capacity of the machines continued to grow exponentially.   By 1990, C was just too small a language for the tasks at hand.  We needed something better.  And C++ was waiting in the wings.  

The C++ era was short lived because Java/C# came along in the latter half of the decade.  By this time machines had gotten so vastly powerful that it was possible to implement whole systemes in _virtual_ machines.  Just ten years before: _that_ would have been _unthinkable_.  

But the unthinkability wasn't done with us.  By 2010 PHP, Ruby, and Python -- _interpreted languages_ -- were running some of the biggest, and most profitable, systems on the planet.  

I want you to sit back for a second and contemplate just how _crazy_ this is.  We, who used to worry about microseconds, now cavalierly push systems to production written in languages that execute in textual interpreters!

So what is it that drove all this language evolution?  It should be quite apparent that the current state of our languages can be traced directly to Moore's Law: the fact that computer speed, component density, and memory size _doubled_ every 18 months; while at the same time, size, cost, and power consumption changed just as dramatically in the opposite direction.

The reason we have Clojure, Elixr, Swift, Dart, F#, Scala, and Go today is the sheer raw power of our machines.  It is that power that enables Eclipse, Vim, EMacs, and IntelliJ.  It is that power that allows us to tolerate garbage collection, and to ignore memory, ignore processor speed, ingore efficiency.  It is that power that allows us to behave as though all resources are infinite -- because, for nearly all intents and purposes, _they are_.  

Most of us work in an environment of virtual infinities.  There may be some contraints out there somewhere, but most of us know we will never bump into them.  That's what happens when you ride Moore's trains through twenty-eight doublings.  

But one of those trains reached the plateau and pulled into its final station over a decade ago.  Clock rates doubled from a few kilohertz to 2.8ghz and then, simply, stopped.
 
<img src="/assets/CPU-Scaling.jpg" width="500" align="middle">

There were those amongh us who expected this.  Others found it jarring.  Many people thought we had many more doublings to go.  That's the way it is with limits -- often you don't see them coming until you hit them.

We took some solace in the fact that although clock rates had plateaued, density was still on the rise.  We took even more solace, along with a fair bit of trepidation, from the fact that processor cores were multiplying.  We saw the first dual core machines in the early years of the new millenium.  Quad core machines followed within a few years.  We were all quite sure that the 8, 16, 32, and 64 core machines were on their way.  

Indeed, some of us viewed this as a crisis.  Multiple cores meant multiple threads -- and not the kinds of thread we had been used to.  Multi-processor threads are wild beasts compared to the well behaved threads that are administered by an operating system.  So we looked to functional languages for our salvation.

Functional langauges put extreme discipline on the mutation of state (i.e. assignment operations).  Disciplined use of assignment is the key to dealing with multiple threads.  You can't have concurrent update problems if your threads don't actually update anything.

And so F# and Scala and Clojure and Elixr and even Erlang started to soar in popularity.

But the 8 core processors never came -- at least in the laptop space.  We're still waiting.  And it's looking more and more as though Moore's law for density has hit the wall.

<img src="/assets/Cortex-A17.jpg" width="500" align="middle">

Now perhaps you want to make the argument that the Apple A10X has 6 processing cores and 12 GPU cores.  Yes, I'll admit there is still progress being made in density.  However, that progress appears to be incremental rather than exponential.  

I think it is quite realistic to expect that the speed and density of computers have reached their practical limits.  It's just possible that we have been living on the plateau for the last decade; and that we will remain on this plateau for the foreseeable future.

We knew this was going to happen.  We knew it was just a matter of time.  We had hoped that there might be a few more doublings ahead of us -- but that appears not to be the case.

As a bit of anecdotal evidence for this I submit my lovely wife's Macbook air.  This machine was purchased back in 2013.  It works quite well for her.  Every time I encourage her to buy a newer model she rebuffs me by saying that the machine is quite adequate for her needs.  Who among us would have said such a thing about a 5 year old machine back in the 90s?  Back then 5 years was three doublings!

If we aren't going to see 1024 core processors in the near future, what need have we of functional programming languages?  Oh, don't get me wrong.  I think functional programming is a good idea.  Every programmer should learn it.  But the crisis that we anticipated -- the crisis that drove the current infatuation with functional languages -- seems not to be occurring.  And that may well mean that functional languages never achieve the ascendency that we had anticipated for them.  It may just be that Java/C#, and Ruby/Python, and Swift/Dart/Go are, and will be, sufficient.

If Moore's law was the driver of our language evolution, what will drive it now?  Could it be that we are living at the beginning of a period in which language evolution will slow it's frenetic pace?  Will we see the number of languages cease it's relentless rise, and begin to decline?  Will our industry gradually abandon the exuberance of adolescence and settle down into a stable period of adulthood and middle age?

I, for one, hope so.  I, for one, look forward to the end of the barnstorming era and the onset of the era of professional, and ethical craftsmanship.  An era where we discard the detritus of the excesses of our youth, and settle upon a small complement of languages, platforms, and frameworks, with which to carry out the long work that is ahead of us.

I, for one, having started near the bottom, and having climbed the plateau all the way to it's current dizzying height, look forward to a long and healthy life up here.



