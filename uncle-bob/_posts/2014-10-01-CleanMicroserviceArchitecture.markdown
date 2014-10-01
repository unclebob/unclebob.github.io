---
layout: post
title: "Clean Micro-service Architecture"
tags: ["Programming"]
---
How do you scale a software system?  One thing should be obvious: at some point you need to have more than one computer.  There was a day, and it wasn't so long ago, that scaling a system could be achieved by waiting.  You simply waited for computers to get faster and more powerful. Every few months you automatically got an increase in scale.  

Whether that was a good strategy or not; it doesn't work anymore.  When the millennium turned, hardware designers stopped trying to increase clock rates and started to proliferate cores instead.  Indeed, in order to achieve that proliferation those hardware designers have been removing the caches and pipelines that used to enhance the speed of single core machines.

So today, scaling a software system means adding more cores, and adding more servers.  There's no way around that.  So how do you do it?  How do you split your application up so that it can be run on multiple cores and multiple servers?

### How do you scale?

Your graphics card uses one approach.  It has many processors that operate in lockstep; performing the same operations on different areas of internal memory.  This form of massive parallelism is ideal for a graphics card since high speed graphics are achieved by performing the same transformations over large arrays of similar data.  Indeed, supercomputers have used this approach for decades to predict weather, and simulate nuclear explosions.  

Another technique is the traditional three-tiered approach.  You subdivide your system into a GUI, a middleware, and a database.  You allocate some servers to the GUI, a few more to the middleware, and yet a few more to the database.  You compose a suite of messages (typically involving serialized objects) that can be passed between the layers.  Voila!  Scaling.

### Micro-services

Lately we are seeing another kind of scaling strategy.  _Micro-services_.  I've written about them [here](http://blog.cleancoder.com/uncle-bob/2014/09/19/MicroServicesAndJars.html), and [here](http://blog.cleancoder.com/uncle-bob/2014/09/18/TheMoreThingsChange.html).  Martin Fowler and James Lewis have famously written about them [here](http://martinfowler.com/articles/microservices.html).

A micro-service is a small executable running on a server somewhere.  It responds to asynchronous messages.  Typically those messages are delivered over HTTP in REST format; though that's a detail, not a requirement.  

A system has a _micro-service architecture_ when that system is composed of many collaborating micro-services; typically without centralized control.  

### Clean Architecture and Micro-services.

