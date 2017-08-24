---
layout: post
title: The Domain Discontinuity
tags: ["Testing"]
---
## What came first, the chicken or the road?

I read, with interest, Justin Searls' [Post #13](http://blog.testdouble.com/posts/2014-01-25-the-failures-of-intro-to-tdd.html).  First I am going to express my deep disagreement with the Author.  I will refute his arguments and utterly destroy his conclusions.  And then, once I'm done salting the ground where he used to live, I'll tell you why I completely agree with him.

# Destruction.

The author bemoans the failures in the way many of us teach Test Driven Development.  He enumerates a set of woes encountered by students as a result of that teaching method.  He then describes a different technique, for teaching and executing TDD, that he believes will eliminate those woes. 

I agree that many people have encountered the woes he describes.  Indeed, _I_ encountered those woes when I first learned TDD.  I became adept at TDD by _solving_ those woes.  And therein lies my problem with the Author's thesis.  You don't learn complex things by having the solutions given to you.  You learn complex things by solving the problems that those complexities present.  Solving the issues of TDD is the _homework_ that all developers must do if they are to become adept.

Here are the woes that the Author presented.

### Large Units.

Given that you are following the discipline of TDD, it is true that when you have a big feature to  write, you're going to write a lot of little tests.  It is also true that programmers who are new to TDD will tend to make those tests pass inside a single large function.  That function will probably grow, and grow, and become a mess.  

The Author rightly says that TDD instructors warn their students about this mess and instruct them to refactor frequently.  The Author also rightly says that many students won't do that refactoring because they are under pressure to get features done.  The Author finally points out that an appeal to professionalism is inadequate to get these programmers to refactor frequently enough.  All true.

However, while the Author describes this as a failure, I describe it as necessary.  You don't learn to refactor by being told.  You learn to refactor by experiencing the alternative -- _and then remembering what you were told_.  

Learning to refactor is a hill that everyone has to climb for themselves.  We, instructors, can do little more than make sure the walking sticks are in your backpack.  We can't make you take them out and use them. 

A good TDD instructor gives everyone a stern warning, and then gives them a few exercises that are designed to make them stumble if they don't refactor.  In the courses I teach, I watch this with a certain amount of glee.  I see the students make the same mistakes I made when I was learning; but in a much safer environment.  As they fail, I see "the lights go on".   

## To get to the other egg.

The Author continues with a causal chain that I don't really understand.  Here is the essence of it:

### Extraction is Costly?

The Author argues that the large units created by passing many tests must be broken up by extracting smaller units from them, _and that extracting such units is costly._  That is not my experience at all.  Done early and frequently, extraction is _trivial_.  Most of the time it amounts to nothing more than selecting a few lines of code, and then hitting `ctrl-alt-M`.

### Tests must be rewritten?

Next the Author argues that the new smaller units need new unit tests.  That's news to me.  I certainly don't rewrite my  tests just because I extracted some functions or classes.  

It is a common misconception that the design of the tests must mirror the design of the production code.  TDD _does not_ require, as the Author suggests, "that every unit in your system is paired with a well-designed [...] unit test."  Indeed, that's one of the reasons that many of us have stopped calling them "unit" tests.

Let me stress this more.  I _do not_ create a test for every method or every class.  I create tests that define _behaviors_, and then I create the methods and classes that implement those behaviors.  

At the start, when there are just a few tests, I might have only one simple method.  But as more and more tests are added, that one simple method grows into something too large.  So I extract functions and classes from it -- _without changing the tests_.  I generally wind up with a few public methods that are called by my tests, and a large number of private methods and private classes that those public methods call; _and that the tests are utterly ignorant of_.

By the way, this is an essential part of good test design.  We _don't_ want the tests coupled to the code; and so we restrict the tests to operate through a small set of public methods.

### An Effect without a Cause.

The Author continues his argument by asserting all the various woes that come about because of all those new tests.  He suggests that the new tests are redundant, messy, written after the fact, over mocked, etc. etc.  He describes a nightmare scenario.  Fortunately, that's all it is, since in the waking world we don't write those new tests.

### Get it right the first time.

The solution that the Author presents is, in short,  Waterfall.  

Actually, he proposes a set of teeny-weeny waterfalls.  His recommendation is:

>Start writing a unit test for the entry point, but instead of immediately trying to solve the problem, intentionally defer writing any implementation logic! Instead, break down the problem by dreaming up all of the objects you wish you had at your disposal...

