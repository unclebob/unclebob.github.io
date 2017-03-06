---
layout: post
title: Testing Like the TSA
tags: ["Software"]
---
I was very glad to read in DHH's [recent post](https://signalvnoise.com/posts/3159-testing-like-the-tsa) that he is actually still using TDD.  I'm glad he has realized that TDD is not, in fact, [dead](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html).  

This blog is a simple response; just to state a couple of disagreements.  But I have to say, I agree more than I disagree.  

DHH presented seven points.  I have reproduced them below, along with my comments.  And since DHH did not justify his opinions, I'll not justify mine.

 * (1. Don’t aim for 100% coverage.

>_Disagree.  Aim as high as you can.  Treat 100% as an asymptotic goal.  No other number makes a reasonable goal.  There is never a reason to stop driving your coverage number higher._

 * (2. Code-to-test ratios above 1:2 is a smell, above 1:3 is a stink.

>_Disagree.  1:1 LOC is about right.  If you have 20K lines of code, you probably ought to have about 20K lines of tests.  (BTW, I can't make any sense of DHH's ratios there.  I think he meant the first one the other way around (2:1) and I think he meant the second one to be (1.5:1).  Or maybe I'm just dense._

 * (3. You’re probably doing it wrong if testing is taking more than 1/3 of your time. You’re definitely doing it wrong if it’s taking up more than half.

>_Agree.  Testing should take none of your time.  Writing tests, with TDD, requires negative time (i.e. it saves you a boatload)  Every worthwhile test you don't write costs you time._

 * (4. Don’t test standard Active Record associations, validations, or scopes.

>_Agree.  There's generally no reason to test your framework; so long as you trust it.  If you don't trust it (and there are many frameworks out there that are not trustworthy) then writing some tests against the framework might be worthwhile.  Consider it equivalent to "Incomming Inspection"._

 * (5. Reserve integration testing for issues arising from the integration of separate elements (aka don’t integration test things that can be unit tested instead).

>_Agree.  Keep your tests focused.  Don't test through UIs.  Don't test through web servers.  Test as close to the code as you can._ 

 * (6. Don’t use Cucumber unless you live in the magic kingdom of non-programmers-writing-tests (and send me a bottle of fairy dust if you’re there!)

>_Agree and Disagree.  Cucumber (Gherkin) is worth it only if you have business people and/or QA who are willing to read your tests.  If they will also write your acceptance tests then: ABSOLUTELY send that fairy dust far and wide; because it's worth its weight in diamonds._

 * (7. Don’t force yourself to test-first every controller, model, and view (my ratio is typically 20% test-first, 80% test-after).

>_Agree...  Sort of.  Some controllers, models, and views are too stupid to bother to test.  If they are obviously correct, because they are one line of code, then testing them might be superfluous.  But be careful.  Sometimes one line of code has 20 lines of semantics._


