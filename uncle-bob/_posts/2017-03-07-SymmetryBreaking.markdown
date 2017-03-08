---
layout: post
title: Symmetry Breaking
tags: ["Software"]
---
Imagine that you are an accountant.  You are responsible for manipulating arcane symbols, concepts, and procedures in order to create deeply complicated and detailed financial models for your business.  The stakes are enormous.  Accuracy is essential.  Millions wait to be lost or gained based upon your rare and esoteric skills.

How do you ensure your performance?  Upon what disciplines do you depend?  How will you make sure that the models you build, and the advice they imply, are faithful to your profession, and profitable for your business?

For the last 500 years, accountants have been using the discipline of _double-entry bookkeeping_.  The idea is simple; but the execution is challenging.  Each transaction is recorded, concurrently within a system of accounts; once as a debit, and then again as a credit.  These debits and credits follow separate but complimentary mathematical pathways, through a system of categorized accounts, until they converge on the balance sheet in a subtraction that must yield a zero.  Anything other than a zero implies an error was made somewhere along one of those pathways.

We, programmers have a similar problem.  We manipulate arcane symbols, concepts, and procedures in order to create deeply complicated and detailed models of behavior for our businesses.  The stakes are enormous.  Accuracy is essential.  Millions wait to be lost or gained based upon our rare and esoteric skills.

How do we ensure our performance?  Upon what disciplines do we depend?  How will we make sure that the models we build, and the behavior they elicit, are faithful to our profession, and profitable for our businesses.

It has long been asserted that Test Driven Development (TDD) is the equivalent of double-entry bookkeeping.  There are some undeniable parallels.  Under the discipline of TDD every desired behavior is written twice; once in test code that verifies the behavior, and once in production code that exhibits that behavior.  The two streams of code are written concurrently, and follow complimentary, yet separate execution pathways until they converge in the count of defects - a count that must be zero.

Another parallel is the granularity of the two disciplines.  Double-entry bookkeeping operates at the extremely fine granularity of individual transactions.  TDD operates at the equivalently fine granularity of individual behaviors and assertions.  In both cases the division between the granules is natural and obvious. There is no other granule for accounting; and it is hard to imagine a more appropriate granule for software. 

Still another parallel is the immediate feedback of the two approaches.  Errors are detected at every granule.  Accountants are taught to check the results for each and every transaction.  Programmers using TDD are taught to check the tests for every assertion.  Therefore, when properly executed, no error can infiltrate into, and thereby corrupt, large swathes of the models.  The rapid feedback, in both instances, prevents long hours of debugging and rework.

But as similar as these two disciplines appear to be on the surface, there are some deep differences between them.  Some are obvious; such as the fact that one deals with numbers and accounts, whereas the other deals with functions and assertions.  Other differences are less obvious and much more profound.  

### Asymmetry

Double-entry bookkeeping is symmetrical.  Debits and credits have no relative priority.  Each is derivable from the other.  If you know the credited accounts and the transactions, then you can derive a reasonable set of debited accounts, and vice versa.  Therefore, there is no reason that accountants must enter a credit or a debit first.  The choice is arbitrary.  The subtraction will work in either case.

This is not true of TDD.  There is an obvious arrow between the tests and the production code.  The tests depend upon the production code; the production code does not depend upon the tests.  This is true both at compile time, and at run time.  The arrow points in one direction, and one direction only.  

This asymmetry leads to the inescapable conclusion that the equivalence to double entry bookkeeping only works if the tests are written first.  There is no way to create an equivalent discipline if the production code is written before the tests.

This may be difficult to see at first.  So let's use the old mathematical trick of reduction to an absurdity.   But before we do that, let's state TDD with the formality that can be inverted; the formality of the three laws.  Those laws are:

1. _You are not allowed to write any production code without first writing a test that fails because the production code does not exist._
2. _You are not allowed to write more of a test than is sufficient to fail; including failure of compilation._
3. _You are not allowed to write more production code than is sufficient to pass the currently failing test._

Following these three laws results in a very orderly and discrete procedure:

* You must decide what production code function you intend to create.
* You must write a test that fails because that production code doesn't exist.
* You must stop writing that test as soon as it fails for any reason, including compilation errors.
* You must write only the production code that makes the test pass.  
* Repeat ad infinitum.

Note how the discipline enforces the fine granularity of individual behaviors and assertions; including compile time assertions.  Notice that there is very little ambiguity about how much code to write at any given point; and whether that code should be production or test code.  Those three laws tie you down into a very tightly constrained behavior.

It should be very clear that following the process dictated by the three laws is the logical equivalent of double-entry bookkeeping.

### Reductio ad Absurdum

Now, let's assume that a similar discipline can be defined that inverts the order, so that test code is written _after_ production code.  How would we write such a discipline?

We could start by simply inverting the three laws.  But as soon as we do we run into trouble:

* 1) _You are not allowed to write any test code without first writing production code that..._

How do you complete that rule?  In the un-inverted rule the sentence is completed by demanding that the test must fail because the production code doesn't yet exist.  But what is the condition for our new, inverted, rule?  We could choose something arbitrary like "_...is a complete function._"  However, this is not really a proper inverse of the first law of TDD.  

