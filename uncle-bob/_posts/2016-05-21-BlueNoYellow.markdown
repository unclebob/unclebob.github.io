---
layout: post
title: Blue. No! Yellow!
tags: ["Software"]
---
>_What language was used to write the very first programs for the very first stored-program computer?_

Binary machine language, of course.

>_Why?_

Well, obviously, because they had no symbolic assembler.  The first programs had to be written in binary.

>_How much easier is it to write programs in Assembler than binary machine language?_

It's _much_ easier.

>_Give me a number.  How many times easier it is?_

Well, gosh, the assembler does all the horrible clerical work for you.  I mean it calculates all the physical addresses.  It constructs all the physical instructions.  It makes sure you don't do things that are physically impossible, like addressing out of range.  And then it creates an easily loadable binary output.

The workload savings is _enormous_.

>_How enormous?  Give me a number._

OK.  So, if I were to write a simple program, like printing the squares of the first 25 integers, in assembler on an old machine like a PDP-8, it would take me about two hours.  If I had to write that same program in binary machine language, it would probably take me double that. 

I say _double_ because I would first write the program in a symbolic syntax on paper; and then I'd do the assembly of the machine language by hand, on paper.  And then I'd have to enter that binary into the computer by hand.  And all that extra work would probably take me just as long as it would take to write the program in the first place.  Maybe longer.

>_Good enough.  So using a symbolic assembler reduces the workload by a factor of two?_

Actually, I think it's a lot more than that.  Squares of integers is a pretty simple program.  Bigger programs are a lot harder to hand-assemble and hand-load.  So I think the workload savings is actually a function of the program size.  For big programs it saves a _lot_ of time.

>_Please explain._

Well, imagine a one-line change to a symbolic assembler program.  That might take me 20 minutes on an old PDP-8 with paper tape.  But if I were hand-assembling, then I'd have to recalculate all the addresses and re-assemble all the instructions by hand.  It would take me hours and hours depending on how big that program was.  Then hand loading it would take even more hours.

I could save some of that time by segmenting the program into modules that were loaded at fixed addresses that had free gaps between them.  I could same a bit more time by writing a smaller program that helped me load the big program.  But the clerical workload would still be very, very high.  

>_OK.  So give me a number.  On average, how much easier is assembler than binary?_

OK.  I guess I'd have to say 10 times easier.

>_So a symbolic assembler allows one programmer to do the work of ten programmers working in binary?_

Yes, that's probably about right.

>_If symbolic assembly reduced the workload by a factor of 10, how much more did Fortran reduce the workload?_

Well, gosh.  If we're talking about the 1950s, Fortran was a pretty simple language back then.  I mean, it was hardly more than a symbolic assembler for symbolic assembly -- if you catch my meaning.  

>_So, does that mean it reduced the workload by another factor of ten?_

Oh, gosh no!  The clerical burden of symbolic assembler was nowhere near that high.  I'd say that Fortran decreased the workload by a smallish factor.  Perhaps 30%.

>_So, 10 Fortran programmers could do the work of 13 assembly programmers?_  

If you want to look at it that way; yes -- that's probably about right.  

>_So how much work does a language like C save over a language like Fortran?_

Well, ok, um, C does save a bit of clerical work over Fortran.  With that old Fortran you had to remember things like line numbers, and the order of common statements.  You also had rampant `goto` statements all over the place.  C is a much nicer language to program in than Fortran 1.  I'd say it might reduce the workload by 20%.  

>_OK.  So 10 C programmers could do the work of 12 Fortran programmers?_

Well, this is all guesswork of course; but I'd say that's a good educated guess.

>_Good.  So, now:  How much did C++ reduce the workload compared to C?_

OK, well, now look.  We're ignoring a much larger effect.

>_Are we?  What?_

The development environments.  I mean, in the 1950s we were using punched cards and paper tapes.  Compiling a simple program took half an hour at least.  And that's only if you could get access to the machine.  But by the late 1980s, when C++ was becoming popular, programmers kept their source code on disks, and could compile a simple program in two or three minutes.  

>_Is that a reduction in workload?  Or is it just a reduction in wait time?_

Ah.  OK.  I see your point.  Yes, back in those days we spent a lot of time waiting for the machine.  

>_So when you give me your workload estimates, please don't consider the wait times.  I'm only interested in the savings of the languages themselves._

OK.  I get it.  So you asked about C++.  Um.  Frankly, I don't think C++ saved an awful lot of workload.  Oh _some_; but not any more than, say 5%.  I mean, the clerical overhead of C just isn't that high, and the comparative savings of C++ is just not that great.

>_If I use your 5% number, then 100 C++ programmers could do the work of 105 C programmers.  Does that sound right?_

Well, yes.  But only for small and intermediate programs.  For _big_ programs C++ provided some extra benefit. 

>_What might that be?_

It's kind of complicated.  But the bottom line is that the object-oriented features of C++, specifically polymorphism, allowed large programs to be separated into independently developable and deployable modules.  And for very large programs that reduces a significant clerical overhead.

>_I need a number._

Well, if you're going to twist my arm... Given the number of truly big programs that were being created in the 80s and 90s, I'd say that, overall, C++ decreased the workload by, um, maybe 7%?

>_You don't seem very confident._

I'm not.  But let's use that number.  7%.  

>_All right.  So then 107 C programmers could do the work of 100 C++ programers?_

