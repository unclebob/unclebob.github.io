---
layout: post
title: "'Interface' Considered Harmful"
tags: ["Craftsmanship"]
---
What do you think of `interface`s?

>_You mean a `Java` or `C#` interface?_

Yes, are `interface`s a good language feature?

>_Of course, they're great!_

Really.  Hmmm.  What is an interface?  Is it a class?

>_No, it's different from a class._

In what way?

>_None of it's methods are implemented._

Then is this an interface?

    public abstract class MyInterface {
      public abstract void f();
    }

>_No, that's an abstract class._

What is the difference?

>_Well, an abstract class can have functions that are implemented._

Yes, but this one doesn't.  So why isn't it an interface?

>_Well, an abstract class can have non-static variables, and an interface can't._

Yes, but this one doesn't.  So, again, why isn't it an interface?

>_Because it's not._

That's not a very satisfying answer.  How does it differ from an interface?  What can you do with an interface that you cannot do with that class?

>_A class that `extend`s another, cannot also `implement` your class._

Why not?

>_Because, in Java, you cannot `extend` multiple classes._

Why not?

>_Because the compiler won't allow you to._

That's odd.  Well, then, why can't I `implement` that class rather than `extend` it?

>_Because the compiler will only allow you to `implement` an `interface`._

My that's a strange rule.  

>_No, it's perfectly reasonable.  The compiler will allow you to `implement` many interfaces but only allow you to `extend` one class._

Why do you suppose the Java compiler will allow you to `implement` multiple interfaces, but won't allow you to `extend` multiple classes?

>_Because multiple inheritance of classes is dangerous._

Really?  How so?

>_Because of the "Deadly Diamond of Death"!_

My goodness, that sounds scary.  Just what is the Deadly Diamond of Death?

>_That's when a class extends two other classes, both of which extend yet another class._

You mean like this:

	class B {}
	class D1 extends B {}	
	class D2 extends B {}	
	class M extends D1, D2 {}
	
>_Yes!  That's bad!_

Why is that bad?

>_Because class B might have an instance variable!_	

You mean like this?

	class B {private int i;}

>_Yes!  And then how many `i` variables would be in an instance of `M`?_

Ah, I see.  Since both `D1` and `D2` have an `i` variable, and since `M` derives from both `D1` and `D2`, then you might expect `M` to have two separate `i` variables.  

>_Yes!  But since `M` derives from `B` which has only one `i` variable, you might expect `M` to have just one `i` variable too._

Ah, so it's ambiguous.

>_Yes!_

So Java (and therefore C#) cannot `extend` multiple classes because someone _might_ create a Deadly Diamond of Death?

>_No, because everyone _would_ create a Deadly Diamond of Death since all objects implicitly derive from `Object`._

Ah!  I see.  And the compiler writers couldn't make `Object` a special case?

>_Uh... Well, they didn't._

Hmmm.  I wonder why?  Have other compiler writers solved this problem?

>_Well, C++ allows you to create diamonds._

Yes, and I think Eiffel does to.

>_And, gosh, I think Ruby figured out a way to do it._

Yes, and so did CLOS and -- well, let's just say that the deadly diamond of death is a problem that was solved decades ago and it isn't deadly, and does not lead to death.

>_Hmmm.  Yeah, I guess that's true._

So then back to my original question.  Why isn't this an interface?

	public abstract class MyInterface {
  	  public abstract void f();
	}

>_Because it uses the keyword class; and the language won't allow you to multiply inherit classes._

That's right.  And so the keyword `interface` was invented as a way to prevent multiple inheritance of classes.

>_Yeah, that's probably true._

So why didn't the authors of Java (and by extension C#) use one of the known solutions to implement multiple inheritance?

>_I don't know._

I don't know either, but I can guess.  

>_What's your guess?_

Laziness.

>_Laziness?_

Yeah, they didn't want to deal with the issue.  So they created a new feature that allowed them to sidestep it.  That feature was the `interface`.  

>_You are suggesting that the `interface` feature of Java was a hack that the authors used in order to avoid some work?_

I can't explain it any other way.

>_Well I think that's kind of rude.  I'm sure their intentions were better than that.  And anyway it's kind of nice to have `interface`s isn't it?  I mean, what harm do they do?_

Ask yourself this question:  Why should a class have to _know_ that it is `implement`ing an interface?  Isn't that precisely the kind of thing you are supposed to hide?

>_You mean a derivative has to know in order to use the right keyword, `extends` or `implements`, right?_

Right!  And if you change a class to an interface, how many derivatives have to be modified?

>_All of them.  At least in `Java`.  They solved that problem in `C#`._

Indeed they did.  The `implements` and `extends` keywords are redundant and damaging.  Java would have been better off using the colon solution of `C#` and `C++`.

>_OK, OK, but when do you really need multiple inheritance?_

So, here is what I would like to do:

	public class Subject {
		private List<Observer> observers = new ArrayList<>();
		private void register(Observer o) {
			observers.add(o);
		}
		private void notify() {
			for (Observer o : observers)
			    o.update();
		}
	}
	
	public class MyWidget {...}
	
	public class MyObservableWidget extends MyWidget, Subject {
		...
	}

>_Ah, that's the Observer pattern!_

Yes.  That's the _Observer_ pattern -- done _correctly_.

>_But it won't compile because you can't extend more than one class._

Yes, and that's a tragedy.

>_A tragedy?  But why?  I mean you could just derive `MyWidget` from `Subject`!_

But I don't want `MyWidget` to know anything about being observed.  I want to maintain the separation of concerns.  The concern of being observed is separate from the concern of widgets.

>_Well then just implement the `register` and `notify` functions in `MyObservableWidget`_

What?  And duplicate that code for every observed class?  I don't think so!

>_Well then have `MyObservableWidget` hold a reference to `Subject` and delegate to it?_

What?  And duplicate the delegation code in every one of my observers?  How crass.  How degenerate.  Ugh.

>_Well, you're going to have to do one or the other of those things._

I know.  And I hate it.  

>_Yeah, it seems that there's no escape.  Either you'll have to violate the separation of concerns, or you'll have to duplicate code.  

Yes.  And it's the language forcing me into that situation.  

>_Yes, that's unfortunate._

And what feature of the language is forcing me into this bad situation?

>_The `interface` keyword._

And so...?

>_The `interface` keyword is harmful._



   