Now consider the so-called [Clean Architecture](http://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html).  Note that it makes use of many components, including Use-cases, Presenters, and Gateways. Those components receive requests in the form of primitive data structures (POJOs) that arrive from a source that is decoupled from the component via a polymorphic input boundary.  Note also that the components respond to these requests by creating new primitive data structures and sending them to an output sink that is decoupled from the component via a polymorphic output boundary.  Could this structure be used to implement a micro-service architecture?

> _Of course._ 

Nothing in the Clean Architecture demands that the messages need to be either synchronous or asynchronous.  Nothing prevents those request and response messages from being transmitted to another server.  Nothing about the architecture prevents the components from being little executables communicating over HTTP via REST.  

So, a micro-service architecture can nicely conform to the Clean Architecture.  Indeed, were I to build a system using micro-services, I would almost certainly follow this route.

### The Component Scalability Scale

A Micro-service is just one way to deploy a software component.  There are others; and they have different scalabilities.  Here is a list of some different deployment options for components, in order of scalability.

 1. Micro-services deployed on lots of servers.  
 2. A smaller number of servers, each running more than one micro-service.
 3. Just one server with a group of micro-services running as simple executables.
 4. Services running as threads in a single virtual machine communicating via message queues
 5. Dynamically linked components (in jars or DLLs) passing data structure messages through function calls. 

Again, it should be obvious that the Clean Architecture works just as well at any level on this scale.  The reason is that the Clean Architecture _does not care_ how the components are deployed.  Indeed a system with a good Clean Architecture _does not know which deployment option it is using._ 

Let me say that again.  The code within the components of a Clean Architecture has no idea whether:

 * it is in a micro-service running on an independent server communicating with other independent servers over the internet, 
 * or in a small executable among many running on a single machine communicating over simple sockets, 
 * or in a lightweight thread communicating with other lightweight threads in the same virtual machine using mailboxes or queues, 
 * or in a simple Jar or DLL communicating with other components using polymorphic function calls.

And that should give you a clue as to what this article is really about.  

### The Deployment Model is a Detail.

If the code of the components can be written so that the communications mechanisms, and process separation mechanisms are irrelevant, _then those mechanisms are details_.  And details are _never_ part of an architecture.

That means that there is no such thing as a micro-service architecture.  Micro-services are a _deployment option_, not an architecture.  And like all options, a good architect keeps them open for as long as possible.  A good architect defers the decision about how the system will be deployed until the last responsible moment.

### Forced Ignorance.

Many folks will likely complain about this viewpoint by suggesting that if you don't design your system for micro-services up front, you won't be able to shim them in after the fact.  

>_That's BDUF Baloney._  

The job of good system architects is to create a structure whereby the components of the system -- whether Use-cases, UI components, database components, or what have you -- have no idea how they are deployed and how they communicate with the other components in the system.  This forced ignorance allows the architects to choose a deployment model that works for the _current_ situation, and to adapt the deployment model as the situation changes.  If the system must massively scale, you deploy it in micro-services.  If the system needs two or three servers only, you deploy it in a combination of processes, threads, and jars.  If you never need more than a single server, you can deploy it in jars alone.  

Breaking that forced ignorance is a good way to over-engineer a system.  Too often I have seen systems that have adopted huge three-tiered architectures in anticipation of scaling, only to discover that they never need to be deployed on more than one server.  How much simpler could that software have been if only they had tried the single server option first, and kept the components ignorant of the deployment model?

### Other Matters.

Of course there are other matters to consider.  Firstly, if you deploy into micro-services, you have the freedom to choose any language you'd like.  You can write your micro-service in Ruby, Clojure, Java, C#, C++, C, assembler, Pascal, Delphi, PHP, Javascript, or even COBOL.  Secondly, you can use whatever framework you like.  One micro-service could use Rails, another could use Spring, still another could use BOOST.  Similarly, each micro-service may be able to use a different database.  One might use Couch, while another used SqlServer and still another used MySql or Datomic.  Finally, there is the intense isolation that a micro-service implies.  A micro-service boundary is the ultimate form of decoupling.

That last point needs amplification.  If two components communicate over HTTP using REST, then they are _strongly decoupled_.  The only thing binding those two components together is the schema of the REST messages; i.e. the interface.  Not only are they decoupled by the interface, they are also decoupled in deployment time.  Those two services do not need to be started at the same time; nor do they need to be shut down at the same time.  It is perfectly possible to reboot a micro-service without affecting those that depend on it.  That's a lot of decoupling.

### Restrictions down the scale.

As you move down the scale from micro-services to processes to threads to jars, you start to lose some of those flexibilities.  The closer you get to jars the less flexibility you have with languages.  You also have less flexibility in terms of frameworks and databases. There is also a greater risk that the interfaces between components will be increasingly coupled.  And, of course, it's hard to reboot components that live in a single executable.  

Or is it?  Actually OSGi has been around in the Java world for some time now.  OSGi allows you to hot-swap jar files.  That's not quite as flexible as bouncing a micro-service, but it's not that far from it.  

As for languages, it's true that within a single virtual machine you'll be restricted.  On the other hand, the JVM would allow you to write in Java, Clojure, Scala, and JRuby, just to name a few.  

So, yes, as you go down the scale the restrictions increase; but perhaps not all that much.

As for frameworks and databases, is it really such a bad thing, especially early in a system's development, to limit their numbers?  Do we really want to start out with one team using JPA and another using Hibernate?  Do we really want one component using Datomic and another using Oracle?  And if we allow that, aren't we creating a lot of configuration complexity?

And, finally, interface coupling is a matter of discipline and good design.  After all, a plain old Java Object (pojo) passed through a polymorphic interface is no more coupled than REST.  A little bit of care in component design is all it takes to make jars whose interfaces are just as loosely coupled as a micro-service.

### TANSTAAFL

As you move up the scale, those restrictions drop away, but new problems start to show up.  In what order to you start up the system?  In what order do you shut it down?  How do you deal with configuration and control of all the services.  What about duplicated code?  How about the versioning of message formats? But rather than me itemizing all the issues here, you can read about some of them  [here](http://eugenedvorkin.com/seven-micro-services-architecture-problems-and-solutions/) and [here](http://highscalability.com/blog/2014/4/8/microservices-not-a-free-lunch.html).  Suffice it to say that the decision to use micro-services is a trade-off not a free lunch.

### Monoliths and Marketeers.

Finally, a word about nomenclature.  Some advocates of micro-services like to classify the alternative as _monolithic_.  This is a pejorative term chosen to imply: "Bad".  The word _monolith_ means "one rock".  The implication is that if you aren't using micro-services, then you must have a big coupled monstrosity.  

>_That's Marketing Baloney._  

A well designed system following the Clean Architecture is about as far from a monolith as you can get.  Rather, it is a set of independently deployable dynamically linked components (jars or DLLs) that are strongly decoupled from each other, can be maintained by different teams, can be written in a multitude of different languages, and can be hot-swapped using something like OSGi.  Hardly monolithic.

### Conclusion and Recommendation[1]

From all of this you might be getting the idea that I think micro-services are a bad idea; and that you should not use them.  This is not the case.  Micro-services are a perfectly viable deployment model that you should strive to be compatible with.  If you can't deploy into micro-services, it means you've coupled your architecture to a particular deployment model.  

By the same token if you can _only_ deploy your system with micro-services, then you have coupled your architecture to _that_ particular deployment model; and that's just as bad.

What I am trying to convince you to do is to _ignore_ any particular deployment model.  Treat the deployment model as a detail, and leave the options open.  Build your system so that you can deploy it into jars, or into micro-services, or anywhere in between. 

Begin by deploying your system into dynamically linked components (Jars or DLLs), and gradually walk up the scale as the need arises.  Don't leap to the top of the scale in anticipation of massive scaling.  Keep that option open by conforming to the Clean Architecture. 

----

[1] Who am I to make this recommendation?  After all, as I said in a previous article, I just encountered the term "Micro-services" a few weeks ago.  

I may have just discovered the _term_; but in the last 40 years of my career I have had ample opportunity to design and build systems that deployed components as independent executables communicating through messages.  Micro-services might be a new term; but it's hardly a new idea.

