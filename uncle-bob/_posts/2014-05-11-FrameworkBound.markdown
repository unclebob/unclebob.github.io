---
layout: post
title: "Framework Bound[2]"
tags: ["Craftsmanship"]
---
Frameworks are powerful tools.  We'd be lost without them.  But there's a cost to using them.

Think of Rails, or Spring, or JSF, or Hibernate.  Think about what writing a web system would be like without these frameworks to help you.  The idea is disheartening.  There'd be so many little piddling details to deal with.  It'd be like endeavoring to construct a mnemonic memory circuit using stone knives and bearskins[1].

And so we happily couple our code to those frameworks in anticipation of all the benefits they promise.  We make the mistake that so many programmers have made before us.  We bind ourselves to the framework.

Using a framework requires a significant commitment.  By accepting the framework into your code, you surrender your control over the details that the framework manages.  Of course this seems like a good thing; and it usually is.  However, there's a trap waiting around the corner; and it's hard to see it coming. Before you know it you find yourself engaged in all manner of unnatural acts, inheriting from it's base classes, relinquishing more and more of the flow of control, bowing ever deeper to the framework's conventions, quirks, and idiosyncrasies.  

And yet despite the huge commitment you've made to the framework, the framework has made no reciprocal commitment to you at all.  That framework is free to evolve in any direction that pleases its author.  When it does, you realize that that you are simply going to have to follow along like a faithful puppy. 

Getting bound to frameworks is an all too common occurrence in software teams.  They begin with rampant enthusiasm and willingly couple their code to the framework, only to find, much later, that as the project matures, the framework gets more and more in the way.

This shouldn't be surprising.  Frameworks are written by people to solve certain problems that _they_ have.  Those problems may be similar to yours, _but they are not yours_.  You have different problems.  To the extent that your problems overlap, the framework can be enormously helpful.  To the extent that your problems conflict, the framework can be a huge impediment.  

TANSTAAFL!

###Framework Authors
Remember that frameworks are written by people, and people have their own agendas.  One of the items on such an agenda is to get you to use their framework.  As an author of past frameworks I can tell you that the more people who used my frameworks, the better I felt about _myself_.  A lot of my self-worth got tied up into the acceptance of my frameworks.  

Now I don't want to get too deep into the psycho-analysis of framework authors.  Framework authors are not bad people.  Indeed, for the most part, they are heroes.  Many unselfishly release their code to the open-source community.  Were it not for these people, our programming lives would be far less enjoyable and productive.

I've made this point about authors because it helps me understand some of the mechanics of the relationship between framework authors and framework users.  

Framework authors will go to great lengths to entice you into the fold.  They'll write papers and give talks.  They'll provide examples showing you how to bind tightly, ever tightly to their code.  They'll demonstrate all the benefits that tight binding gives you.  They are convinced that their code can help you, and they work hard to convince you too.

That's perfectly normal, and not at all dishonorable.  They want you in the fold.

Once you are in the fold, however, their interest in you might change somewhat.  Here's a picture of one famous framework author telling his users what he thinks about some of their concerns. [(R rated)](https://www.flickr.com/photos/planetargon/127984254/) 

###Arm's Length.
Over the years, I've adopted a healthy skepticism about frameworks.  While I acknowledge that they can be extremely useful, and save a boatload of time; I also realize that there are costs.  Sometimes those costs can mount very high indeed.  

So my strategy is to keep frameworks like Spring, Hibernate, and Rails at arm's length; behind architectural boundaries.  I get most of the benefit from them that way; and I can take ruthless advantage of them. 

But I don't let those frameworks get too close.  I surrender none of my autonomy to them.  I don't allow the tendrils of their code to intermingle with the high level policy of my systems.  They can touch my peripheral subsystems; but I keep them away from the core business logic.  The high level policies of my systems shall never be touched by frameworks.
 

-----
[1] Star Trek: _The City on the Edge of Forever_, Harlan Ellison, 1967  
[2] [Apology](https://gist.github.com/unclebob/2abcce451bafeab421f2)







 



