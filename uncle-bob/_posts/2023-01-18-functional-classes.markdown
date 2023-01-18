---
layout: post
title: Functional Classes
tags: ["Software"]
---
I recently tweeted the following:

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr">Should you subdivide a functional program into classes the way you would an object oriented program?<br><br>Yes. You should. Because the rules don’t change just because you’ve chosen to use immutable data structures.</p>&mdash; Uncle Bob Martin (@unclebobmartin) <a href="https://twitter.com/unclebobmartin/status/1615436628385824769?ref_src=twsrc%5Etfw">January 17, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This led to a bevy of interesting responses about the difference between classes and modules.  In answer to those responses I tweeted this:

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr">A class is a group of cohesive and narrowly defined functions that operate on an encapsulated data structure. The functions may, or may not, be polymorphically deployed.</p>&mdash; Uncle Bob Martin (@unclebobmartin) <a href="https://twitter.com/unclebobmartin/status/1615438162746134528?ref_src=twsrc%5Etfw">January 17, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Of course that only led to an increased number of interesting responses.  And so I thought that it might be wise to blog about my reasoning rather than to continue trying to cram that reasoning into tweets.

If you are in doubt about what FP is, and about what OO is, and about whether the two are compatible, then I recommend [this](http://blog.cleancoder.com/uncle-bob/2018/04/13/FPvsOO.html) old blog of mine.

What is a class? According to the dictionary a class is:

>_A set, collection, group, or configuration containing members regarded as having certain attributes or traits in common; a kind or category._

Now consider that definition when reading the next paragraph.

In OO languages we organize our programs into classes of objects that share similar traits.  We describe those objects in terms of the attributes and behaviors that they have in common.  We strive to create hierarchies of classification that those objects can fit within.  We consider the higher level classifications to be abstractions that allow the expression of general truths that are independent of irrelevant details.  (Indeed, I once defined abstraction as: _The Amplification of the essential, and the elimination of the irrelevant._[[1]](https://www.amazon.com/Designing-Object-Oriented-Applications-Method/dp/0132038374))

In 1966 the power of abstraction by classification led the authors of Simula to create the keyword `class`.  In 1980, Bjarne Stroustrup continued that convention and used the `class` keyword in C++.  This was actually somewhat strange because C already had the keyword `struct` which had a virtually identical meaning.  But the power of the word `class` held sway.

In the mid 90s the power of that word led the authors of Java (and then C#) to declare _and enforce_ that _everything_ in a program must be part of a class.  This was a dramatic overreach.  It seems to me that some of the things that Java forces into classes ought not to be in classes at all.  For example, the class `java.lang.Math` is really just a namespace for a batch of functions and is not, in any sense, a classification of objects.

This conflation of object classification and namespaces is confusing and unnecessary; and is probably part of the reason my initial tweet generated the responses that it did.

Another overreach in Java (and by extension C#) is that methods are polymorphic by default.  Polymorphism is a tool, not a rule.  Many, if not most, function calls do not require dynamic dispatch.  

These kinds of overreach lead to confusion about what a class really is.  I believe that most of the responses to my tweet were the result of that confusion.

So let's cut to the chase.  

One of the oldest rules of software design is that we should partition the elements of the system into loosely coupled and internally cohesive elements.  Those elements become well named places where we can put data and behavior.  This follows the old proverb: _A place for everything, and everything in its place._  

What are those elements?  It seems obvious that the classification structures of objects ought to be high on the list.  Namespaced function libraries like `java.lang.Math` are another obvious choice.  In the one case we have a batch of functions that manipulate an internal data structure.  In the other case we have a batch of functions that manipulate an external data structure.  

The essential charachteristic of these elements, these batches of functions, is that they are internally cohesive.  That means that all the functions in the batch are strongly related to each other because they manipulate the same data structures, whether internal or external.  It is that cohesion that drives the partitioning of a software design.

###Example

Recently I have been writing an application called [`more-speech`](http://github.com/unclebob/more-speech) which is a client that browses messages on the [`nostr`](https://nostr.com) network.  This nework is composed of relays that use a simple websocket protocol to transmit messages to clients.  The `more-speech` client is written in Clojure, which is a Functional Programming language.  

Early on I created a module named `protocol` to house the code that implemented the `nostr` protocol.  I began this module by managing the websockets over which the messages travelled, and then decoding those messages and manipulating them according to the rules of the protocol.  

Clojure is not a traditional OOPL, there is no `class` keyword that is used to declare and define objects and the methods that manipulate them.  Rather, a module in Clojure is just a batch of functions that are not syntactically bound to any particular data.  Thus my `protocol` module had functions that dealt with `WebSocket`s and functions that dealth with messages and functions that dealth with protocol elements.  They were cohesive in the sense that they were all related to the `nostr` protocol; but there was no central data structure that unified them.

The other day I realized that I was missing an abstraction.  The `nostr` protocol may be transmitted over websockets but the protocol rules have nothing to do with websockets.  Those rules deal with the data that comes through the websockets, but not the websockets themselves.  Yet my `protocol` module was littered with websocket code.

So I separated the websocket code from the `protocol` code by creating an abstraction that I called `relay`.  A `relay` is a data structure that contains the `url` of a websocket, the websocket itself, and a function to call when messages are received.  The `relay` data structure is manipulated by functions such as `make`, `open`, `close`, and `send`.   

This `relay` module very clearly defines a class of objects.  The `protocol` constructs a `relay` object for each of the urls in a list of active relays.  It `open`s those `relay`s and `send`s messages to them.  Messages that are received are sent to `protocol` through the callback functions that are passed into the function that constructs the `relay` object.  In order to maintain the immutability and referential transparency constraints of Functional Programming, the functions that update the state of a `relay` return a new instance of that `relay`.

###Lesson

Java, C#, Ruby, and C++ all either enforce, or stronly encourage, the partitioning of systems into classes. Clojure does not; it is entirely agnostic about classes. The lesson that I learned from `protocol` and `relay` is that I had not been paying enough attention to class structure when writing complex Clojure programs.  Instead, I had been allowing functions to accumulate in modules in a, more or less, ad hoc fashion -- similar to the way one might program in C, Fortran, Basic, or even Assembler.  But that was lazy.  Objects exist in programs, and they can, and should, be classified.  So, from now on, I will be paying much more attention to the classification structure of the objects my systems.  

>_A place for everything, and everything in its place._





