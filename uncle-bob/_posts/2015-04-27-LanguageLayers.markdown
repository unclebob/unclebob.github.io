---
layout: post
title: "Language Layers"
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2015/04/27/language-layers.html" />
I'm sitting here, passing time, by playing the old "Lunar Lander" game from 1969.  This game was written by Jim Storer, a high school student.  He wrote it on a PDP-8 in the FOCAL language.  Here's what a sample run looks like:

<img src="/assets/LunarLanderOutput.jpg"/>

And here's the source code in FOCAL:

<img src="/assets/LunarLanderListing.jpg"/>

Jim Storer was a pretty talented High School student.  I mean, look at that code.  He's got some pretty interesting Taylor expressions in there.  

Anyway..., what I'm _actually_ doing is conducting a binary search in order to find the value of `K` that, when used consistently, will land the craft perfectly.  So I've modified the program to accept a single entry, and then apply that entry repeatedly until the craft either lands or crashes.  As I write this I know that the answer lies between `76.40625` and `76.4453125`, and I am trying `76.4257813`.  I'm beginning to think I'll run out of precision before I find the answer.

Meanwhile, it occurred to me that I was running this program on the [PDP-8 emulator](https://github.com/unclebob/PDP8EmulatorIpad) that I wrote in Lua for my iPad.  

So, OK, let's think about this. 
 
 * The iPad has an [A8X](http://en.wikipedia.org/wiki/Apple_A8X) chip, with three cores running at a gigahertz or so. 
 * Lua is written in C compiled down to A8X assembler.
 * My PDP8 Emulator is written in Lua, using the [CODEA](http://twolivesleft.com/Codea/) package from Two Lives Left.  
 * FOCAL was written in the late 1960s in PDP8 assembler.
 * Lunar Lander was written in FOCAL.  

So that's A8X, C, Lua, PDP8, and FOCAL. That's five different languages.  Five different mechanisms for telling a machine what to do; all stacked up on top of each other!

What's up with that?  Why so many languages? In fact, forget the iPad, the PDP-8, C, Lua, and the rest.  Why are there so many languages?

##Why are there so many languages?

Think about it!  How many computer languages can you name?  Here, off the top of my head, let me give you a partial list:

* FORTRAN
* ALGOL
* COBOL
* SNOBOL
* LISP
* BCPL
* B
* C
* SIMULA
* SMALLTALK
* EIFFEL
* C++
* JAVA
* C#
* PYTHON
* RUBY
* LOGO
* LUA
* BASIC
* PL/1
* JAVASCRIPT
* GO
* DART
* PROLOG
* FORTH
* SWIFT
* ML
* OCCAM
* OCAML
* ADA
* ERLANG
* ELIXER
* FOCAL

You can certainly think of others that I've left out.  The question is, why are there so many?

There can really only be one answer to that question.  The reason there are so many computer languages is:

> _We don't like any of them._

Well, maybe that's too strong a statement.  Perhaps what I should say is:

>_We've been to Hollywood<br>We've been to Redwood<br>We've crossed the ocean for the code of gold.<br>We've been in our mind,<br>It's such a fine line<br>That keeps us searching for the code of gold.<br><br>And we're getting old._

OK, perhaps I should speak for myself...  Anyway, didn't you always want to just yell at Neil Young that he ought to just stop complaining, find some nice girl, and settle down with her?  Didn't you want to tell him that the search of the heart of gold was fruitless?  I mean, what would he do with it if he found it?

And what would _we_ do with the perfect language if we found it?  

>_We'd write PDP-8 Emulators and run FOCAL so we could play Lunar Lander written by a High School student in 1969!_

## Here's what I think.  

Get over it.  Stop searching.  There is no perfect language.  We've searched everywhere.  We've looked high and low.  We've looked in and out. 

>_We've looked at languages from both sides now<br>From in and out<br>and still somehow<br>It's languages illusions we recall.<br><br>We really don't know languages....<br>....at all._

Yeah, OK, it's a weird day.  

But, still, the point is this:

>_We don't need another language.<br>We don't need to know the way home.<br>All we want is life beyond<br>S.Q.L._

Yeah, weird day.  

So here's a thought.  Maybe we need to stop writing new languages and just settle down and pick one or two that work really well.  That would make life a lot simpler for us wouldn't it?

And, in case you were wondering, `76.4384461` gives you a pretty good landing at 2.23 MPH.  