Like I said.  Let's use that number.  

>_How much work did Java save over C++?_

Well, ok, um.  _Some_.  I mean, Java is a simpler language.  It has garbage collection.  It doesn't have header files.  It runs on a VM.  There are lots of advantages.  (And a few disadvantages.)

>_The number?_

We're kind if in the mud here.  But since you are pressing me, I'd say that, all else being equal (which it never is), you might get a 5% reduction in workload by using Java over C++.  

>_So, 100 Java programmers could do the work of 105 C++ programmers?_

Well, yeah; but.  No.  That's not right.  The standard deviation is too high.  If you pick 100 Java programers at random and compare them to 105 C++ programmers at random, I can't predict the results.  We need much larger numbers to see the real benefit.

>_How much bigger?_

Two orders of magnitude at least.  

>_So, 10,000 randomly chosen Java programmers could probably do the work of 10,500 randomly chosen C++ programmers?_

I'd take that bet.  

>_Very well.  How much does a language like Ruby reduce the workload over Java?_

Hoo boy!  (sigh).  Really?  Look, Ruby is a really nice language.  It is both simple, and complex; both elegant and quirky.  It's dog slow compared to Java; but computers are just so cheap that...

>_That's not what I'm asking you._

Right.  I know.  OK, so the major workload that Ruby reduces over a language like Java is _Types_.  In Java you have to create a formal structure of types and keep that structure consistent.  In Ruby, you can play pretty fast and loose with the types.

>_That sounds like a big reduction in workload._

Well, no.  You see that's offset by the fact that playing fast and loose with the type structure leads to a class of runtime errors that Java programmers don't experience.  So, Ruby programmers have a bigger test and debug overhead.

>_Are you saying that the effects cancel out?_

That depends on who you ask.

>_I'm asking you._

OK then.  I'd say that the effects do not cancel out.  Ruby reduces the workload over Java.

>_How much?  20%?_

People used to think so.  In fact, in the 90s people thought that Smalltalk programmers were many times more productive than C++ programmers.

>_You are confusing me.  Why mention those languages?_

Well, because C++ is pretty close to Java, and Smalltalk is pretty close to Ruby.  

>_I see.  So then Ruby reduces the workload many times over Java?_

No, probably not.  You see, back in the 90s, that wait-time issue was still quite pronounced.  The compile time for a typical C++ program was many minutes.  The compile time for Smalltalk was, um, _zero_. 

>_Zero?_

Effectively yes.  The problem is that languages like Java and C++ have a lot of work to do to reconcile all the types.  Languages like Smaltalk and Ruby don't bother.  So, back in the 90s, it was minutes to milliseconds.

>_I see.  So this is all just wait-time, and we can ignore it._

Not quite.  You see, when the compile time if effectively _zero_ it encourages a different programming style and discipline.  You can work in _very_ short cycles; _seconds_ as opposed to minutes.  This allows _very_ rapid feedback.  When compile times are long, that rapid feedback isn't possible.

>_Does rapid feedback reduce workload?_

Yes, in a way.  When your cycles are extremely short, the clerical overhead in each cycle is very small.  Your brain has less to keep track of.  Longer cycles increase clerical overhead -- in a non-linear fashion.

>_Non linear?_

Yes, clerical overhead grows out of proportion to the cycle time.  It might be as high as `O(N^2)`.  I don't know.  But I'm quite sure it's not linear.

>_Well, then, Ruby takes the lead by a mile!_

No.  That's the point.  Because our hardware has improved so much in the last twenty years, compile times for Java are effectively _zero_.  The cycle time of a Java programmer is no longer (or _need_ be no longer) than the cycle time of a Ruby programmer.

>_What are you saying?_

I'm saying that programmers who use a short-cycle discipline will see little or no difference in workload between Java and Ruby.  What differences there are will not be enough to measure. 

>_No measurable difference?_

I think to measure a statistical difference you'd need to run trials with thousands of programmers.  

>_But you said before that Ruby reduces the workload over Java._

I think it does; but only if the cycle time is long.  If the edit/compile/test cycle time is kept very short, then the effect is negligible.

>_Zero?_

Well, no, probably more like 5%.  But the standard deviation is enormous.

>_So, it takes 10,500 short-cycle Java programmers to do the work of 10,000 short-cycle Ruby programmers?_

If you add another order of magnitude to the sample size; then I might take that bet.  

>_Are there any languages that can do better than Ruby?_

Oh, you might get another 5% out of a language like Clojure, just because it's so simple, and because it's functional.  

>_You are giving only 5% to a functional language?_

No, I'm saying that a short-cycle discipline virtually erases the productivity differences in modern languages.  

So long as you work in short cycles, it hardly matters what modern language you use.

>_So: Swift? Dart? Go?_

Negligible. 

>_Scala?  F#?_

Negligible.

>_You are saying that we've reached the pinnacle.  That no future language will be better than what we have now._

Not quite.  What I'm saying is that we've passed the point of diminishing returns.  No future language will give us the factor of 10 advantage that assembler gave us over binary.  No future language will give us 50%, or 20%, or even 10% reduction in workload over current languages.  The short-cycle discipline has reduced the differences to virtual immeasurability.

>_So then why are there always new languages being invented?_ 

It's a quest for the _Holy Grail_.

>_Ah, so it's really just a matter of your favorite color._





