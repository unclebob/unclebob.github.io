---
layout: post
title: "Test Induced Design Damage?"
tags: ["Craftsmanship"]
---
I miss Jim Weirich.  I miss his laugh.  I miss his good nature.  Most of all, I miss what he would have taught me next year, and in the years to follow.  I _feel_ that loss.

Last October Jim gave a talk named [Decoupling from Rails](https://www.youtube.com/watch?v=tg5RFeSfBM4) in which he showed how to refactor a rails app in order to decouple business logic from rails framework code.  The talk is _spectacular_.  The hour you spend watching it will pay you back many times over.  

At the end of his talk Jim stated his motivation for giving the talk.  He said:
>"The thing I want to stress is that: I don't think Rails is evil.  I don't think it's a bad framework.  I think that as applications grow what it gives you by default is not good for growth."

What Rails programmer of a growing system hasn't discovered _that_?

Recently, I read [Test Induced Design Damage](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html) by DHH.  In it he refers to Jim's talk, and then asserts that Jim was damaging the design of his application.  

That's not what I saw.  Not at all.  What I saw was a tightly interwoven mass of rails and business logic being teased apart by a master of his craft.  The result was, frankly, _beautiful_.  At the end of his talk, the students began to realize all the options this new structure gave them.  You can see Jim's eyes light up as he sees that his message has gotten through, and as he amplifies their observations with even more of his own.   

What Jim did was to, very elegantly, separate concerns.  The separation of concerns is an old design principle, first explained to us by David Parnas in his 1972 paper: [On the Criteria To Be Used in Decomposing Systems into Modules](http://dl.acm.org/citation.cfm?id=361623).  I recommend giving that paper a good read.  If you do you'll note that the  modularization scheme that Parnas recommended is quite consistent with Jim's refactoring.  

As Parnas notes, one of the primary benefits of separating concerns is changeability.  Listen to the students at the end of Jim's talk as they remark about how the decoupled business logic could now be invoked through a service instead of over the web or how the data could be fetched from a source other than the DB.  They are talking about changeability.

How do you separate concerns?  You separate behaviors that change at different times for different reasons.  Things that change together you keep together.  Things that change apart you keep apart.  

The code in a Rails app that binds the business rules to the Rails framework changes for different reasons, and at different rates, than the business rules themselves.  Putting those business rules into Rails controllers or Rails models therefore violates Parnas' principle.  

GUIs change at a very different rate, and for very different reasons, than business rules.  Database schemas change for very different reasons, and at very different rates than business rules.  Keeping these concerns separate is _good_ design.

How does testing play into this?  Jim noted several times in his talk that once he had separated a concern, he could test that concern more easily, and the test would run faster because it wasn't coupled to the Rails framework.  DHH contends that by focusing on test speed Jim was damaging his design.  But Jim would have done this separation no matter how fast the tests were running.  Jim would have done it because it enhanced changeability, and made the business rules much clearer than they were before.  Jim's separation _improved_, it did not damage, the design of the code. 

The primary thesis of DHHs paper is that programmers who "faithfully" practice TDD will create code that is "warped out of shape solely to accommodate testing objectives".  In his paper, DHH does not actually show any examples of this.  Rather he refers to Jim's talk.  Yet Jim's talk does not show someone who is warping their code out of shape.  On the contrary it shows a faithful TDDer vastly improving the shape of his code.  

Now of course tests do run faster when you separate concerns.  It's easy to see why.  If you aren't coupled to a spinning disk, you'll run faster.  If you aren't coupled to an SQL interpreter you'll run faster.  If you don't have to send data over a web socket you'll run faster.  If you aren't coupled to a framework that has a long load time you'll run faster.  You pick it.  If you aren't coupled to it, you'll run faster.  So if your tests run slowly, it is an indication that you have not separated concerns, and that therefore your design is lacking.  

Is it possible to warp your code out of shape solely to increase test speed?  I suppose it might be.  I don't know what that might look like, and I don't want to know.  Perhaps that was what DHH was talking about, and not the separation of concerns.  However, DHH pointed specifically at Jim's talk, and described Jim's refactorings as: "needless indirection and conceptual overhead".  What he's referring to is Jim's definition of architectural boundaries, and his discipline of managing the dependencies that cross those boundaries.  In other words: The separation of concerns.  I find it hard to accept Jim's separations as "warping".

To conclude: It seems to me that using good design principles that make your tests run faster is a noble goal. It also seems to me that decoupling from frameworks such as Rails, as your applications grow, is a wise action.  I believe these things to be evidence that professionals, like Jim Weirich, are at work.  
