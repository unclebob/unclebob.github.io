---
layout: post
title: Test Contra-variance
tags: ["Software"]
---
Do you write unit tests?

>_Yes, of course!_

Do you write them first?

>_Yes, I follow [the three laws of TDD](http://www.softwaretestingmagazine.com/knowledge/the-three-rules-of-test-driven-development/)._

What is the difference in module structure between your tests and your code?

>_I create one test class per production class._

So if you have a production class named `User` you will have a test class named `UserTest`?

>_Yes, almost always._

So the structure of your tests, and the structure of your code are _covariant_.

>_Um.  I suppose so, yes._

And so you are coupling the structure of your tests to the structure of your production code.

>_I hadn't thought about it being a coupling before; but, yes, I suppose I am._

So when you refactor the class structure of your production code, without changing any behavior; do your tests all break?

>_Well, yes.  Of course._

And that means you can't run your tests while you are refactoring, doesn't it?

>_Yes, yes, it does._

So then you can't really call it refactoring, can you?

>_Why not?_

Because refactoring is defined as a sequence of small changes that keep the tests passing at all times.  

>_Well, OK.  I guess by that definition, the changes aren't refactoring._

Instead, you have to commit yourself to a big change and hope you can put everything back together again -- including the tests.

>_Yes, yes.  What of it?_

That is an example of the Fragile Test Problem.

>_The Fragile Test Problem?_

Yes.  A common complaint amongst people who try TDD for the first time.  They note that small changes to the production code can cause large changes to the tests.

>_Yeah.  That's really frustrating.  I almost gave up on TDD when I first encountered it._

Unfortunately, that is a common reaction.

>_OK, but what can be done about it?_

Design the structure of your tests to be contra-variant with the structure of your production code.

>_Contra-variant?_

Yes.  The structure of your tests should not be a mirror of the structure of your code.  The fact that you have a class named `X` should not automatically imply that you have a test class named `XTest`.  

>_But wait.  That breaks the rules!_

What rules?

>_The rules that say that there should be one test class per class._

There is no such rule.

>_There isn't?  I'm sure I've read it and seen it._

Not everything your read and see is a rule.  

>_Fair enough.  But if the structure of the code and the tests must be, um. contra-variant, then how should the tests be structured?_

First, let's agree on a basic fact.  If a small change in one module of a system causes large changes in many other modules of the system, the system has a design problem.  

>_Yes, I think that's obvious -- software design 101 so to speak._

Then, clearly, if a small change to the production code causes large changes to the tests, there is a design problem.

>_I see that point, yes._

Therefore the tests must have their own design.  They cannot simply follow along with the structure of the production code.

>_Hmmm.  I see.  If the two designs are the same, then they are coupled; and that coupling causes fragility._ 

Right.  The coupling between the tests and the production code must be minimized.  

>_But wait!  The tests and the code must be coupled because they both describe the same behavior._

Correct.  Their behavior is coupled; but their structure need not be coupled.  And even the behavioral coupling need not be as tight as you think.

>_Can you give me an example?_

Suppose I begin writing a new class.  Call it `X`.  I first write a new test class named `XTest`.

>_But you said we shouldn't do that._

Bear with me.  We've just begun.  As I add more and more unit tests to `XTest` I add more and more code to `X`.  

>_And you refactor that code!_

Indeed I do.  I refactor it by extracting private methods from the original functions that are called by `XTest`.  

>_And you refactor the tests too, right?_

Absolutely!  I look at the coupling between `XTest` and `X` and I work to minimize it.  I might do this by adding constructor arguments to `X` or raising the abstraction level of the arguments I pass into `X`.  I may even impose a polymorphic interface between `XTest` and `X`.  

>_You'd do all that just for a test?_

Think of it this way.  The `XTest` is just the first client of `X`.  I always want to decrease the coupling between clients and servers.  So I used the same techniques I would use in normal production code to reduce the coupling between the `XTest` and `X`.

>_OK, but the structure of the tests is still the same as the structure of the code.  You still have `X` and `XTest`._

Yes, at the class level they are the same; and that's about to change.  Before we explore that change, however, I want you to note that there are already profound structural differences at the method level.  

>_Um.  Sure.  `XTest` is just using the public methods of `X`; but most of the code is now in the private methods that you extracted._

Right!  The structural symmetry is already broken.  But now it's going to break even more.

>_How so?_

As I look at all those private method in `X` I will inevitably see that there are ways to group those methods into different classes.  One group of methods will use a particular subset of the fields of `X`.  That group can be extracted as a class.

>_But you don't write a new test for that class, do you?_

No!  Because every bit of the code within that new class is being covered by the tests that are still just using the public API of `X`.  

>_And this process continues, doesn't it?_

Yes!  More and more functions are extracted.  More and more classes are discovered.  After awhile we have a whole family of classes in the production code that sit behind that simple API of `X`.

>_And they are all tested by `XTest`._

Right!  The structure has been almost perfectly decoupled.  What's more the API of `X` has been successively refined to be so narrow and abstract that it is minimally coupled to the clients that use it; including `XTest`.  

>_OK, I see that.  I see that the structure of the tests can vary independently from the structure of the production code.  And I agree that that's a good thing.  But what about the behavior.  They are still strongly coupled by behavior._

Think about what's going while `X` is being developed.  What's happening to `XTest`?

>_Well, more and more tests are being added to it; and the interface with `X` is being progressively narrowed and abstracted._

Right.  Now say that first part again.

>_More and more tests are being added?_

Right.  And each one of those tests is entirely concrete.  Each one of those tests is a small specification of a very particular behavior.  Taken together the sum of all the tests is...

>_The specification of the behavior of the `X` API._

Right!  So as development proceeds the test suite becomes more and more of a specification -- it becomes more and more _specific_.

>_Sure.  I see that._

But what is happening to the classes behind the `X` API?  What would any good software designer do when confronted with an ever growing list of specifications?  

>_Well, of course, the way we deal with complex specifications is to generalize._

Correct!  Instead of writing code for each and every case of every paragraph of every specification, we find ways to make the code _generic_.

>_Why does this matter to the coupling of the behavior?_

As development proceeds, the behavior of the tests becomes ever more specific.  The behavior of the production code becomes ever more generic.  The two behaviors move in opposite directions along the generalization axis.

>_And this reduces coupling?_

Yes.  Because while the behavior of the production code _satisfies_ the specifications within the tests, it also has the ability to satisfy a whole spectrum of unspecified behaviors.

And that last bit is absolutely essential, because no test suite can specify every required behavior.  The production code _must_ generalize the subset of behaviors specified by the tests to _all_ the behaviors required of the system.

>_So you are saying that the test suite in incomplete?_

Of course!  It is entirely impractical to specify absolutely everything.  So what happens instead is that we gradually increase the generality of the production code until every test that we could possibly write will pass.

>_Woah!  We keep writing failing tests in order to drive the generality of the production code to a point where it becomes impossible to write another failing test.  Woah!_

Woah indeed.  But here's the thing.  The act of generalizing _is the act of decoupling_.  We decouple by generalizing!

>_Oh, wow!  And so we decouple structure AND behavior.  Wow._

Right!  Now, give me a recap.

>_OK.  Um.  The structure of the tests must not reflect the structure of the production code, because that much coupling makes the system fragile and obstructs refactoring.  Rather, the structure of the tests must be independently designed so as to minimize the coupling to the production code._

Good!  And what about behavior?

>_As the tests get more specific, the production code gets more generic.  The two streams of code move in opposite directions along the generality axis until no new failing test can be written._

Good!  I think you've got it.

>_Yeah!  Contra-variance FTW!_
