---
layout: post
title: Why Clojure?
tags: ["Software"]
---
I've programmed systems in many different languages; from assembler to Java.  I've written programs in binary machine language.  I've written applications in Fortran, COBOL, PL/1, C, Pascal, C++, Java, Lua, Smalltalk, Logo, and dozens of other languages.  I've used statically typed languages, with lots of type inference.  I've used typeless languages.  I've used dynamically typed languages.  I've used stack based languages like Forth, and logic based languages like Prolog.  

Over the last 5 decades, I've used a LOT of different languages.  

And I've come to a conclusion.  

My favorite language of all, the language that I think will outlast all the others, the language that I believe will eventually become the standard language that all programmers use...

...is Lisp.

I have not come to this conclusion casually, nor even willingly.  I was not fan of Lisp.  For 40 years I was not a fan of Lisp.  I saw the `CAR`s and `CDR`s and `CADDADDR`s and thought it was all just academic baloney; interesting but not truly useful.

And then, a decade ago I found [SICP](https://mitpress.mit.edu/sites/default/files/sicp/index.html).  And after that I found Clojure.  Clojure is a Lisp that rides on top of the Java ecosystem (and does not have `CAR`s or `CDR`s, or `CADADAADR` s).  

I wasn't convinced right away.  It took a few years.  But after the usual stumbling around and frustration, I began to realize that this language was the easiest, most elegant, least imposing language I had ever used -- and not by a small margin.

So, why Clojure?  I've made a list.  Are you ready for it?  Here it is.

1. Economy of Expression.

If you are wondering where the rest of the list is, there isn't any more.  That's the reason.  There's only one.  It is just simpler, and easier, and less occluding to write expressive code in Clojure.  It requires fewer lines.  It require fewer characters.  It require fewer hours.  It requires fewer mental gymnastics. 

Why should this be so?  The answer is very simple.  Indeed the answer is: Very simple.  The language has almost no syntax or grammar.

Let me say that again:

> _The language has almost no syntax or grammar._

Probably the best way to elucidate this point is to show you an example.  So, here, for your entertainment, is the program that prints the first 25 squares of integers.

	(println (take 25 (map #(* % %) (range))))
	
Let's walk through the syntax here.

* `(` means: begin a list.
* `)` means: end the innermost open list.
* _names_ are just names, in this case all are functions.
* `*` is the name of the multiply function.
* `#` means: interpret the next list as a function.
* `%` means: the first argument of that function.

You've just seen 80% or so of the syntax of Clojure.  But before I show you any more, let's just walk through the code above.  

First of all there is that opening `(`.  That means everything that follows until the final `)` is a list.  In most contexts Clojure interprets lists as function calls.  In this case the function is `println`.  That's just the regular old `System.out.println` we're used to in Java -- sort of. 

What are we passing to `println`?  We're passing the result of the `take` function.  The `take` function expects two arguments.  The first, `25` is the number of items to take.  The second argument is a list.  In this case the `take` function will return a list with the first 25 items of the list in the second argument.  

What is the list in the second argument?  It's the result of calling the `map` function.  The `map` function expects two arguments.  The first is a function, and the second is a list.  The `map` function will return a list that is the result of calling the passed-in function on every element of the passed-in list.  

What function is passed-in to `map`?  It's the anonymous function created by `#`, which simply calls multiply on it's duplicated first argument `%`.  

What list is passed into `map`?  It is the list returned by calling the `range` function.  The `range` function simply returns a list of "all" non-negative integers.  That list is _lazy_, so only the integers required by the upstream functions will actually be generated.

Some folks don't like the complication of the `%` syntax, so we can create a `square` function as follows:

	(defn square [x] (* x x))
	(println (take 25 (map square (range))))

The `defn` "function"[1] defines a new function named `square`.  The brackets `[]` work just like parentheses, except that they define a different kind of "list" called a vector.  Lists have the runtime complexity of linked lists.  Vectors have the runtime complexity of arrays.  Sort of.  Anyway, in this case, the vector tells `defn` that the `square` function requires one argument named `x`.  The rest you should be able to infer.

This makes the second line a little nicer.  The `map` function simply invokes `square`.

Now you've seen about 90% of the syntax of Clojure.	

Now let's compare that to the equivalent Java program:

	public class SquaresOfIntegers {
	  public static void main(String[] args) {
	    for (int i=0; i<25; i++)
	      System.out.println(i*i);
	  }
	}

This is considerably more wordy, even if you don't count the enclosing class.  More to the point, however, is that this covers, perhaps 5% of the syntax of Java.  And don't get me started in a comparison with C++.  

Now I don't want to belabor the point.  I could go on with comparison after comparison with language after language.  The bottom line is that Clojure has a much smaller syntax than most languages.  That minimal syntax means that I can express problems clearly, and directly, with much less effort and contortion than most other languages.  

Look.  I was not an easy sell.  I was (am) a C++ programmer. More than that, I was (am no longer) a C++ language lawyer.  I revelled in the heavyweight syntax of the language.  I was enthralled by all its lovely "fidelty bits". I found the transition to Java, twenty years ago, to be rather "meh".  It was just a slimmed down C++ (it's gotten a bit more corpulent since).  

But my transition to Clojure was an eye-opener.  Based on the lightweight syntax I expected it to be suitable for a few classroom exercises, but not for building large systems.  In my mind, large systems equated to large syntax.  Boy, was I wrong.  

What I found, instead, was that the minimal syntax of Clojure is _far more_ conducive to building large systems, than the heavier syntax of Java or C++.  In fact, it's no contest.  Building large systems is Clojure is just simpler and easier than in any other language I've used.  

And as I pointed out at the beginning, I've used a lot of languages.

## But What About...?

So you've probably got some complaints, questions, objections, etc.  Let me see if I can anticipate them.

### _OMG!_ All Those Parentheses

How old are you...?  Look, here's a java function call: `f(x)`.  Now here's the corresponding function call in Clojure: `(f x)`.  Do you see any extra parentheses in there?

OK, that's not entirely fair.  We do wind up with a few more parentheses, but thats just because we tend to nest function calls.  Look at that squares of integers program above and you'll see why.  Don't fret though, if you really don't like that nested syntax, you can always use the threading macros (Let the reader understand).  

### But isn't it slow?

No.  Clojure is not slow.  Oh, look, it's not C.  It's not assembler.  If nanoseconds are your concern than you probably don't want Clojure in your innermost loops.  You also probably don't want Java, or C#.  But 99.9% of the software we write nowadays has no need of nanosecond performance.  I've built a real time, GUI based, animated [space war](https://github.com/unclebob/spacewar) game using Clojure.  I could keep the frame rates up in the high 20s even with hundreds of objects on the screen.  Clojure is not slow.

### But what about Javascript?

ClojureScript compiles right down to Javascript and runs in the browser just fine.  Indeed, that space war program I mentioned above was compiled using ClojureScript and ran in the Browser at even higher frame-rates than in native mode (I'm still trying to figure _that_ out.)

### But it's dynamically typed!

You do write tests, don't you? And as part of those tests you can employ the `clojure/spec` library to specify the schema of your types, and do dynamic checking with pre-conditions and post-conditions (Design by Contract style) to your heart's delight.

### But it's dynamically typed[2]!!

Declaring types requires syntax.  Syntax reduces economy of expression. (Incomming!)

### But, _Dammit_, it's dynamically typed!!!

OK, I get it. You like static typing.  Fine.  You use a nice statically typed language, and I'll use Clojure.  And I'll be in Scotland before ye.

### What about IDEs?

IntelliJ with the Cursive plugin does very nicely.  Most Clojure programmers use Emacs though.

### What about refactoring?

IntelliJ with Cursive has some nice refactorings; though they don't have "Extract Method" yet (Come on guys!)

### What about Java interop?

No brainer.  Clojure programs can call Java directly.  Java programs can call Clojure programs with only a little bit of fuss.  Interop should not be your concern.

### Where do I find Clojure programmers?

They're out there; but you are better off making them.  The syntax is trivial.  The Java platform you likely already know.  Just decide to build your next system in Clojure.  Spend a week or two getting used to the language.  Then start.  Oh, by the time you're a couple of months in you'll realize how primitive your early stuff was, and you'll be tempted to clean it up.  But what else is new?

### But what about that new language over there... What's is called?  Um.  Gazoo?

Yes, there are new languages every month.  Every week.  Every day.  We have no lack of new languages.  The thing about those new languages is that there's nothing new in any of them.  They're just made up of the pieces of old languages that they cut up and shook in a jar and then dumped out in a game of language Yahtzee.  

OK, that's not entirely fair.  There are still some good ideas percolating in the language space.  But none of those ideas are revolutionary.  We've entered the era of "tweaking".  I don't find any of the new languages nearly as compelling as Clojure, simply because of the extra syntax virtually all of them carry.

### Other languages have had minimal syntax.  Why not one of them?

Yes, that's true.  Forth had a tiny syntax.  So did Smalltalk.  But those two languages had baggage of their own.  Forth was a reverse polish stack language.  Economy of expression is not the phrase I would use for Forth.  Though I find it fascinating that PostScript was based on it.

Smalltalk was small and elegant and beautiful.  It spawned the Design Patterns revolution.  It spawned the Refactoring revolution.  It spawned the TDD revolution.  It helped to spawn the Agile revolution.  Smalltalk was a language of tremendous impact.  

Smalltalk was also an image based language.  Very few programmers have ever wrapped their minds around what that really meant.  So, unfortunately, the language languished compared to all the text-file based languages.  

Lisp is older, by far, than Smalltalk or Forth.  It was created in 1957 from concepts that came out of the '30s) and it has never languished in the way Smalltalk and Forth have.  Indeed, it's the language that _refuses_ to die.  We've tried to kill it many times.  But like the annoying neighborhood stray cat it _keeps ... coming ... back_.

Finally, Lisp is functional.  And the future is looking very functional to me.

----
[1] `defn` is not actually a function; but you didn't need to know that.
