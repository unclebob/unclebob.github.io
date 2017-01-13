---
layout: post
title: Types and Tests
tags: ["Software"]
---
>_Friday the 13th!_

The response to my [Dark Path](http://blog.cleancoder.com/uncle-bob/2017/01/11/TheDarkPath.html) blog has been entertaining.  It has ranged from effusive agreement to categorical disagreement.  It also elicited a few abusive insults.  I appreciate the passion. A nice vocal debate is always the best way to learn. As for the insulters: you guys need to move out of your Mom's basement and get a life.

To be clear, and at the risk of being repetitive, that blog was not an indictment of static typing.  I rather enjoy static typing.  I've spent the last 30 years working in statically typed languages and have gotten rather used to them.  

My intent, with that blog, was to complain about how far the pendulum has swung.  I consider the static typing of Swift and Kotlin to have swung too far in the statically type-checked direction.  They have, IMHO, passed the point where the tradeoff between expressiveness and constraint is profitable.  

----

One of the more common responses to that blog was: "Hey, man, like... _Types are Tests!_"

No, types are not tests.  Type systems are not tests.  Type checking is not testing.  Here's why.

A computer program is a specification of behavior.  The purpose of a program is to make a machine behave in a certain way.  The text of the program consists of the instructions that the machine follows.  The sum of those instructions is the behavior of the program.  

Types do not specify behavior.  Types are constraints placed, by the programmer, upon the textual elements of the program.  Those constraints reduce the number of ways that different parts of the program text can refer to each other.

Now this kind of constraint system can be very useful in reducing the incidence of textual errors in a program.  If you specify that function `f` must be called with an `Int` then the type system will ensure that no other part of the program text will invoke `f` with a  `Double`.  If such an error were allowed to escape into a running program (as was common back in the good old `C` days) then a runtime error would likely result.  

You might therefore say that the type system is a kind of "test" that fails for all inappropriate invocations of `f`.  I might concede that point except for one thing -- _the way `f` is called has nothing to do with the required behavior of the system_.  Rather it is a test of an arbitrary constraint imposed by the programmer.  A constraint that was likely _over_ specified from the point of view of the system requirements.

The system requirements likely do not depend on the fact that the argument of `f` is an `Int`.  We could very likely change the declaration of `f` to take a `Double`, without affecting the observed behavior of the program at all.  

So what the type system is checking is not the external behavior of the program.  It is checking only the internal consistency of the program text.  

Now this is no small thing.  Writing program text that is internally consistent is pretty important.  Inconsistencies and ambiguities in program text can lead to misbehaviors of all kinds.  So I don't want to downplay this at all.

On the other hand, internal self-consistency does not mean the program exhibits the correct behavior.  Behavior and self-consistency are orthogonal concepts. Well behaved programs can be, and have been, written in languages with high ambiguity and low internal consistency.  Badly behaved programs have been written in languages that are deeply self-consistent and tolerate few ambiguities.

So just how internally consistent do we need the program text to be?  Does every line of text need to be _precisely_ 60 characters long?  Must indentations always be multiples of two spaces?  Must every floating point number have a dot?  Should all positive numbers begin with a `+`?  Or should we allow certain ambiguities in our program text and allow the language to make assumptions that resolve them?  Should we relax the specificity of the language and allow it to tolerate certain easily resolvable ambiguities?  Should we allow this even if sometimes those ambiguities are resolved incorrectly?

Clearly, every language chooses the latter option.  No language forces the program text to be _absolutely_ unambiguous and self consistent.  Indeed, such a language would likely be impossible to create.  And even if it weren't, it would likely be impossible to use.  Absolute precision and consistency has never been, nor should it ever be, the goal.  

So how much internal unambiguous self-consistency do we need?  It would be easy to say that we need as much as we can get.  It might seem obvious that the more unambiguous and internally consistent a language is, the fewer defects programs written in that language will have.  But is that true?

The problem with increasing the level of precision and internal self consistency, is that it implies an increase in the number of constraints.  But constraints need to be specified; and specification requires notation.  Therefore as the number of constraints grows, so does the complexity of the notation.  The syntax and the semantics of a language grows as a function of the internal self-consistency and specificity of the language. 

As notation and semantics grow in complexity, the chance for unintended consequences grows.  Among  the worst of those consequences are Open-Closed violations.

Imagine that there is a language named `TDP` that is ultimately self-consistent and specific.  In `TDP` every single line of code is self-consistent with, and specific to, every other line of code.  A change to one line forces a change to every other line in order to maintain that self-consistency and specificity.

Do languages like this exist?  No; but the more type-safe a language is, the more internally consistent and specific it forces the programmers to be, the more it approaches that ultimate `TDP` condition.

Consider the `const` keyword in `C++`.  When I was first learning `C++` I didn't use it.  It was just too much on top of everything else there was to learn.  But as I gained in knowledge and comfort with the language the day came when I used my first `const`.  And down the rathole I went, fixing one compile error after another, changing hundreds and hundreds of lines of code, until the system I was working on was `const`-correct.  

Did I stop using `const` because of this experience?  No, of course not.  I just made sure that I knew, up front, which fields and functions were going to be `const`.  This required a lot of up-front design; but that was worth the alternative.  Did that make the problem go away?  Of course not.  I frequently found myself running around inside the system smearing `const` all over the place.

Is `TDP` a good condition to be in?  Do you want to have to change every line of code every time anything at all changes?  Clearly not.  This violates the OCP, and would create a nightmare for maintenance.

Perhaps you think I'm setting up a straw man argument.  After all, `TDP` does not exist.  My claim, however, is that Swift and Kotlin have taken a step in that undesirable direction.  That's why I called it: _The Dark Path_.  

Every step down that path increases the difficulty of using and maintaining the language.  Every step down that path forces users of the language to get their type models "right" up front; because changing them later is too expensive.  Every step down that path forces us back into the regime of Big Design Up Front.  

But does that mean we should never take even a single step down that path?  Does that mean our languages should have no types and no specific internal self consistency.  Should we all be programming in `Lisp`?  

>_(That was a joke, all you guys living in your Mom's basement can keep your insults to yourself please; and stay off my lawn.  As for `Lisp` the answer is: Yes, we probably should all be programming in Lisp; but for different reasons.)_

Type safety has a number of benefits that, at first, outweigh the costs.  A few steps down the dark path allow us to gather some pretty nice low hanging fruit.  We can gain a decent amount of specificity and self consistency without huge violations of the OCP.  Type models can also enhance expressivity and readability.  And type models definitely help IDEs with refactorings and other mechanical operations.  

But there is a _balance point_ after which every step down _The Dark Path_ increases the cost over the benefit.  

I think Java and C# have done a reasonable job at hovering near the balance point.  (If you ignore the horrible syntax for generics, and the ridiculous proscription against multiple inheritance.)  In my opinion those languages have gone just a bit too far; but the costs of type-safety aren't too high to tolerate.  Ruby, Groovy, and Javascript, on the other hand, hover on the other side of the balance point.  They are, perhaps, a bit too permissive; a bit too ambiguous (Does anybody really understand the sub-object graph in Ruby?). 

So, a little type safety, like a little salt, is a good thing.  Too much, on the other hand, can have unfortunate consequences.

-----

Does every step down _The Dark Path_ mean that you can ignore a certain number of unit tests?  Does programming in _Dark Path_ languages mean that you don't have to test as much?  

No.  A thousand times: NO.  Type models do not specify behavior.  The correctness of your type model has no bearing on the correctness of the behavior you have specified.  At best the type system will prevent some mechanistic failures of representation (e.g. `Double` vs. `Int`); but you still have to specify every single bit of behavior; and you still have to _test_ every bit of behavior.  

So, no, type systems do not decrease the testing load.  Not even the tiniest bit.  But they _can_ prevent some errors that unit tests might not see. (e.g. `Double` vs. `Int`)











