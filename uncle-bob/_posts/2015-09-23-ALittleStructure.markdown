---
layout: post
title: "A Little Structure"
tags: ["Craftsmanship"]
---
What is _Structured Programming_?

>_Ummm?...  Wasn't it some ancient history having to do with `GOTO`?_ 

Ancient.  Hmmm.  Yes, I guess some might consider 1968 to be ancient.  But can you tell me what Structured Programming is?

>_It was a rule that said not to use `GOTO` statements._

Why do you keep using the past tense?

>_Because nobody cares about Structured Programming anymore._ 

They don't?

>_No, I mean, hardly anybody knows what it is; except that it's got something to do with not using `GOTO`._

Do you use `GOTO`?

>_Of course not!  I mean, well...  Hardly ever._

Why not?

>_Well, mostly because the languages I use don't have `GOTO`._

Why do you suppose that is?

>_Because you don't really need it._

How do you know you don't need `GOTO`?

>_Well...  I haven't had to use it ...   much._

Have you ever heard of Corrado Bohm or Giuseppe Jacopini?

>_Who?_

Corrado Bohm and Giuseppe Jacopini.  In 1966 they wrote a [paper](https://en.wikipedia.org/wiki/Structured_program_theorem) that mathematically proved that `GOTO` was not necessary.  

>_Huh.  That's cool...  I guess._

Actually, yes, it's very cool.  Because, you see, in 1966 the `GOTO` statement was the primary means by which programmers connected their programs together.

>_Really?_

Yes.  For example, here's an `if` statement in _FORTRAN_:

	IF (A-10) 22,33,44
	
>_That looks primitive.  What does it mean?_

It means, if the value of the variable `A` minus 10 is negative, `GOTO` statement 22.  If zero, `GOTO` statement 33.  Otherwise `GOTO` statement 44.

>_Wow!  That's kinda gnarly.  So, like, how did you use that?_

So in Java I might say:

	if (a>10)
	  b++;
	else
	  b--;

In _FORTRAN_ that would be:

		IF (A-10) 20,20,30
	20	B = B - 1
		GOTO 40
	30	B = B + 1
	40	...

>_Yuk!  Yuk!  That's awful._

That's what we were used to.  We'd never even thought it could be different.  

>_And so then those two guys, Bohm and Jacowhatsit..._

Bohm and Jocobini.

>_Yeah, they wrote their paper and everybody stopped using `GOTO`._

No, not quite.  In fact, not at all.  You see their paper was a pretty technical mathematical proof, so hardly anybody read it.  

>_Heh heh, yeah, I get that.  But somebody must have..._

Oh yes.  Several.  But most notably a man named [Edsger Dijkstra](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra).

>_Dije...  DIYGE.._

You pronounce his last name: DIKEstruh.  In March of 1968 he wrote a [letter](http://homepages.cwi.nl/~storm/teaching/reader/Dijkstra68.pdf) to the ACM.

>_A letter?  To who?_

Yes, a very short note.  It was written to the editors of a magazine called _The Communications of the ACM_. He titled it _Go To Statement Considered Harmful_.

>_What did the letter say?  Did it convince everybody?_

No, it really didn't.  Oh, some people saw the logic right away.  Others were -- um -- skeptical -- for a long time.  

>_So what did the letter say?_

Well, you should read it.  It's pretty short.  But I'll give you the gist.  

He made the case that you could restrict your program to three different control structures:  Sequence, Selection, and Iteration.

>_OK, so -- Huh?_

Sequence is when two statements follow each other in sequence like this:

	doStepOne();
	doStepTwo();
	
Those statements might be simple assignments, or procedure calls, or any other kind of valid statement.  They are executed in sequence.  Right?

>_OK, Sure.  So then...  what's the next one?_

Selection.  One of two statements will be executed based on some boolean value.  Like this:

	if (someBooleanValue())
		doThisStep();
	else
		doOtherStep();

>_Yeah, OK.  So then... that last one..._

Iteration.  A statement can be repeated until a boolean value becomes false.  Like this:

	while(someBooleanValue())
	 	doThisStep();

>_Yeah, so, sure.  That's how we write code nowadays.  But you said people didn't buy into this right away?_

No, they didn't.  Dijkstra argued that if you restricted yourself to those three structures then...

>_Oh!  Structures.  Structured Programming.  I get it!_

Um.  Yes.  That's right.  So, if you restrict yourself to those three, um, structures, then you can easily reason about your code.  But if you use unrestricted `GOTO` then you can't.  

>_Wait.  What?  Whaddya mean, reason?_

Well, Dijkstra's argument was that a structured program can be easily analyzed because the state of the system at any line of code, depends only on the boolean values being tested by _selection_ and _iteration_, and the list of calling procedures on the stack.  

>_Um. sure.  Whatever._

(Sigh.)  Look, just read his paper, he makes it pretty clear.  

>_OK, well, so then what happened.  I mean, how did people become convinced?_

Well, in 1972, Dijkstra wrote a book with [O. J. Dahl](https://en.wikipedia.org/wiki/Ole-Johan_Dahl), and [C. A. R. Hoare](https://en.wikipedia.org/wiki/Tony_Hoare).  It was called [Structured Programming](http://www.amazon.com/Structured-Programming-P-I-C-studies-processing/dp/0122005503).   

>_Oh!  So that's what convinced everybody._

Well, no.  Though it did -- uh -- _elevate_ the controversy.

>_You mean like you guys were having flame wars over this?_

No, we didn't have Facebook.  We didn't even have the internet.  But we could write letters to the editors of the various trade journals.  And, let me tell you, some of those letters were _scathing_.  

>_Ha ha.  Sort of like snail mail flames._ 

Indeed.  The more things change, the more they stay the same.

Anyway, the good thing was that the book got lots of people talking, and trying things out, and even convinced some people.

>_But not everyone._

No, not everyone.  Many people continued to hold on to their `GOTO` statements; and would not give them up.

>_So then when did that end?_

It ended when people stopped making and using languages that had `GOTO` statements, and started using languages that didn't.  

>_You mean like Java?_

Yes.  Like Java.  Nowadays the majority of programmers use a language that has no `GOTO`.  And an even larger majority avoid using `GOTO` even if their language has one.  So, for the most part, Dijsktra's war has been won.  Structured Programming is the norm today.

>_Wow! So, Hurray for Dijsktra for giving us this new technology...  back in the olden days..._

New Technology?  No, no, you misunderstand.

>_Why?  I mean, this structured programming thingie was like his invention, right?_

Oh, no.  He didn't invent anything.  What he did was to identify something we _shouldn't do_.  That's not a technology.  That's a _discipline_.

>_Huh? I thought Structured Programming made things better._

Oh, it did.  But not by giving us some new tools or technologies.  It made things better by taking away a damaging tool.  

>_Hmmm.  OK.  Yeah, I guess that's right.  He took `GOTO` away from us._

It might be better to say that _Structured Programming imposes discipline upon direct transfer of control._

>_That sound like gobeltygoop._

Yes, I suppose it does.

