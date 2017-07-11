---
layout: post
title: Pragmatic Functional Programming
tags: ["Software"]
---
The move to functional programming began, in earnest, about a decade ago.  We saw languages like Scala, Clojure, and F# start to attract attention. This move was more than just the normal "Oh cool, a new language!" enthusiasm.  There was something real driving it -- or so we thought.

Moore's law told us that the speed of computers would double every 18 months.  This law held true from the 1960s until 2000.  And then it stopped.  Cold.  Clock rates reached 3ghz, and then plateaued.  The speed of light limit had been reached.  Signals could not propagate across the surface of the chip fast enough to allow higher speeds.  

So the hardware designers changed their strategy.  In order to get more throughput, they added more processors (cores).  In order to make room for those cores they removed much of the cacheing and pipelining hardware from the chips.  Thus, the processors were a bit slower than before; but there were more of them.  Throughput increased.

I got my first dual core machine 8 years ago.  Two years later I got a four core machine.  And so the proliferation of the cores had begun.  And we all understood that this would impact software development in ways that we couldn't imagine.

One of our responses was to learn Functional Programming (FP).  FP strongly discourages changing the state of a variable once initialized.  This has a profound effect upon concurrency.  If you can't change the state of a variable, you can't have a race condition.  If you can't update the value of a variable, you can't have a concurrent update problem.  

This, of course, was thought to be the solution to the multi-core problem.  As cores proliferated, concurrency, NAY!, _simultaneity_ would become a significant issue.  FP ought to provide the programming style that would mitigate the problems of dealing with 1024 cores in a single processor.

So everyone started learning Clojure, or Scala, or F#, or Haskell; because they knew the freight train was on the tracks heading for them, and they wanted to be prepared when it arrived.

But the freight train never came.  Six years ago I got a four core laptop.  I've had two more since then.  The next laptop I get looks like it will be a four core laptop too.  Are we seeing another plateau?  

>_As an aside, I watched a movie from 2007 last night.  The heroine was using a laptop, viewing pages on a fancy browser, using google, and getting text messages on her flip phone.  It was all too familiar.  Oh, it was dated -- I could see that the laptop was an older model, that the browser was an older version, and the flip phone was a far cry from the smart phones of today.  Still -- the change wasn't as dramatic as the change from 2000 to 2011 would have been.  And not nearly as dramatic as the change from 1990 - 2000 would have been.  Are we seeing a plateau in the rate of computer and software technology?_

So, perhaps, FP is not as critical a skill as we once thought.  Maybe we aren't going to be inundated with cores.  Maybe we don't have to worry about chips with 32,768 cores on them.  Maybe we can all relax and go back to updating our variables again.

I think that would be a mistake.  A big one.  I think it would be as big a mistake as rampant use of `goto`.  I think it would be as dangerous as abandoning dynamic dispatch.  

Why?  We can start with the reason we got interested in the first place.  FP makes concurrency much safer.  If you are building a system with lots of threads, or processes, then using FP will strongly reduce the issues you might have with race conditions and concurrent updates.  

Why else?  Well, FP is easier to write, easier to read, easier to test, and easier to understand.  Now I imagine that a few of your are waving your hands and shouting at the screen.  You've tried FP and you have found it anything but easy.  All those maps and reduces and all the recursion -- especially the _tail_ recursion are anything but _easy_.  Sure.  I get it.  But that's just a problem with familiarity.  Once you are familiar with those concepts -- and it doesn't take long to develop that familiarity -- programming gets a _lot_ easier.  

Why does it get easier?  Because you don't have to keep track of the state of the system.  The state of variables can't change; so the state of the system remains unaltered.  And it's not just the system that you don't have to keep track of.  You don't need to keep track of the state of a list, or the state of a set, or the state of a stack, or a queue; because these data structures cannot be changed.  When you push an element onto a stack in an FP language, you get a new stack, you don't change the old one.  This means that the programmer has to juggle fewer balls in the air at the same time.  There's less to remember.  Less to keep track of.  And therefore the code is much simpler to write, read, understand, and test.

So what FP language should you use?  My favorite is Clojure.  The reason is that Clojure is absurdly simple.  It's a dialect of Lisp, which is a beautifully simple language.  Here, let me show you.  

Here's a function in Java:  `f(x);`

Now, to turn this into a function in Lisp, you simply move the first parenthesis to the left: `(f x)`.

Now you know 95% of Lisp, and you know 90% of Clojure.  That silly little parentheses syntax really is just about all the syntax there is to these languages.  They are _absurdly_ simple.  

Now I know, maybe you've seen Lisp programs before and you don't like all those parentheses.  And maybe you don't like `CAR` and `CDR` and `CADR`, etc.  Don't worry.  Clojure has a bit more punctuation than Lisp, so there are fewer parentheses.  Clojure also replaced `CAR` and `CDR` and `CADR` with `first` and `rest` and `second`.  What's more, Clojure is built on the JVM, and allows complete access to the full Java library, and any other Java framework or library you care to use.  The interoperability is quick and easy.  And, better still, Clojure allows full access to the OO features of the JVM.

"But wait!" I hear you say.  "FP and OO are mutually incompatible!"  Who told you that?  That's nonsense!  Oh, it's true that in FP you cannot change the state of an object; but so what?  Just as pushing an integer onto a stack gives you a new stack, when you call a method that adjusts the value of an object, you get a new object instead of changing the old one.  This is very easy to deal with, once you get used to it.  

But back to OO.  One of the features of OO that I find most useful, at the level of software architecture, is dynamic polymorphism.  And Clojure provides complete access to the dynamic polymorphism of Java.  Perhaps an example would explain this best.

	(defprotocol Gateway
	  (get-internal-episodes [this])
	  (get-public-episodes [this]))

The code above defines a polymorphic `interface` for the JVM.  In Java this interface would look like this:

	public interface Gateway {
		List<Episode> getInternalEpisodes();
		List<Episode> getPublicEpisodes();
	}

At the JVM level the byte-code produced is identical.  Indeed, a program written in Java would `implement` the interface just as if it was written in Java.  By the same token a Clojure program can implement a Java interface.  In Clojure that looks like this:

	(deftype Gateway-imp [db]
	  Gateway
	  (get-internal-episodes [this]
	    (internal-episodes db))

	  (get-public-episodes [this]
	    (public-episodes db)))

Note the constructor argument `db`, and how all the methods can access it.  In this case the implementations of the interface simply delegate to some local functions, passing the `db` along.

Best of all, perhaps, is the fact that Lisp, and therefore Clojure, is (wait for it) _Homoiconic_, which means that the code is data that the program can manipulate.  This is easy to see.  The following code: `(1 2 3)` represents a list of three integers.  If the first element of a list happens to be a function, as in: `(f 2 3)` then it becomes a function call.  Thus, all function calls in Clojure are lists; and lists can be directly manipulated by the code.  Thus, a program can construct and execute other programs.

The bottom line is this.  Functional programming is important.  You should learn it.  And if you are wondering what language to use to learn it, I suggest Clojure.  





 

