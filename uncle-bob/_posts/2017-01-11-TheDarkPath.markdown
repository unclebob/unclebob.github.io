---
layout: post
title: The Dark Path
tags: ["Software"]
---
Over the last few months I've dabbled in two new languages.  [Swift](https://swift.org/) and [Kotlin](https://kotlinlang.org/).  These two languages have a number of similarities.  Indeed, the similarities are so stark that I wonder if this isn't a new trend in our language [churn](http://blog.cleancoder.com/uncle-bob/2016/07/27/TheChurn.html).  If so, _it is a dark path_.

Both languages have integrated some functional characteristics.  For example, they both have lambdas.  This is a good thing, in general.  The more we learn about functional programming, the better.  These languages are both a far cry from a truly functional programming language; but every step in that direction is a good step.  

My problem is that both languages have doubled down on strong static typing.  Both seem to be intent on closing _every single type hole_ in their parent languages.  In the case of Swift, the parent language is the bizarre typeless hybrid of C and Smalltalk called _Objective-C_; so perhaps the emphasis on typing is understandable.  In the case of Kotlin the parent is the already rather strongly typed Java. 

Now I don't want you to think that I'm opposed to statically typed languages.  I'm not.  There are definite advantages to both dynamic and static languages; and I happily use both kinds.   I have a slight preference for dynamic typing; and so I use Clojure quite a bit.  On the other hand, I probably write more Java than Clojure.  So you can consider me bi-typical.  I walk on both sides of the street -- so to speak.

It's not the fact that Swift and Kotlin are statically typed that has me concerned.  Rather, it is the _depth_ of that static typing.  

I would not call Java a strongly opinionated language when it comes to static typing.  You can create structures in Java that follow the type rules nicely; but you can also violate many of the type rules whenever you want or need to.  The language complains a bit when you do; and throws up a few roadblocks; but not so many as to be obstructionist.  

Swift and Kotlin, on the other hand, are completely inflexible when it comes to their type rules.  For example, in Swift, if you declare a function to throw an `exception`, then _by God_ every call to that function, _all the way up the stack_, must be adorned with a `do-try` block, or a `try!`, or a `try?`.  There is no way, in this language, to silently throw an exception all the way to the top level; without paving a super-hiway for it up through the entire calling tree.  (You can watch Justin and I struggle with this in our [Mobile Application Case Study](https://cleancoders.com/videos/mobile-app-case-study) videos.)   

Now, perhaps you think this is a good thing.  Perhaps you think that there have been a lot of bugs in systems that have resulted from un-corralled exceptions.  Perhaps you think that exceptions that aren't escorted, step by step, up the calling stack are risky and error prone.  And, of course, you would be right about that.  Undeclared and unmanaged exceptions are very risky.

The question is: Whose job is it to manage that risk?  Is it the language's job?  Or is it the programmer's job?

In Kotlin, you cannot derive from a class, or override a function, unless you adorn that class or function as `open`.  You also cannot override a function unless the overriding function is adorned with `override`.  If you neglect to adorn a class with `open`, the language will not allow you to derive from it.

Now, perhaps you think this is a good thing.  Perhaps you believe that inheritance and derivation hierarchies that are allowed to grow without bound are a source of error and risk.  Perhaps you think we can eliminate whole classes of bugs by forcing programmers to explicitly declare their classes to be `open`.  And you may be right.  Derivation and inheritance _are_ risky things.  Lots can go wrong when you override a function in a derived class.

The question is: Whose job is it to manage that risk?  Is it the language's job?  Or is it the programmer's job.

Both Swift and Kotlin have incorporated the concept of _nullable_ types.  The fact that a variable can contain a `null` becomes part of the _type_ of that variable.  A variable of type `String` cannot contain a `null`; it can only contain a reified `String`.  On the other hand, a variable of type `String?` has a _nullable_ type and _can_ contain a `null`.

The rules of the language insist that when you use a nullable variable, you _must_ first check that variable for `null`.  So if `s` is a `String?` then `var l = s.length()` won't compile.  Instead you have to say `var l = s.length() ?: 0` or `var l = if (s!=null) s.length() else 0`.

Perhaps you think this is a good thing.  Perhaps you have seen enough `NPE`s in your lifetime.  Perhaps you know, beyond a shadow of a doubt, that unchecked `null`s are the cause of billions and billions of dollars of software failures.  (Indeed, the Kotlin documentation calls the `NPE` the _"Billion Dollar Bug"_).  And, of course, you are right.  It is very risky to have `null`s rampaging around the system out of control.

The question is: Whose job is it to manage the `null`s.  The language?  Or the programmer?

These languages are like the little Dutch boy sticking his fingers in the dike.  Every time there's a new kind of bug, we add a language feature to prevent that kind of bug.  And so these languages accumulate more and more fingers in holes in dikes.  The problem is, eventually you run out of fingers and toes.

But before you run out of fingers and toes, you have created languages that contain dozens of keywords, hundreds of constraints, a tortuous syntax, and a reference manual that reads like a law book.  Indeed, to become an expert in these languages, you must become a _language lawyer_ (a term that was invented during the `C++` era.)

_This is the wrong path!_

Ask yourself why we are trying to plug defects with language features.  The answer ought to be obvious.  We are trying to plug these defects because these defects happen too often.

Now, ask yourself why these defects happen too often.  If your answer is that our languages don't prevent them, then I strongly suggest that you quit your job and never think about being a programmer again; because defects are _never_ the fault of our languages.  Defects are the fault of _programmers_.   It is _programmers_ who create defects -- not languages.

And what is it that programmers are supposed to do to prevent defects?  I'll give you one guess.  Here are some hints.  It's a verb.  It starts with a "T".  Yeah.  You got it.  _TEST!_

You _test_ that your system does not emit unexpected `null`s.  You _test_ that your system handles `null`s at it's inputs.  You _test_ that every exception you can throw is caught somewhere.  

Why are these languages adopting all these features?  Because programmers _are not testing_ their code.  And because programmers are not testing their code, we now have languages that _force_ us to put the word `open` in front of every class we want to derive from.  We now have languages that _force_ us to adorn every function, all the way up the calling tree, with `try!`.   We now have languages that are so constraining, and so over-specified, that you have to design the whole system up front before you can code any of it.

Consider:  How do I know whether a class is `open` or not?  How do I know if somewhere down the calling tree someone might throw an exception?  How much code will I have to change when I finally discover that someone really needs to return a `null` up the calling tree?

All these constraints, that these languages are imposing, presume that the programmer has perfect knowledge of the system; _before the system is written_.  They presume that you _know_ which classes will need to be `open` and which will not.  They presume that you _know_ which calling paths will throw exceptions, and which will not.  They presume that you _know_ which functions will produce `null` and which will not.

And because of all this presumption, they _punish_ you when you are wrong.  They force you to go back and change massive amounts of code, adding `try!` or `?:` or `open` all the way up the stack.  

And how do you avoid being punished?  There are two ways.  One that works; and one that doesn't.  The one that doesn't work is to design everything up front before coding.  The one that _does_ avoid the punishment is to _override all the safeties_.

And so you will declare _all_ your classes and _all_ your functions `open`.  You will _never_ use exceptions.  And you will get used to using lots and lots of `!` characters to override the `null` checks and allow `NPE`s to rampage through your systems.

----


Why did the nuclear plant at Chernobyl catch fire, melt down, destroy a small city, and leave a large area uninhabitable?  _They overrode all the safeties._  So don't depend on safeties to prevent catastrophes.  Instead, you'd better get used to writing lots and lots of tests, no matter what language you are using!







