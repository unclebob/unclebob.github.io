---
layout: post
title: "When TDD doesn't work."
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2014/04/30/When-tdd-does-not-work.html" />
Over the years many people have complained about the so-called "religiosity" of some of the proponents of Test Driven Development.  The [recent brouhaha]({% post_url 2014-04-25-MonogamousTDD %}) over TDD has, once again, brought these complaints to the fore.  So I thought it would be a good idea to talk about when TDD does not work.

I have often compared TDD to double-entry bookkeeping.  The act of stating every bit of logic twice, once in a test, and once in the production code, is very similar to the accounting practice of entering every transaction twice, once on the asset side, and once on the liability side.  The running of the tests is very similar to the creation of the balance sheet.  If the balance of assets and liabilities isn't zero, somebody made a mistake somewhere.

So stating that there are places that TDD doesn't work may seem like stating that there are places where double entry bookkeeping doesn't work.  However, software is different from accounting in one critical way: software controls machines that physically interact with the world.  

For example, let's say that I am writing a program that controls a machine that has a bell.  The software must ring the bell when certain events occur.  How can I test that my software rings the bell?

The only way to actually test that the software rings the bell is to connect a microphone to my computer and write some code that can detect the ringing of a bell.  

Well, no, that's not right.  I could test that the software rings the bell by listening.  In other words, I can test that manually.  

Now, I can write unit tests that mock out the bell driver, and I can test that my software sends the appropriate signals to that driver at the appropriate times.  I can write unit tests that prove that the software _should_ ring the bell.  But if I want to be sure that the bell rings when the proper signals are sent to the driver, I either have to set up that microphone or just listen to the bell. 

How can I test that the right stuff is drawn on the screen?  Either I set up  a camera and write code that can interpret what the camera sees, or I look at the screen while running manual tests.  

Now, I can mock out the screen and test that my software sends the right signals to the screen driver.  I can test that my software _should_ put the right stuff on the screen.  But if I want to be absolutely sure, I have to either set up that camera, or look at the screen.

You can see where I'm going with this, can't you.  It's the stuff out at the boundaries of the system.  It's the IO devices that require manual testing.  At the moment the software controls something that physically interacts with the world, automated tests become so impractical that manual tests are the best option.

But what about the layer just before the physical world?  Can you write automated tests for that layer? 

Consider CSS.  Can you write a test that ensures that the CSS for a page is correct?  Yes, you can, but it's almost certainly a waste of time.  The reason is that in order to write that test you have to know the contents of the CSS.  If you want to test that the width for a certain element is 5px, then 5px must appear both in the CSS and the test.

Remember the TDD rule: _As the tests get more specific, the code gets more generic._  Every new test case makes the test suite more constrained and more specific.  To make the new test case pass, the programmer strives to make the production code more general, not more specific.  We don't pass tests by adding if statements that correspond to each test.  We pass tests by innovating general algorithms.

But CSS doesn't work like that.  There is no general algorithm for CSS.  The CSS is just as specific as any test you could write.  Indeed, you could write a program that reads the CSS and writes the tests.  Such tests add very little value, and they certainly aren't written first.

Besides, how do you know if the CSS is correct?  Remember we are doing TDD.  We are writing our tests first.  How do you know, in advance, what the CSS should be?  The answer is that usually you don't.  Usually you write some initial CSS, and then you look at the screen and _fiddle_ with the CSS until it looks right.  Your eyes, and your mind, are the actual test.  Once the CSS is right, there's no point in writing a test for it.

So near the physical boundary of the system there is a layer that requires _fiddling_.  It is useless to try to write tests first (or tests at all) for this layer.  The only way to get it right is to use human interaction; and once it's right there's no point in writing a test.

So the code that sets up the the panels, and windows, and widgets in Swing, or the view files written in yaml, or hiccup, or jsp, or the code that sets up the configuration of a framework, or the code that initializes devices, or...  Well you get the idea.  Anything that requires human interaction and _fiddling_ to get correct is not in the domain of TDD, and doesn't require automated tests.   

So, now we have two places where TDD is impractical or inappropriate.  The physical boundary, and the layer just in front of that boundary that requires human interaction to _fiddle_ with the results.  Are there any other areas where tests aren't appropriate?

Yes.  The test code itself.  I don't mean the actual unit tests.  I mean the support code for those unit tests.  The FitNesse fixtures, or the cucumber steps, or the Object Mothers, or the Test Doubles.  You don't have to write tests for those because the actual unit tests and the production code _are_ the tests for those pieces of code.  

That's really about it.  For pretty much everything else you can write tests, and you can write them first.  For pretty much everything else, you can use TDD. 

However, there's one other rule.  It's not fair to load those layers with logic just so you can avoid writing tests for that logic.  Indeed, it is imperative to denude these layers of logic, and export that logic to modules that you can test.

This exporting of logic from the boundaries of the system, and from the fiddling layers next to those boundaries has a name.  It's called _[Humility](http://xunitpatterns.com/Humble%20Object.html)_.   We keep these layers humble by moving all the logic associated with them out into other modules for which we can easily write tests.  

This means you don't put any unnecessary logic into your JSP files, or you Swing setup code, or your yaml files.  You keep that code humble by moving logic into other modules that can be tested.

It has been claimed that this exporting of logic is damaging to the design of the application.  I disagree.  From my point of view, exporting logic is nothing more than separating concerns.  In this case we separate the code that must be _fiddled_ from the code that can be tested.  Those two domains of code will change for very different reasons and at very different rates; so it is wise to separate them.  Separating them is _good_ design.


