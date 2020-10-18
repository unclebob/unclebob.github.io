---
layout: post
title: Solid Relevance
tags: ["Software"]
---
Recently I received a letter from someone with a concern.  It went like this:

----

>_For years the knowledge of the SOLID principle has been a standard part of our recruiting procedure. Candidates were expected to have a good working knowledge of these principles.  Lately, however, one of our managers, who doesn't code much anymore, has questioned whether that is wise.  His points were that the Open-Closed principle isn't very important anymore because most of the code we write isn't contained in large monoliths and making changes to small microservices is safe and easy.  The Liskov Substitution Principle is long out of date because we don't focus on inheritance nearly as much as we did 20 years ago.  I think we should consider [Dan North's position on SOLID](https://speakerdeck.com/tastapod/why-every-element-of-solid-is-wrong) -- "Just write simple code."_

----
	
I wrote the following letter in response:

The SOLID principles remain as relevant to day as they were in the 90s (and indeed before that).  This is because software hasn’t changed all that much in all those years — and __that__ is because software hasn’t change all that much since 1945 when Turing wrote the first lines of code for an electronic computer.  Software is still `if` statements, `while` loops, and assignment statements — _Sequence_, _Selection_, and _Iteration_.

Every new generation likes to think that their world is vastly different from the generation before.  Every new generation is wrong about that; which is something that every new generation learns once the next new generation comes along to tell them how much everything has changed. `<grin>`  

So let’s walk through the principles, one by one.  

__SRP__) The Single Responsibility Principle.  

>_Gather together the things that change for the same reasons.  Separate things that change for different reasons._  

It is hard to imagine that this principle is not relevant in software.  We do not mix business rules with GUI code.  We do not mix SQL queries with communications protocols.  We keep code that is changed for different reasons separate so that changes to one part to not break other parts.  We make sure that modules that change for different reasons do not have dependencies that tangle them.  

Microservices do not solve this problem.  You can create a tangled microservice, or a tangled set of microservices if you mix code that changes for different reasons.  

Dan North's slides completely miss the point on this, and convinces me that he did not understand the principle at all.  (or that he was being ironic, which knowing Dan, is far more likely)  His answer to the SRP is to “Write Simple Code”.  I agree.  The SRP is one of the ways we keep the code simple. 

__OCP__) The Open-Closed Principle.  

>_A Module should be open for extension but closed for modification._  

Of all the principles, the idea that anyone would question this one fills me full of dread for the future of our industry.  Of course we want to create modules that can be extended without modifying them.  Can you imagine working in a system that did not have device independence, where writing to a disk file was fundamentally different than writing to a printer, or a screen, or a pipe?  Do we want to see `if` statement scattered through our code to deal with all the little details?

Or…  Do we want to separate abstract concepts from detailed concepts.  Do we want to keep business rules isolated from the nasty little details of the GUI, and the micro-service communications protocols, and the arbitrary behaviors of the database?  Of course we do!  

Again, Dan’s slide gets this completely wrong.  When requirements change only _part_ of the existing code is wrong.  Much of the existing code is still right.  And we want to make sure that we don’t have to change the right code just to make the wrong code work again.  Dan’s answer is “write simple code”.  Again, I agree.  And, ironically, he is right.  _Simple code is both open and closed_.

__LSP__) The Liskov Substitution Principle.  

>_A program that uses an interface must not be confused by an implementation of that interface._

People (including me) have made the mistake that this is about inheritance.  It is not.  It is about sub-typing.  All implementations of interfaces are subtypes of an interface.  All duck-types are subtypes of an implied interface.  And, every user of the base interface, whether declared or implied, must agree on the meaning of that interface.  If an implementation confuses the user of the base type, then `if/switch` statements will proliferate.  

This principle is about keeping abstractions crisp and well-defined.  It is impossible to believe that this is an outmoded concept.  

Dan’s slides are entirely correct on this topic; he simply missed the point of the principle.  Simple code is code that maintains crisp subtype relationships.

__ISP__) The Interface Segregation Principle.  

>_Keep interfaces small so that users don’t end up depending on things they don’t need._  

We still work with compiled languages.  We still depend upon modification dates to determine which modules should be recompiled and redeployed.  So long as this is true we will have to face the problem that when module A depends on module B at _compile_ time, but not at run time, then changes to module B will force recompilation and redeployment of module A.

This issue is especially acute in statically typed languages like Java, C#, C++, GO, Swift, etc.  Dynamicaly typed languages are affected much less; but are still not immune.  The existence of Maven and Leiningen are proof of that.  

Dan’s slide on this topic is provably false.  Clients _do_ depend on methods they don’t call, if they have to be recompiled and redeployed when one of those methods is modified.  Dan’s final point on this principle is fine, so far as it goes.  Yes, if you can split a class with two interfaces into two separate classes, then it is a good idea to do so (SRP).  But such separation is often not feasible, nor even desirable.

__DIP__) The Dependency Inversion Principle.  

>_Depend in the direction of abstraction. High level modules should not depend upon low level details._

It is hard to imagine an architecture that does not make significant use of this principle.  We do not want our high level business rules depending upon low level details.  I hope that is perfectly obvious.  We do not want the computations that make money for us polluted with SQL, or low level validations, or formatting issues.  We want isolation of the high level abstractions from the low level details.  That separation is achieved by carefully managing the dependencies within the system so that all source code dependencies, especially those that cross architectural boundaries, point towards high level abstractions, not low level details.

In every case Dan’s slides end with: _Just write simple code_.  This is good advice.  However, if the years have taught us anything it is that simplicity requires disciplines guided by principles.  It is those principles that define simplicity.  It is those disciplines that constrain the programmers to produce code that leans towards simplicity.  

The best way to make a complicated mess is to tell everyone to “just be simple” and give them no further guidance.