The Author's goal seems to be the elimination of refactoring.  He seems to be saying: "Let's just design it correctly up front."  His goal is apparently to create the same structure that refactoring would have created; but without the refactoring step.  

The Author has apparently forgotten that up-front design is a risky proposition.  Often when you "dream up the objects you wish you had" you find those dreams were actually nightmares.  To correct those nightmares, you have to: _refactor_.  

And so, up front design does _not_ eliminate the refactor step; it just postpones it until it is very expensive.

## Salt the Earth
In short, the Author recommends that we teach students to do TDD by having them do Waterfall.  (Cue: Nuclear Explosion).  

# I Completely Agree

If you step back from the Author's actual examples, and view his post in terms of system architecture, then _everything changes_.  

* Refactoring across architectural boundaries is _costly_.  
* Behaviors extracted across architectural boundaries need _newly rewritten_ tests.
* Architecture is an _up-front_ activity.

As an example, let's just say that you wrote some tests for a feature, and you implemented that feature in a way that passed the tests, but was an architectural nightmare.  Let's say that you've got SQL in your GUI, you've got business rules in your SQL, and you've got formatting code in JSPs.  You know, like 90% of the code out there...

Separating the GUI, the business rules, and the database from such a mess is very costly.  What's more, since the tests run through the GUI and use the Database, they are slow; and must be _rewritten_ to be fast by using mocks and respecting boundaries.  You know, all that good test design stuff we've been yelling about for the last decade.  

The solution to _that_ problem is to _know in advance_ where you are going to put certain behaviors.  You need to know, in advance, that there will be a boundary between the GUI, the business rules, and the database.  You have to know, in advance, that the features of your system have to be broken up into those areas.  In short, before you write your first test, you have to "dream up the [boundaries] that you wish you had".  

### The Domain Discontinuity

What's going on here?  First I say you _don't_ want to do your design up front; then I say that you _do_ want to do your design up front. Which is it?  

Both.  Or rather, it depends what kind of design it is.  So let's look deeper.  

When we follow the TDD discipline, we write tests and get them to pass.  What do those tests describe?  They describe features requested by the customer.  The tests are a kind of _formal statement of the requirements_.  They describe the problem domain.  

The design of the code that passes those tests is closely associated with the problem domain that the tests describe.  In the red-green-refactor cycle, we extract methods and classes in order to better fit within the problem domain described by the tests.  

But there is another level of software design -- a level that has nothing to do with the business rules, or the problem domain.  This is the level of system architecture.  

### Architecture Classifications

Applications can be mapped into several broad classifications.  For example, our application might be a request/response system, like nearly all websites.  Or it might be an event driven system, like most computer games.  Or it might be a batch processing system, like most old banking and manufacturing applications.

These classifications have nothing to do with the business rules, nor the problem domain.  And yet these classifications have a profound effect on the shape and design of the software.  That shape, that design, is architecture.  

If you were to look at two completely different event driven applications, say Quicken and Minecraft, the odds are that you would find a lot of similarities at the highest levels.  The same should be true of any two request/response systems, or two batch systems.  At their highest levels, there will be similarities in shape, flow, and structure.  

This shape is the system architecture, and it is dictated by the desired user experience -- _not_ by the problem domain.  I could, for example, write an accounting application, like Quicken, in a batch style.  I could, though it would be a deep shame, write an adventure program like Minecraft, in a request/response style.  So architecture and problem domain are discontinuous -- they do _not_ form a continuum -- they differ in _kind_ not just in level.

This discontinuity means that, although we can use TDD to help us with the design of the problem domain,  we cannot use it to help us with the architecture.  TDD can't even be _begun_ until we know the shape of the system that is to be created.

So we have to decide the architecture up front, based on our desired user experience, and then we can use TDD to help us design a problem domain that lives _within_ that architecture.  

Fortunately, the number of system architectures is relatively small; and there are a number of well-conceived patterns that they fall into.  What's more, the architecture of the system is almost entirely a function of the desired user experience. 

So our up front decisions can be limited to choosing a user experience, and choosing the architectural pattern that is most consistent with that user experience.  Once those choices are made, we can TDD the problem domain into existence.

## Conclusion

And so the status of our misbegotten Post #13 is a quantum-bit.  Like Shrodinger's cat, it lives in a superposition of states.  It is right after all; except that it is dead wrong.  And in being wrong, it is certainly correct.  Which state it finally collapses into depends on which side of the Domain Discontinuity you stand.





