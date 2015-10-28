---
layout: post
title: "Does Organization Matter?"
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2015/04/15/does-organization-matter.html" />
>  _I wonder if it's even possible to get all three of readability, hackability and abstraction._ 
>   _--  Ben Kuhn. from [Readability, hackability, and abstraction](http://www.benkuhn.net/rha)_

The short answer, Ben, is: No.  It's _not_ possible.  But then... I'm not answering the question you _wrote_.  I'm answering the question you _actually asked_.  That question is:

>_I wonder if it's even possible to make a complicated program as readable and changeable as a simple program._

And there, of course, the answer is: _No!_  

So let's get in to why you asked the question; and why you asked it in that particular way.  

You noted that simple programs are easy to read, and easy to change.  That's perfectly true.  This program is easy to read, and easy to change:

	void dial(modem, pno, quiet) {
		if (quiet) {
			modem.setVolume(0);
		} else {
			modem.setVolume(10);
		}
		modem.dial(pno);
	}

Or is it?  

If the whole program is twenty lines long, and you can see all the interactions of all the variables then, yes, this is easy to read and easy to change.  

But what if that code up there was part of a ten thousand line program?  What are the implications of that `if` statement?  Will any other function be confused by the fact that the `dial` function changed the `volume` of the `modem`?

As programs grow, parts start to depend on other parts.  The connections become ever more tangled.  And the program begins to turn into _a system_.  And _a system_ is always harder to understand, harder to read, and harder to change, than a program. 

But you were actually asking a very different question.  You were asking whether breaking big functions into small functions, eliminating redundancy, and partitioning the application into polymorphic objects really made things easier to read and change.   You were asking whether it would just be better to leave everything duplicated, in big functions, without any polymorphism.  And the reason you asked this was because you had seen small programs that had some duplications, some big-ish functions, and no polymorphism that _were_ easy to read and change.

The question you are asking is an essential one.  The question you are asking is whether organization matters.

To answer that, let me show you a picture of my desk:

<img src="/assets/UB_DESK.jpg"/>

As you can see it's not real well organized.  Oh, there's _some_ organization.  But there's a lot of chaos too.  Does this mean that organization doesn't matter?

Well, the thing about my desk is that it's relatively simple.  There is a pile on my right.  There is another pile on my left.  The pile on my right has books I'm currently reading or referring to.  The pile on my left is mostly crap that I don't want to think about today, but that I'll have to think about tomorrow.  Neither pile is particularly large.  A quick linear search through either pile gets me what I need.  The dependencies are limited.  

In other words, my desk is somewhat unorganized, but it's relatively simple.  And that's the key here.  Relatively simple things can tolerate a certain level of disorganization.  However, as complexity increases, disorganization becomes suicidal.  Consider trying to find a book in here:

<img src="/assets/badLibrary.jpg"/>

Don't you think it would be easier to find it here?

<img src="/assets/goodLibrary.jpg"/>

A _lot_ of effort went into organizing that second library.  And consequently, it requires a fair bit of effort to learn the organization scheme.  A newbie can't just walk into that well organized library and go right to the book they want.  Instead, the newbie is going to have to learn a bit about the [Dewy Decimal System](http://en.wikipedia.org/wiki/Dewey_Decimal_Classification), and about how to use a card catalog, or the automated index system.  It will require a bit of study and thought before the newbie can find the book they want.  

But ask one of the librarians to find a book, and they'll typically have it in their hands in a matter of seconds!  

And so this gets to the crux of the question that you were really asking.  You were asking whether the time required to learn the organization scheme of the system is worth the bother.  Learning that organization scheme is _hard_.  Becoming proficient at reading and changing the code within that scheme take time, effort, and practice.  And that can feel like a waste when you compare it to how simple life was when you only had 100 lines of code.

And, then, there's another problem.  

The organization structure that works for a thousand line program, does not work for a ten thousand line program.  And the organization structure that works for a ten thousand line program does not work for a hundred thousand line program.  

This almost feels intolerable.  Because as the program grows you must invest time, effort, and practice into an organization scheme that is bound to become obsolete.  

And so the question you are asking is whether it is worthwhile to invest in _any_ organization scheme given that they'll all become obsolete one day.

The answer to that question should be obvious.  If you decide that organizing your system isn't worth the effort, you'll wind up as a [Code Hoarder](http://blog.cleancoder.com/uncle-bob/2014/04/03/Code-Hoarders.html).  





	

