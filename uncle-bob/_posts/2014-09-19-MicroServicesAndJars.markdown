---
layout: post
title: "Microservices and Jars"
tags: ["Programming"]
---
One of my clients recently told me that they were investigating a micro-service-architecture.  My first reaction was: "What's that?"  So I did a little research and found Martin Fowler's and James Lewis' [writeup](http://martinfowler.com/articles/microservices.html) on the topic. 

So what is a micro-service?  It's just a little stand-alone executable that communicates with other stand-alone executables through some kind of mailbox; like an http socket.  Lots of people like to use REST as the message format between them.

Why is this desirable?  Two words.  _Independent Deployability_.

You can fire up your little MS and talk with it via REST.  No other part of the system needs to be running.  Nobody can change a source file in a different part of the system, and screw your little MS up.  Your MS is immune to all the other code out there.  

You can test your MS with simple REST commands; and you can mock out other MSs in the system with little dummy MSs that do just what your tests need them to do.  

Moreover, you can control the deployment.  You don't have to coordinate with some huge deployment effort, and merge deployment commands into nasty deployment scripts.  You just fire up your little MS and make sure it keeps running.  

You can use your own database.  You can use your own webserver.  You can use any language you like.  You can use any framework you like.

Freedom!  Freedom!  

## Meet the new boss.
But wait.  Why is this better?  Are the advantages I just listed absent from a normal Java, or Ruby, or .Net system?  

What about: _Independent Deployability_?

We have these things called `jar` files.  Or Gems.  Or DLLs.  Or Shared Libraries.  The reason we have these things is so we can have independent deployability.  

Most people have forgotten this.  Most people think that `jar` files are just convenient little folders that they can jam their classes into any way they see fit.  They forget that a `jar`, or a DLL, or a Gem, or a shared library, is _loaded and linked at runtime_.  Indeed, DLL stands for _Dynamically Linked Library_.  

So if you design your `jar`s well, you can make them just as independently deployable as a MS.  Your team can be responsible for your `jar` file.  Your team can deploy your DLL without massive coordination with other teams.  Your team can test your GEM by writing unit tests and mocking out all the other Gems that it communicates with.  You can write a `jar` in Java or Scala, or Clojure, or JRuby, or any other JVM compatible language.  You can even use your own database and wesbserver if you like. 

If you'd like proof that `jar`s can be independently deployable, just look at the plugins you use for your editor or IDE.  They are deployed entirely independently of their host!  And often these plugins are nothing more than simple `jar` files. 

So what have you gained by taking your `jar` file and putting it behind a socket and communicating with REST?

## Freedom's just another word for...
One thing you lose is time.  It takes _time_ to communicate through a socket.  It takes _time_ to decode REST messages.  And that time means you cannot use micro-services with the impunity of a `jar`.  If I want two `jar`s to get into a rapid chat with each other, I can.  But I don't dare do that with a MS because the communication time will kill me.  

On my laptop it takes 50ms to set up a socket connection, and then about 3us per byte transmitted through that connection.  And that's all in a single process on a single machine.  Imagine the cost when the connection is over the wire!

Another thing you lose (and I hate to say this) is debuggability.  You can't single step across a REST call, but you can single step across `jar` files.  You can't follow a stack trace across a REST call.  Exceptions get lost across a REST interface.  

## Backpedal
After reading this you might think I'm totally against the whole notion of Micro-Services.  But, of course, I'm not.  I've built applications that way in the past, and I'll likely build them that way in the future.  It's just that I don't want to see a big fad tearing through the industry leaving lots of broken systems in it's wake.  

For most systems the independent deployability of `jar` files (or DLLS, or Gems, or Shared Libraries) is more than adequate.  For most systems the cost of communicating over sockets using REST is quite restrictive; and will force uncomfortable trade-offs.  

My advice:  
>_Don't leap into microservices just because it sounds cool.  Segregate the system into `jar`s using a plugin architecture first.  If that's not sufficient, then consider introducing service boundaries at strategic points._
