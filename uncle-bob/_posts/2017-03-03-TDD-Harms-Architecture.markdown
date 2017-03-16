---
layout: post
title: TDD Harms Architecture
tags: ["Software"]
---
The idea that TDD damages design and architecture is not new.  DHH suggested as much several years ago with his notion of [Test Induced Design Damage](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html); in which he compares the design he prefers to a design created by [Jim Weirich](https://en.wikipedia.org/wiki/Jim_Weirich) that is "testable".  The argument, boils down to separation and indirection.  DHH's concept of good design _minimizes_ these attributes, whereas Weirich's _maximizes_ them.

I strongly urge you to read DHH's article, [and watch Weirich's video](https://www.youtube.com/watch?v=tg5RFeSfBM4), and judge for yourself which design you prefer.

Recently I've seen the argument resurface on twitter; though not in reference to DHH's ideas; but instead in reference to a [very old interview](https://www.infoq.com/interviews/coplien-martin-tdd) between James Coplien and myself.  In this case the argument is about using TDD to allow architecture to _emerge_.  As you'll discover, if you read through that interview, Cope and I agree that architecture does not emerge from TDD.  The term I used, in that interview was, I believe -- Horse shit.

Still another common argument is that as the number of tests grows, a single change to the production code can cause hundreds of tests to require corresponding changes.  For example, if you add an argument to a method, every test that calls that method must be changed to add the new argument.  This is known as [The Fragile Test Problem](http://xunitpatterns.com/Fragile%20Test.html).  

A related argument is: The more tests you have, the harder it is to change the production code; because so many tests can break and require repair.  Thus, tests make the production code rigid.

### What's behind this?

Is there anything to these concerns?  Are they real?  Does TDD really damage design and architecture?  

There are too many issues to simply disregard.  So what's going on here?

Before I answer that, let's look at a simple diagram.  Which of these two designs is better?

<img src="/assets/TDD_Harms_Architecture/user_service.JPG" size="80%">

Yes, it's true, I've given you a hint by coloring the left (sinister) side red, and the right (dexter) side green.  I hope it is clear that the right hand solution is generally better than the left.  

Why?  Coupling, of course.  In the left solution the users are directly coupled to a multitude of services.  Any change to a service, regardless of how trivial, will likely cause many users to require change.  So the left side is fragile. 

Worse, the left side users act as anchors that impede the ability of the developers to make changes to the services.  Developers fear that too many users may be affected by simple changes. So the left side is rigid.

The right side, on the other hand, decouples the users from the services by using an API.  What's more, the services _implement_ the API using inheritance, or some other form of polymorphism.  (That is the meaning of the closed triangular arrows -- a UMLism.)  Thus a large number of changes can be made to the services without affecting either the API or the users.  What's more the users are not an anchor making the services rigid.  

The principles at play here are the [Open-Closed Principle (OCP)](https://en.wikipedia.org/wiki/Open/closed_principle) and the [Dependency Inversion Principle (DIP)](https://en.wikipedia.org/wiki/Dependency_inversion_principle).  

Note, that the design on the left is the design that DHH was advocating in his article; whereas the design on the right was the topic of Weirich's exploration.  DHH likes the directness of the design on the left.  Weirich likes the separation and isolation of the design on the right.

### The Critical Substitution

Now, in your mind, I want you to make a simple substitution.  Look at that diagram, and substitute the word "TEST" for the word "USER"  -- and then _think_.

Yes.  That's right.  _Tests need to be designed_.  Principles of design apply to tests just as much as they apply to regular code.  Tests are _part of the system_; and they must be maintained to the same standards as any other part of the system.

### One-to-One Correspondence.

If you've been following me for any length of time you know that I describe TDD using [three laws](http://programmer.97things.oreilly.com/wiki/index.php/The_Three_Laws_of_Test-Driven_Development).  These laws force you to write your tests and your production code simultaneously, virtually line by line.  One line of test, followed by one line of production code, around, and around and around.  If you've never seen or experienced this, you might want to watch [this](https://www.youtube.com/watch?v=AoIfc5NwRks) video.

Most people who are new to TDD, and the three laws, end up writing tests that look like the diagram on the left.  They create a kind of one-to-one correspondence between the production code and the test code.  For example, they may create a _test class_ for every production code class.  They may create _test methods_ for every production code method.  

Of course this makes sense, _at first_.  After all, the goal of any test suite is to test the elements of the system.  Why wouldn't you create tests that had a one-to-one correspondence with those elements?  Why wouldn't you create a test class for each class, and a set of test methods for each method?  Wouldn't that be the _correct_ solution?

And, indeed, most of the books, articles, and demonstrations of TDD show precisely that approach.  They show tests that have a strong structural correlation to the system being tested.  So, of course, developers trying to adopt TDD will follow that advice.

The problem is -- and I want you to think carefully about this next statement -- _a one-to-one correspondence implies extremely tight coupling_.   

Think of it!  If the structure of the tests follows the structure of the production code, then the tests are inextricably coupled to the production code -- and they follow the sinister red picture on the left!

### FitNesse
It, frankly, took me many years to realize this.  If you look at the structure of [FitNesse](https://github.com/unclebob/fitnesse), which we began writing in 2001, you will see a strong one-to-one correspondence between the test classes and the production code classes.  Indeed, I used to tout this as an advantage because I could find every unit test by simply putting the word "Test" after the class that was being tested.  

And, of course, we experienced some of the problems that you would expect with such a sinister design.  We had fragile tests.  We had structures made rigid by the tests.  We felt the pain of TDD.  And, after several years, we started to understand that the cause of that pain was that we were not designing our tests to be decoupled.

If you look at part of FitNesse written after 2008 or so, you'll see that there is a significant drop in the one-to-one correspondence.  The tests and code look more like the green design on the right.

### Emergence.

The idea that the high level design and architecture of a system emerge from TDD is, frankly, absurd.  Before you begin to code any software project, you need to have some architectural vision in place.  TDD will not, and can not, provide this vision.  That is not TDD's role. 

However, this does not mean that designs do not emerge from TDD -- they do; just not at the highest levels.  The designs that emerge from TDD are one or two steps above the code; and they are intimately connected to the code, and to the red-green-refactor cycle.  

It works like this: As some programmers begin to develop a new class or module, they start by writing simple tests that describe the most degenerate behaviors.  These tests check the absurdities, such as what the system does when no input is provided.  The production code that solves these tests is trivial, and gradually grows as more and more tests are added.  

At some point, relatively early in the process, the programmers look at the production code and decide that the structure is a bit messy.  So the programmers extract a few methods, rename a few others, and generally clean things up.  This activity will have _little or no effect on the tests._  The tests are still testing all that code, regardless of the fact that the structure of that code is changing.

This process continues.  As tests of ever greater complexity and constraint are added to the suite, the production code continues to grow in response.  From time to time, relatively frequently, the programmers clean that production code up.  They may extract new classes.  They may even pull out new modules.  And yet the tests remain unchanged.  The tests still cover the production code; _but they no longer have a similar structure_.

And so, to bridge the different structure between the tests and the production code, _an API emerges_.  This API serves to allow the two streams of code to evolve in very different directions, responding to the opposing forces that press upon tests and production code.

### Forces in Opposition

I said, above, that the tests remain unchanged during the process.  This isn't actually true.  The tests are also refactored by the programmers on a fairly frequent basis.  But the direction of the refactoring is _very different_ from the direction that the production code is refactored.  The difference can be summarized by this simple statement:

> _As the tests get more specific, the production code gets more generic._

This is (to me) one of the most important revelations about TDD in the last 16 years.  These two streams of code evolve in opposite directions.  Programmers refactor tests to become more and more concrete and specific.  They refactor the production code to become more and more abstract and general.  

Indeed, _this is why_ TDD works.  This is why designs can emerge from TDD.  This is why algorithms can be derived by TDD.  These things happen as a direct result of programmers pushing the tests and production code in opposite directions.  

_Of course_ designs emerge, if you are using design principles to push the production code to be more and more generic.  _Of course_ APIs emerge if you are pulling these two streams of communicating code towards opposite extremes of specificity and generality.  _Of course_ algorithms can be derived if the tests grow ever more constraining while the production code grows ever more general.

And, of course, highly specific code _cannot have_ a one-to-one correspondence with highly generic code.  

### Conclusion

What makes TDD work?  You do.  Following the three laws provides no guarantee.  The three laws are a discipline, not a solution.  It is _you_, the programmer, who makes TDD work.  And you make it work by understanding that tests are part of the system, that tests must be designed, and that test code evolves towards ever greater specificity, while production code evolves towards ever greater generality.  

Can TDD harm your design and architecture?  _Yes!_ If you don't employ design principles to evolve your production code, if you don't evolve the tests and code in opposite directions, if you don't treat the tests as part of your system, if you don't think about decoupling, separation and isolation, you will damage your design and architecture -- TDD or no TDD. 

You see, it is not TDD that creates bad designs.  It is not TDD that creates good designs.  It's you.  TDD is a discipline.  It's a way to organize your work.  It's a way to ensure test coverage.  It is a way to ensure appropriate generality in response to specificity.  

TDD is important. TDD works.  TDD is a professional discipline that all programmers should learn and practice.  But it is not TDD that causes good or bad designs.  _You_ do that.

Is is only programmers, not TDD, that can do harm to designs and architectures.  


