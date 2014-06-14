---
layout: post
title: Dance you Imps!
tags: ["Tools"]
---
There are several tools out there that promise to bridge the divide between objects and relational tables.  Many of these tools are of high quality, and are very useful.  They are collectively known as ORMs, which stands for _Object Relational Mappers_.  There's just one problem.  There ain't no such beast!

### The Impedance Mismatch

It all started long ago, in the hinter-times of the 1980s.  Relational databases were the big kids on the block.  They had grown from their humble beginnings, into adventurers and conquerors.  They had not yet learned to be: _THE POWER THAT DRIVES THE INTERNET (TM)_; but they were well on their way.  All new applications, no matter how humble, _had to have_ a relational database inside them.  The marketing people didn't know _why_ this was true; but that knew that is _was_ true.

And then OO burst forth upon the land.  It came as Smalltalk.  It came as Objective-C, and it came, most importantly, as C++, and then Java and C#.  OO was new.  OO was shiny.  OO was a mystery.  

Most creatures at the time feared new things.  So they hid in caves and chanted spells of protection and exorcism.  But there were imps in the land, who thrived by embracing change.  The imps grabbed for the shiny new OO bauble and made it their own.  They could _feel_ its power.  They could _sense_ that it was good -- very, very good; but they couldn't explain _why_.  So they invented reasons.  Reasons like: _It's a closer way to model the real world!_ (What manager could say no to _that_?)

Then one day, when the OO imps were just minding their own business, doing their little dance of passing messages back and forth to each other, the RDBMS gang (pronounced _Rude Bums_) walked up to them and said: "If you imps want to do your impy little dance of passing your stupid messages back and forth in _our_ neighborhood, then your bits are ours.  You're going to store them with us.  Capisce?"

What could the OO imps say?  The _Rude Bums_ ruled the roost; so, of course, they agreed.  But that left them with a problem.  The _Rude Bums_ had some very strict, _and very weird_, rules about how bits were supposed to be stored.

These rules didn't sit well with the imps.  When the imps did their impy message passing dance, they just sort of threw the bits back and forth to each other.  Sometimes they'd hold the bits in their pockets for awhile, and sometimes they'd just toss them over to another impy dancer. In a full fledged impy dance, the bits just zoomed around from dancer to dancer in a frenzy of messages.

But the _Rude Bums_ demanded that the bits be stacked up on strictly controlled tables, all kept in one big room.  They imposed very strict rules about how those bits could be arranged, and where they had to be placed.  In fact, there were forms to fill out, and statements to make, and transactions to execute.  And it all had to be done using this new street banter named SQL (pronounced _squeal_).  

So all the OO imps had to learn _squeal_, and had to stack their bits on the _Rude Bums_ tables, and had to fill out the forms and do the transactions, and that just didn't match their dancing style.  It's _hard_ to throw your bits around in the impy dance when you've got to stack your bits on tables while speaking _squeal_! 

This was the beginning of the Impy Dance Mismatch between OO and the _Rude Bums_.

### ORMs to the rescue, _Not!_

The next decade saw a huge increase in the political power of the _Rude Bums_.  They grabbed more and more territory, and ruled it with an iron fist.  The imps also gained territory; possibly more than the _Rude Bums_; but they never managed to break free of the _Rude Bums'_ rules.  And so they forgot how to dance.  The free and lively OO dance they had done at the very beginning faded from their memory.  It was replaced by a lock-step march around the _Rude Bum's_ tables.

Then, one day, some strangers appeared.  They called themselves ORM (the _OutRaged Mongrels_).  The _Mongrels_ had seen the free OO dancing of the imps before the _Rude Bums'_ took control of them; and the _Mongrels_ longed to see the imps dance free again.  

So they came up with a plan.  _They_ would do the _squealing_!  _They_ would arrange the bits on the tables.  _They_ would fill out the forms and execute the transactions.  _They_, the _Mongrels_, would stand between the imps and the _Rude Bums_ and free the imps to dance once again.

"Oh _dance free_ you imps, _dance free!_"

But the imps _didn't_ dance free. They kept right on doing their lock-step march.  Oh they were happy to have someone else take care of the nasty job of speaking _squeal_ and arranging bits on the tables.  They were happy that they didn't have to deal directly with the _Rude Bums_.  But now, instead of marching around the _Rude Bum's_ tables, they marched around the "Mongrels'" cabinets (which looked an awful lot like the _Rude Bums'_ tables). 

The Impy Dance Mismatch between OO and the _Rude Bums_ had simply changed to the Impy Dance Mismatch between OO and ORMs.

### Their ain't no such mapping.

An object is not a data structure.  Repeat after me: _An Object Is Not A Data Structure._  OK, you keep repeating that while I keep talking.  

An object is not a data structure.  In fact, if you are the consumer of an object, you aren't allowed to see any data that might be inside it.  And, in fact, the object might have no data inside it at all.  

What do you see when you look at an object from the outside?  You see _methods!_  You don't see any data; because the data (if any) is kept private.  All you are allowed to see, from the outside looking in, are methods.  So, from the outside looking in, an object is an abstraction of behavior, not an abstraction of data.

How do you store an abstraction of behavior in a database?  Answer: _You don't!_  You can't store behavior in a database.  And that means _you can't store objects in a database._  And that means there's no Object to Relational mapping!

### OK, now wait!

Yeah?  Wait for what?  You want to argue that point?  Really?  You say you've got an _account_ object stored in the database, and you use hibernate to bring it into memory and turn it into an object with methods?  

_Balderdash!_  Hibernate doesn't turn _anything_ into an object.  All Hibernate does is to migrate data structures that are stored on a disk (a what?  You still using disks?  No wonder you're confused.) to data structures stored in RAM.  That's all.  Hibernate changes the form and location of data structures.  Data structures, not objects!

What is a data structure?  It's a bunch of public data with no methods.  Compare that to an object which is a bunch of public methods with no visible data.  These two things are the exact opposites of each other!

I could get into a whole theoretical lecture on the nature of data structures and objects, and polymorphism, and switch statements, and...   But I won't.  Because the point of this article is simply to demonstrate that ORMs aren't ORMs.

###What are ORMs?

ORMs are data structure migrators.  That's all.  They move data from one place to another while making non-semantic changes to the form of that data.  They do _NOT_ create objects out of relational tables.

### Why is this important?

It's important because the imps aren't dancing free!  Too many applications are designed such that the relational schema is bound, in lock-step, to the business objects.  The methods on the business objects are partitioned according to the relational schema.

Think about that for a minute.  Think about that, and then weep.  Why should the message pathways of an application be bound to the lines on a E-R diagram?  What does the behavior of the application have to do with the structure of the data?

Try this thought experiment.  Assume that there is no database.  None at all.  There are no tables.  No schema.  No rows.  No SQL.  Nothing.  

Now think about your application.  Think about the way it behaves.  Group similar behaviors together by responsibility.  Draw lines between behaviors that depend on each other.  Do you know what you'll wind up with?  You'll wind up with an object model.  And do you know what else?  It won't look much like a relational schema.

_Tables are not business objects!_  Tables aren't objects at all.  Tables are just data structures that the true business objects use as a resource.

###Moral

So, designers, feel free to use ORMs to bring data structures from the disk (the disk?  You still using one?) into memory.  But please don't think of those data structures as your business objects.  What's more, please design your business objects without consideration for the relational schema.  Design your applications to _behave_ first.  _Then_ figure out a way to bind those behaviors to the data brought into memory by your ORM.

















