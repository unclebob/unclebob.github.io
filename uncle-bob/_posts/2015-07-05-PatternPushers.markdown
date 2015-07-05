---
layout: post
title: "Pattern Pushers"
tags: ["Craftsmanship"]
---
There were a number of interesting tweets in response to [The Little Singleton](http://blog.cleancoder.com/uncle-bob/2015/07/01/TheLittleSingleton.html).  Here are a few.

Some complained that Singleton was bad because Singletons are global variables.  

It is true that Singletons involve using global variables.  However, despite Martin Fowler's oft referenced imitation of me, global variables are not a sin.  Most systems have a few globals that are used to hold the roots of their data structures, factories, and other important resources.  This is normal, and by no means sinful.  And, if you have a public API to protect, a Singleton can be a useful way to manage a global.  

Others complained that Singletons don't work well in distributed environments.  I Imagine that might be true in some distributed environments.  In others, I think the notion of a singular object is useful.  Of course this depends on what we mean by singular.  Singular to the whole distributed system?  Or singular within each process of the distributed system.  The Singleton pattern works well for the latter.  The former is another matter altogether.  

Another complaint was that there is documentation value in the `instance()` method, and the `private` encapsulation of the `instance` variable.  That's true enough; but I prefer the public `instance` variable when working in a small trusted team.

Someone else was rather upset at my notion that trust is a factor and thought that I was violating _Clean Code_ by not being as explicit as possible about all the rules.  But in small trusted teams implicit rules are a huge efficiency gain that I am loathe to abandon.

One of the strangest tweets I saw in response to was the article was:

>_This is supposed to be clever. Pattern pushers are full of hot air_

What the devil is a _Pattern Pusher_?

>_Hey, bud, c'mere.  Ya wanna get high?  I've got some el primo Visitor Pattern, direct from Columbia.  Man, this stuff will double dispatch you right to cloud nine!  I've got too much of this stuff right now, so you can have this hit for free.  Come back later and maybe I'll have some real hot Memento pattern for you._

So let's be clear.  It's a good idea to _learn_ patterns.  It is not a good idea to hunt for places to _use_ patterns.  Instead, if you _know_ the patterns well, then you will find places in your systems where they fit naturally.  Then, if you use the pattern names and canonical forms, you provide a kind of automatic documentation to others on your team who know those patterns.  

I mean, if I see a class named `ReportVisitor`, I immediately know what the author's intent was, and what the structure of the code is.

Am I a _Pattern Pusher_?  I suppose I am, inasmuch as I strongly recommend that people learn them.  But that's no different from an electronics engineer learning the names and forms of common electrical circuits, or a sailor learning the names and forms of common sailing knots.  

I'm not sure I'll ever understand what people have against learning things.  

