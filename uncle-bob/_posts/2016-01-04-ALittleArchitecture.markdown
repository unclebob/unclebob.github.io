---
layout: post
title: A Little Architecture
tags: ["Craftsmanship"]
---
I want to become a Software Architect.

> _That's a fine goal for a young software developer._

I want to lead a team and make all the important decisions about databases and frameworks and web-servers and all that stuff.

> _Oh.  Well, then you don't want to become a Software Architect after all._

Of course I do!  I want to be the one who makes all the important decisions.

>_That's fine, but you didn't list the important decisions.  You listed the irrelevant ones._

What do you mean?  The Database isn't an important decision?  Do you know how much money we spend on them?

>_Too much probably.  And, no; the database is not one of the most important decisions._

How can you say that?  The database is the heart of the system!  It's where all the data is organized, sorted, indexed, and accessed.  Without it there would be no system!

>_The database is merely an IO device.  It happens to provide some useful tools for sorting, querying, and reporting but those are ancillary to the system architecture._

Ancillary?  That's crazy.

>_Yes, ancillary.  The business rules of your system may be able to make use of some of those tools; but those tools aren't intrinsic to those business rules.  If you had to, you could replace those tools with different tools; but your business rules would still be the same._

Well, yeah, but I'd have to recode them all since they all used the tools in the original database.

>_Well, there's your problem._

What do you mean?

>_Your problem is that you believe the business rules depend upon the tools of the database.  They don't.  Or at least they shouldn't if you've provided a good architecture._

That's crazy talk.  How can I create business rules that don't use the tools that they have to use.

>_I didn't say they don't use the tools of the database; I said they shouldn't depend upon them.  The business rules should not know what specific database you are using._

How do you get the business rules to use tools without knowing about them?

>_You invert the dependency.  You have the database depend upon the business rules.  You make sure the business rules don't depend on the database._

You are talking gibberish.

>_On the contrary, I am speaking the language of Software Architecture.  This is the Dependency Inversion Principle.  Low level policies should depend upon high level policies._

More gibberish!  The high level policies (I presume you mean the business rules) call down to the low level policies (I presume you mean the database).  So the high level policies depend upon the low level policies in the same way that callers depend upon callees.  Everybody knows this!

>_At runtime this is true.  But at compile time we want the dependencies inverted.  The source code of the high level policies should not mention the source code of the lower level policies._  

Oh come on!  You can't call something without mentioning it.

>_Of course you can.  That's what Object Orientation is all about._

Object Orientation is about creating models of the real world, it's about combining data and function into cohesive objects.  It's about organizing code into an intuitive structure.  

>_Is that what they told you?_

Everybody knows it.  It's obviously true.

>_No doubt.  No doubt.  And yet, using the principles of object-orientation you can indeed call something without mentioning it._

OK.  How?

>_You know that in an object-oriented design objects send messages to each other?_

Yes.  Of course.  

>_And you know that the sender of the message does not know the type of the receiver._

That depends on the language.  In Java the sender knows at least the base type of the receiver.  In Ruby the sender at least knows that the receiver can handle the message being sent.

>_True.  But in either case the sender does not know the exact type of the receiver._

Yeah.  OK.  Sure.

>_Therefore the sender can cause a function to execute in the receiver, without mentioning the exact type of the receiver._

Yeah.  Right.  I get that.  But the sender still depends upon the receiver.

>_At runtime, yes.  But not at compile time.  The source code of the sender does not mention, or depend upon, the source code of the receiver.  In fact the source code of the receiver depends upon the source code of the sender._

Nahh!  The sender still depends on the class it's sending to.  

>_Perhaps some source code would make this clearer.  I'll write this in Java.  First the package `sender`:_

	package sender;

	public class Sender {
	  private Receiver receiver;

	  public Sender(Receiver r) {
	    receiver = r;
	  }

	  public void doSomething() {
	    receiver.receiveThis();
	  }

	  public interface Receiver {
	    void receiveThis();
	  }
	}
	
>_Next the `receiver` package._

	package receiver;

	import sender.Sender;

	public class SpecificReceiver implements Sender.Receiver {
	  public void receiveThis() {
	    //do something interesting.
	  }
	}

>_Notice that the `receiver` package depends upon the `sender` package.  Note also that the `SpecificReceiver` depends upon the `Sender`.  Notice also that nothing in the `sender` package knows anything at all about the `receiver` package._

Yeah, but you cheated.  You put the receiver's interface in the sender's class.

>_You are beginning to understand, grasshopper._

Understand what?

>_The principles of architecture, of course.  Senders own the interfaces that the receivers must implement._

Well if that means I have to use nested-classes then...

