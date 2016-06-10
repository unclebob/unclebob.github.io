---
layout: post
title: Mutation Testing
tags: ["Software"]
---
At [XP2016](http://conf.xp2016.org) I attended an open-space demonstration of mutation testing.  In particular, an open source tool for the Java space named [pitest](http://pitest.org).  I came away pretty impressed.

I had heard of mutation testing before.  A decade and a half ago there was an open source tool named [jester](http://jester.sourceforge.net/).  Nothing much came of Jester back then.  Perhaps we were too focussed upon TDD to think beyond it.  After all, the very notion of rigorous unit testing was very controversial -- at the time.

But before I get philosophical, perhaps I should describe what mutation testing is.

### The Problem.

What's the problem with unit tests?  Dijkstra said it long, long ago.  "Testing shows the presence, not the absence of bugs."  This sentiment has long been used as a complaint against the discipline of TDD.  "Write all the tests you like", the detractors would say, "it still doesn't prove your code actually works."

Dijkstra was right, of course.  However, his complaint could also be raised about the scientific method.  To paraphrase: Experiments can only disprove, never prove, a theory."  And yet every day we are willing to bet our lives on those unproven theories of Newton, Einstein, Maxwell, and Boltzmann.  If experiments are good enough for science, why aren't unit tests good enough for software? 

There are many answers to that, which I will phrase as questions[1]:

 1. Have you written enough tests?
 2. Have you covered every line, every branch, every path?
 3. If a semantic change is made to the code, will some test fail?

###Sufficiency.

The first question is obvious.  If you missed a test, you may have a bug.  There are two kinds of missing tests.  The first is some statements in the code that are not tested by the tests.  The second is some requirements that the developers missed.  

There's not much we can do about that later case other than to carefully review the requirements and to expose the software to customers and users early and often to get their feedback.

The former case is a symptom of test-after.  If you write the code first, and then write the tests for the code, you are very likely to miss some statements or branches.  This is one of the biggest arguments in favor of the test-first strategy of TDD.  If you write your tests first, and if you refuse to write a line of production code unless it is to get a failing test to pass, then you are not likely to leave any code uncovered by the tests.

###Coverage.

And that brings us to the second question.  Have you covered all the lines, branches, and paths?  Covering every path is impractical.  The simple explosion in the number of paths makes the testing burden enormous.  We may solve this problem one day; but today is not that day.  

However, we have tools that will tell us what lines and branches are covered.  These coverage tools are readily available, easy to run, and generally quite fast.  They aren't perfect, but overall they're pretty good.  

So what should your goal be?  The question is absurd.  There is no justifiable goal other than 100%.  Every single line, and every single branch, should be tested by your unit tests.  I realize that this goal is not practicably achievable. So I think of it as an asymptotic goal -- one that we are always pushing towards, but never quite achieving.  

Of course the problem with coverage is that it doesn't prove what you might think it proves.  It does not prove that you have _tested_ every line and every branch.  All it proves is that you have _executed_ every line and every branch.  Pull out all the asserts from your tests, and your coverage remains unchanged!

###Semantic Stability. 

And that leads us to the third, and most important question.  If you make a semantic change to the code -- a change that alters the meaning of the code -- will one of your tests detect it?  That's a very high bar for a test suite.  But, again, it is the only justifiable bar to set.  _Of course_ your test suite should fail if you make a semantic change to your production code.  Does anybody realistically doubt that?

And think about what such semantic stability means.  It means that your test suite _tests_ every line and every branch.  It means that your test suite verifies every behavior written into your system.  

Well, perhaps not quite.  Remember we aren't ensuring that all pathways are covered.  Still, if we change the sense of an `if` statement, and some test doesn't fail, that's a problem.  If, on the other hand, we walk through the code and, one-by-one, change the sense of every `if` statement to see if a test fails, then we can be pretty sure that all our `if` statements are covered _and_ tested.  If we also change, one-by-one, the sense of every `while` statement; and if, one-by-one, we remove every function call; and we ensure that each of those changes causes our test-suite to fail, then we can be pretty sure that those `while` statements and function calls are covered and tested.

###Mutation Testing
This is what mutation testing does.  The `pitest` tool first runs a coverage analysis by executing your test suite and measuring which lines are covered by the tests.  Then, one-by-one, it makes semantic changes to the `Java` byte code, such as inverting the sense of `if` and `while` statements, and removing function calls.  Each one of those changes is called a _mutant_.  

For each mutant the tool runs the unit test suite; and if that suite _fails_, the mutant is said to have been _killed_. That's a _good_ thing.

If, on the other hand, a mutant passes the test suite, it is said to have _survived_.  This is a bad thing. It means that the tests do not check for that semantic change.  Strangely, the sense for mutant tests is _inverted_; we expect them to fail.  A passing mutant test is bad.  Mutants should all be red!

A surviving (green) mutant might be the result of tests that have been `@ignore`d, or commented out, or when asserts have been removed, or  never added.  It can also happen if TDD discipline got a little lax at some point, and some code got added without a corresponding test.

As a side note, I have found `pitest` to be pretty easy to operate, and relatively fast.  It apparently does some smart dependency detection to help it determine what tests need to be run for particular mutants.  Your mileage may vary; and I did have to break the mutation tests up into small parts for one larger project I am working on.  Still, I have found the tool to be quite useful at identifying semantic instabilities in my test suites.

###Implications
A fundamental goal of TDD is to create a test suite that you can trust, so that you can effectively refactor.  We need to be able to refactor in order to keep the code clean enough to modify and enhance without paying huge costs and taking huge risks.  The cleaner the code the longer it's useful lifetime.

For years the argument has been that test-after simply cannot create such a high reliability test suite.  Only diligent application of the TDD discipline has a chance of creating a test suite that you implicitly trust.  

However, with a mutation testing tool like `pitest` I have successfully augmented a test-suite created with lax TDD discipline into one that I can implicitly trust.  The implication of that is significant.  If a development team dedicates itself to creating a test suite that is semantically stable, and verifies that stability by using a mutation tester like `pitest`, then does it really matter if the tests were written first?

Oh there are other arguments for test-first, of course.  There's the design argument, and the cycle time argument, and the fun factor argument among many others. Valid as those arguments may be, they pale in comparison to creating a test suite that guarantees semantic stability.

As hard-nosed as I am about TDD as a necessary discipline; if I saw a team using mutation testing to guarantee the semantic stability of a test-after suite; I would smile, and nod, and consider them to be highly professional.  (I would also suggest that they work test-first in order to streamline their effort.)

----

[1] If you think about it, each of those questions has an analog for science.  We don't trust our lives to theories that have not been subjected to experimental testing that is complete, covered, and controlled (semantically stable).  