Indeed, _there is no proper inverse_.  The first law cannot be inverted.  The reason is that the first law presumes that you know what production feature you are about to create -- but so must _any_ first law, including any inverted first law.  

For example, we could try to invert the first law as follows:

* 1) _You are not allowed to write any test code without first writing production code that will fail the test code for the behavior you are writing._  

I think you can see why this is not actually an inversion.  In order to follow this rule, you'd have to write the test code in your mind, _first_, and then write the production code that failed it.  In essence the test has been identified before the production code is written.  The test has still come first.

You might object by noting that it is always possible to write tests after production code; and that in fact programmers have been doing just that for years.  That's true; but our goal was to write a _rule_ that was the inverse of the first law of TDD.  A rule that constrained us to the same level of granularity of behaviors and assertions; but that had us inventing the tests last.  That rule does not appear to exist.

The second rule has similar problems.

* 2) _You are not allowed to write more production code than is sufficient to..._

How do you complete that sentence?  There is no obvious limit to the amount of production code you can write.  Again, if we choose a predicate, that predicate will be arbitrary.  For example: _...complete a single function._  But, of course, that function could be huge, or tiny, or any size at all.  We have lost the obvious and natural granularity of individual behaviors and assertions.

So once again, the rule is not invertible. 

Notice that these failures of invertibility are all about granularity.  When tests come first the granularity is naturally constrained.  When production code comes first, there is no constraint.

This unconstrained granularity implies something deeper.  Note that the third law of TDD forces us to make the currently failing test, and only the currently failing test, pass.  This means that the production code we are about to write _will be derived_ from the failing test.  But if you invert the third law you end up with nonsense:

* 3) _You are not allowed to write more test code than is sufficient to pass the current production code._

What does that mean?  I can write a test that passes the current production code by writing a test with no assertions -- or a test with no code at all.  Is this rule asking us to test every possible assertion?  What assertions are those? They haven't been identified. 

This leads us to the conclusion that tests at fine granularity cannot obviously be derived from production code.

Let's state this more precisely.  It is straight forward, using the three laws of TDD, to derive the entirety of the production code from a series of individual assertion tests; but it is not straight forward to derive the entirety of a test suite from the completed production code.

This is not to say that you cannot impute tests from production code.  Of course you can.  What this is telling us is: (and anyone who has ever tried to write tests from legacy code knows this) it is remarkably difficult, if not utterly impractical, to write fine-grained, comprehensive, tests from production code.  In order to write such tests from production code, you must first understand the entirety of that production code; because any part of that production code can affect the test you are trying to write.  And second, the production code must be decoupled in a way that allows the fine granularity.  

When tests are written first, granularity and decoupling are trivial to achieve.  When tests follow production code, decoupling and granularity are much more difficult to achieve.   

### Irreversibility

This means that tests and production code are irreversible.  Accountants don't have this problem.  Debited accounts and credited accounts are mutually reversible.  You can derive one from the other.  But tests and production code progress in one direction.  

Why should this be?

The answer lies in yet another asymmetry between tests and production code: their structure.  The structure of the production code is vastly different from the structure of the test code.  

The production code forms a system with interacting components.  That system operates as a single whole. It is subdivided into components, separated by abstraction layers, and organized with communication pathways all of which support the operation, throughput, and maintainability of that system. 

The tests, on the other hand, _do not form a system_.  They are, instead, a set of unrelated assertions.  Each assertion is independent of all the others.  Each small test in the test suite stands alone.  Each test can execute on its own.  Indeed, the tests have no preferred order of execution; and many test frameworks enforce this by executing the tests in a random order.  

The discipline of TDD tells us to build the production code one small test case at a time.  That discipline also gives us guidance on the order in which to write those tests.  We choose the simplest tests at first, and only increase the complexity of the tests when all simpler tests have been written and passed.

This ordering is important.  Novices to TDD often get the ordering wrong and find that they have written a test that forces them to implement too much production code.  The rules of TDD tell us that if a test cannot be made to pass by a trivial addition or change to the production code; then a simpler test should be chosen.

Thus, the tests, and their ordering, form the assembly instructions for the production code.  If those tests are made to pass in that order, then the production code will be assembled through a series of trivial steps.

But, as we all know, assembly instructions are not reversible.  It is difficult to look at an airplane, for example, and derive the assembly procedure.  On the other hand, given the assembly procedure, an airplane can be built one piece at a time.

Thus, the conversion of the test suite into production code is a trap-door function; rather like multiplying two large prime numbers.  It can be trivially executed in one direction; but is very difficult, if not completely impractical, to execute in the other.  Tests can trivially drive production code; but production code cannot practicably drive the equivalent test suite.

### Bottom Line

What we can conclude from this is that there is a well defined discipline of test-first that is equivalent to double-entry bookkeeping; but _there is no such discipline for test-after._  It is possible to test after, of course, but there's no way to define it as a _discipline_.  The discipline only works in one direction.  Test-first.  

As I said at the start: The stakes are enormous.  Millions are waiting to be gained or lost.  Lives and fortunes are at stake.  Our businesses, and indeed our whole society, are depending upon us.  What discipline will we use to ensure that we do not let them down? 

If accountants can do it, can't we?

Of course we can.

  