>_Nested classes are just one means to achieve an end.  There are others._

OK, now wait.  What does all this have to do with databases.  That's how we started this conversation.  

>_Let's look at some more code.  First a simple business rule:_

	package businessRules;

	import entities.Something;

	public class BusinessRule {
	  private BusinessRuleGateway gateway;

	  public BusinessRule(BusinessRuleGateway gateway) {
	    this.gateway = gateway;
	  }

	  public void execute(String id) {
	    gateway.startTransaction();
	    Something thing = gateway.getSomething(id);
	    thing.makeChanges();
	    gateway.saveSomething(thing);
	    gateway.endTransaction();
	  }
	}
	
That business rule doesn't do much.  

>_It's just an example.  You'd likely have many classes like this, implementing lots of different business rules._

OK, so what's that `Gateway` thingy?

>_It supplies all the data access methods used by the business rule.  It's implemented as follows:_

	package businessRules;

	import entities.Something;

	public interface BusinessRuleGateway {
	  Something getSomething(String id);
	  void startTransaction();
	  void saveSomething(Something thing);
	  void endTransaction();
	}
	
>_Notice that it's in the `businessRules` package._

Yeah OK.  And what's that `Something` class?

>_That represents a simple business object.  I put it in a package named `entities`._
	
	package entities;

	public class Something {
	  public void makeChanges() {
	    //...
	  }
	}
	
>_And then finally there is the implementation of the `BusinessRuleGateway`.  This is the class that knows about the actual database:_

	package database;

	import businessRules.BusinessRuleGateway;
	import entities.Something;

	public class MySqlBusinessRuleGateway implements BusinessRuleGateway {
	  public Something getSomething(String id) {
	    // use MySql to get a thing.
	  }

	  public void startTransaction() {
	    // start MySql transaction
	  }

	  public void saveSomething(Something thing) {
	    // save thing in MySql
	  }

	  public void endTransaction() {
	    // end MySql transaction
	  }
	}
	
>_Again, notice that the business rules call the database at run time; but at compile time it is the `database` package that mentions and depends upon the `businessRules` package._

OK, ok, I think I get it.  You're just using polymorphism to hide the database implementation from the business rules.  But you still have to have an interface that provides all the database tools to the business rules.

>_No, not at all.  We don't try to provide all the database tools to the business rules.  Rather, we have the business rules create interfaces for only what they need.  The implementation of those interfaces can call the appropriate tools._

Yeah, but if all the business rules need all the tools then you just have to put all the tools in that `gateway` interface.

>_Ah.  I see that you still do not understand._

Understand what?  It seems perfectly clear to me.

>_Each business rule defines an interface for just the data access facility that it needs._

Wait.  What?

>_This is called the Interface Segregation Principle.  Each business rule class will only use some of the facilities of the database.  And so each business rule provides an interface that gives it access to just those facilities._

But that means that you're going to have lots of interfaces, and lots of little implementation classes that call other database classes.

>_Ah, good.  I see you are beginning to understand._

But that's a mess, and a waste of time!  Why would I do that?

>_You would do that in order to be clean, and save time._

Oh come on.  That's just a lot of code for code's sake.

>_On the contrary, these are the important architectural decisions that allow you to defer the irrelevant decisions._

What do you mean by that?

>_Remember that you started by saying that you wanted to be a Software Architect?  You wanted to make all the really important decisions?_

Yes, that's what I want.

>_Among those decisions that you wanted to make were the database, and the webserver, and the frameworks._

Yeah, and you said those weren't the important decisions.  You said they were irrelevant.

>_That's right.  They are.  The important decisions that a Software Architect makes are the ones that allow you to **NOT** make the decisions about the database, and the webserver, and the frameworks._

But you have to make those decisions first!

>_No, you don't.  Indeed, you want to be allowed to make them much later in the development cycle -- when you have more information._

<p/>  

>_Woe is the architect who prematurely decides on a database, and then finds that flat files would have been sufficient._  

<p/>

>_Woe is the architect who prematurely decides upon a web-server, only to find that all the team really needed was a simple socket interface._  

<p/>

>_Woe is the team whose architects prematurely impose a framework upon them, only to find that the framework provides powers they don't need and adds constraints they can't live with._

<p/>

>_Blessed is the team whose architects have provided the means by which all these decisions can be deferred until there is enough information to make them._  

<p/>

>_Blessed is the team whose architects have so isolated them from slow and resource hungry IO devices and frameworks that they can create fast and lightweight test environments._  

<p/>

>_Blessed is the team whose architects care about what really matters, and defer those things that don't._

Nonsense.  I don't get you at all.  

>_Well, perhaps you will in a decade or so...  If you haven't gone into management by then._
