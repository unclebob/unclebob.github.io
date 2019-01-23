---
layout: post
title: "Future Proof"
tags: ["Craftsmanship"]
---
I recently read Christin Gorman's blog [Future Proof](http://kranglefant.tumblr.com/post/131808192355/future-proof).  In it she lambastes the idea that you can create code that is protected from future change.  

>_Awesome or not, some things are impossible.  Cordless garden hoses are impossible. So is software that changes without being changed. If you need it to adapt to future requirements, then guess what: that involves adaptation -- AKA change._

She's right about that, of course.  When the requirements change, either some code, or some data is going to have to change.  The Open-Closed Principle cannot apply to every element of the system.  No matter how open you make your system to extension, something, somewhere will need modification.

### External Data

Indeed, Gorman rants rather poetically about the notion that changes to an .xml file, or a database table, are somehow better than changes to source code.  

>_Moving changes outside the code itself does in NO WAY stop you from having to make changes. It does however create extra complexity in the code, while limiting the types of changes you can make. It also makes it much harder to track the changes made, who made them, when were they made and by whom. So if you like the idea of allowing random changes in your production environment, with limited accountability or ability to keep track of what's going on -- by all means move all your settings to your database, or properties files. Have fun._

At this point you are probably saying to yourself: "Yeah, but..."

Let's explore that "but..."

In most systems there are some system parameters that vary between different installations.  Such parameters clearly belong outside the code.  We don't want to have to compile different versions of the code for different customers[1].  

In fact most systems have certain parameters that the users want to change on a whim without involving the developers who compile the system.  Consider for example, a security system that sends a text message to a security guard whenever there is a security event.  It would be a real shame if the supplier of that security system had to recompile and redeploy their code every time the security guard's phone number changed.

So, for the sake of argument, let's assume that Gorman would acknowledge that there are some data elements that belong outside of the application and in a text file, or an xml file or a properties file.  What rule can we use to tell which data elements should be inside the code, and which should be outside the code. 

The answer to that is trivial.  Anything that the programmers don't want to be bothered with on a regular basis should be outside the code.  What kinds of things bother the programmers?  Things that change frequently.  

So our rule is: _Anything that changes frequently should be outside the code._

### Decoupling Modules

Gorman continues.  

>_If you want a future proof system, you don't want immortal and flexible code. You don't want the T1000 terminator. You want Southpark's Kenny. You need code that's easy and fun to kill. You need to get used to killing it, often, so you can replace it with whatever you end up needing._ 

This is generally very good advice.  It is better to create a _changeable_ system than to try to protect all parts of the system from change.  However, there are issues with creating code that's easy to change (or "kill").

If killing or modifying a particular module causes many others modules to break, either at compile time, or at test time, then the _cost_ of making that change (the impact) is going to be high.  Sometimes it can be _very_ high.  I have worked on systems where the impact of certain changes was so prohibitive that they were delayed for years.  

What causes this to happen?  Why does changing certain modules affect others?  Dependencies, of course.

When one module depends upon the internals of another; then when those internals change, both modules will require changes.  Dependencies like this can propagate through a system making it very hard to change.  So certainly we'd like to mitigate this by somehow decoupling the modules that need to be changed, from the rest of the system. 

Which modules should be decoupled?  I think the rule is similar to the previous rule:  _Any module that changes frequently should be decoupled from the rest of the system._ 

How do you decouple one module from another?  That depends on the level of decoupling you need.  Sometimes simply extracting that code into a separate function is enough.  More often, it's better to move all the related code into a separate class, and even a separate source file.  And in extreme cases, you want to put those classes behind polymorphic interfaces.  

### Interfaces
And this is where I part company from Gorman to a certain extent.  Because she goes on:

>_Java interfaces are meant to be used when there are a bunch of implementations available, and your code wants to access them all in the same manner._

And again...

>_Interfaces with only one implementation are the committees of code. If you don't want to make a decision yourself, if you're worried about being blamed if it was wrong, you delegate it to a committee._

Actually she rants much longer about this; and even refers to the satirical [EnterpriseFizzBuzz](https://github.com/EnterpriseQualityCoding/FizzBuzzEnterpriseEdition) as an example of code that is "not far from the reality out there".  

In general I agree that using polymorphic interfaces without good reason is overkill.  But I am not at all opposed to having interfaces with a single implementation.  Sometimes the decoupling that provides is exactly right for isolating a module that changes frequently.  One should not look at Enterprise Fizz Buzz and conclude that interfaces should be avoided at all costs.

### Tests

Finally, as she continues her rant against interfaces, Gorman asserts that interfaces don't make testing easier. 

>_Some will tell you that interfaces are great for making your code testable.  No they aren't. They do no harm, but they don't help either. You don't need interfaces to create mocks or stubs or spies. Use Mockito or any other sensible mocking framework, and you can easily create mocks for concrete classes. You should also ask yourself why you need those mocks or stubs or spies -- with a little rework of your code, you might be able to write tests with very little mocking:_   

I can sympathize with that last point.  I think that many software teams use mocks more than they should.  I use mocks with a certain parsimony.  I will mock; but usually only across significant architectural boundaries.   I don't mock every class and every function.  But when I am facing a significant boundary, mocks are very useful, and therefore interfaces become essential.

About Gorman's middle point, that you can always use a mocking tool like Mockito in order to avoid interfaces,  I'll say two things.

1. The choice to use a tool, like Mockito, should not be motivated by a resistance to interfaces.  Interfaces should not be actively avoided.  Being restrained is not the same as being repelled.

2. I don't often use mocking tools because I write my own mocks.  So I find interfaces quite helpful.

That last point may strike some of you as odd; but it's true.  Mocks are very easy to hand write; and hand written mocks can be given nice names, and placed in nicely named packages, and nicely named source files, and nicely named directories.  Hand written mocks don't pollute your setups with random sequences of dots and parentheses.  So, unless I need a mocking tool's super powers, I tend not to use them.

So in the end, I don't completely agree with Gorman's initial assertion.  Interfaces may not always make testing easier; but at certain boundaries they are absolutely essential.  

### Bottom Line
For the most part, I think Gorman made some good points.  The goal is not to "Future Proof" your code.  The future will aways find a way to thwart you.

However, that doesn't mean that you shouldn't arrange your code to minimize the impact of frequent change.  And if you can do that by externalizing certain data elements, and putting certain modules behind polymorphic interfaces, there's no reason you shouldn't.   Decoupling shouldn't be gratuitous; but it's not something to actively avoid.  Indeed, strategic decoupling in moderation is a very, very good thing.

Finally, Gorman herself acknowledges that if you don't make things easy to change, the cost can be very high.  

>_ In Norway our parliament voted to change our criminal laws in 2005. But they have only now (2015) been put into full effect, because the police's computer systems prevented them from applying the new rules._





----

[1] Believe it or not we used to do this all the time.  Each installation would have a separately compiled version of the code, complete with their own configuration data that was compiled into the application.  We wound up with an entire group of _configuration engineers_ whose job it was to keep the configuration data, software versions, and compiled binaries straight.  It was a nightmare.
