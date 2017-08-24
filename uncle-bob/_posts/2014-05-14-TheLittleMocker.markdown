---
layout: post
title: "The Little Mocker"
tags: ["Testing"]
---
The following is a conversation around mocking:

What is this?:

    interface Authorizer {
	  public Boolean authorize(String username, String password);
    }
>_An interface._

So what then, is this?

    public class DummyAuthorizer implements Authorizer {
	  public Boolean authorize(String username, String password) {
		return null;
  	  }
    }
>_That's a Dummy._

And what do you do with a Dummy?
>_You pass it into something when you don't care how it's used._

Such as?
>_As part of a test, when you must pass an argument, but you know the argument will never be used._

Can you show an example?
>_Sure._

	  public class System {
		public System(Authorizer authorizer) {
			this.authorizer = authorizer;
		}

		public int loginCount() {
			//returns number of logged in users.
		}
	  }

	  @Test
	  public void newlyCreatedSystem_hasNoLoggedInUsers() {
		System system = new System(new DummyAuthorizer());
		assertThat(system.loginCount(), is(0));
	  }

I see.  In order to construct the `System` an `Authorizer` must be passed to the constructor; but the `authorize` method of that `Authorizer` will never be called since, in this test, no one will log in.
>_You got it._

And so the fact that the `authorize` method of the `DummyAuthorizer` returns a `null` is not an error.
>_Indeed not.  In fact, it's the best thing a Dummy can return._

Why is that?
>_Because if anybody tried to use that Dummy, they'd get a `NullPointerException`._

Ah, and you don't want the Dummy to be used.
>_Right!  It's a dummy._

But isn't this a mock?  I thought these test objects were called mocks.
>_They are; but that's slang._

Slang?
>_Yes, the word "mock" is sometimes used in an informal way to refer to the whole family of objects that are used in tests._

Is there a formal name for these test objects?
>_Yes, they are called "Test Doubles"[1]._

You mean like "Stunt Doubles" in the movies?
>_Exactly._

So then the word "mock" is just colloquial slang?
>_No, it has a formal meaning too; but when we are speaking informally the word mock is a synonym for Test Double._

Why do we have two words?  Why don't we just use Test Double instead of Mock?
>_History._

History?
>_Yes, long ago some very smart people wrote a [paper](http://www.ccs.neu.edu/research/demeter/related-work/extreme-programming/MockObjectsFinal.PDF) that introduced and defined the term Mock Object.  Lots of other people read it and started using that term.  Other people, who hadn't read the paper, heard the term and started using it with a broader meaning.  They even turned the word into a *verb*.  They'd say, "Let's mock that object out.", or "We've got a lot of mocking to do."_

That kind of thing happens a lot with words, doesn't it?
>_Yes it does.  Especially when a word has just one syllable, and is easy to say._

Yeah, I guess it's easier to say: "Let's mock that." instead of: "Let's make a test double for that."

>_Right.  Colloquialisms are a fact of life._

OK, but when we need to speak precisely...
>_You should use the formal language. Yes._

So then what is a Mock?
>_Before we get to that, we should look at other kinds of Test Doubles._

Like what?
>_Let's look at Stubs._

What's a stub?
>_This, is a stub:_

    public class AcceptingAuthorizerStub implements Authorizer {
	  public Boolean authorize(String username, String password) {
		return true;
  	  }
    }

It returns `true`.
>_That's right._

Why?
>_Well,  suppose you want to test a part of your system that requires you to be logged in._

I'd just log in.
>_But you already know that login works,  You've tested it a different way.  Why test it again?_

Because it's easy?
>_But it takes time.  And it requires setup.  And if there's a bug in login, your test will break.  And, after all, it's an unnecessary coupling._

Hmmm.  Well, for the sake of argument, let's say I agree.  What then?
>_You simply inject the `AcceptingAuthorizerStub` into your system for that test._

And it will authorize the user without question.
>_Right._

And if I want to test the part of the system that handles unauthorized users, I could use a stub that returns `false`.
>_Right again._

OK, so what else is there?
>There's this:

    public class AcceptingAuthorizerSpy implements Authorizer {
	  public boolean authorizeWasCalled = false;

	  public Boolean authorize(String username, String password) {
		authorizeWasCalled = true;
		return true;
	  }
    }

I suppose that's called a Spy.
>_That's right._

So why would I use it?
>_You'd use this when you wanted to be sure that the `authorize` method was called by your system._

Ah, I see.  In my test I'd inject it like a stub, but then at the end of my test I'd check the `authorizerWasCalled` variable to make sure my system actually called `authorize`.
>_Absolutely._

So a Spy, spies on the caller.  I suppose it could record all kinds of things.
>_Indeed it could.  For example, it could count the number of invocations._

Yeah, or it could keep a list of the arguments passed in each time.
>_Yes.  You can use Spies to see inside the workings of the algorithms you are testing._

That sounds like coupling.
>_It is!  You have to be careful.  The more you spy, the tighter you couple your tests to the implementation of your system.  And that leads to fragile tests._

What's a fragile test?
>_A test that breaks for reasons that shouldn't break a test._

Well if you change the code in the system, some tests are going to break.
>_Yes, but well designed tests minimize that breakage.  Spies can work against that._

OK, I get that.  What other kinds of test doubles are there?
>_Two more.  Here's the first:_

	public class AcceptingAuthorizerVerificationMock implements Authorizer {
	  public boolean authorizeWasCalled = false;

	  public Boolean authorize(String username, String password) {
		authorizeWasCalled = true;
		return true;
	  }

	  public boolean verify() {
		return authorizedWasCalled;
	  }
	}

And, of course, this is a mock.
>_A **True Mock**. Yes._

True?
>_Yes, this is a formal mock object according to the original meaning of the word._

I see.  And it looks like you moved the assertion from the test, into the `verify` method of the, uh, _true_ mock.
>_Right.  Mocks know what they are testing._

So that's it?  You just put the assertion into the mock?
>_Not quite.  Yes, the assertion goes into the mock.  However, what the mock is testing is **behavior**._

Behavior?
>_Yes.  The mock is not so interested in the return values of functions.  It's more interested in what function were called, with what arguments, when, and how often._

So a mock is always a spy?
>_Yes.  A mock spies on the behavior of the module being tested.  And the mock knows what behavior to expect._

Hmmm.  Moving the expectation into the mock feels like a coupling.
>_It is._

So why do it?
>_It makes it a lot easier to write a mocking tool._

A mocking tool?
>_Yes, like JMock, or EasyMock, or Mockito.  These tools let you build mock objects on the fly._

That sounds complicated.
>_It's not.  [here](http://martinfowler.com/articles/mocksArentStubs.html) is a famous paper by Martin Fowler that expains it well._

And there's a book too, isn't there?
>_Yes.  [Growing Object Oriented Software, Guided by Tests](http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627) is a great book about a popular design philosophy driven by mocks._

OK, so then are we done?  You said there was still another kind of test double.
>_Yes, one more.  Fakes._

	  public class AcceptingAuthorizerFake implements Authorizer {
		  public Boolean authorize(String username, String password) {
			return username.equals("Bob");
		  }
	  }

OK, that's strange.  Everybody named "Bob" will be authorized.
>_Right.  a Fake has business behavior.  You can drive a fake to behave in different ways by giving it different data._

It's kind of like a simulator.
>_Yes, simulators are fakes._

Fakes aren't stubs are they?
>_No, fakes have real business behavior; stubs do not.  Indeed, none of the other test doubles we've talked about have real business behavior._

So fakes are different at a fundamental level.
>_Indeed they are.  We can say that a Mock is a kind of spy, a spy is a kind of stub, and a stub is a kind of dummy.  But a fake isn't a kind of any of them.  It's a completely different kind of test double._

I imagine Fakes could get complicated.
>_They can get **extremely** complicated.  So complicated they need unit tests of their own.  At the extremes the fake becomes the real system._

Hmmm.
>_Yes, Hmmm.  I don't often write fakes.  Indeed, I haven't written one for over thirty years._

Wow!  So what do you write?  Do you use all these other test doubles?
>_Mostly I use stubs and spies.  And I write my own, I don't often use mocking tools._

Do you use Dummies?
>_Yes, but rarely._

What about mocks?
>_Only when I use a mocking tool._

But you said you don't use mocking tools.
>_That's right, I usually don't._

Why not?
>_Because stubs and spies are very easy to write.  My IDE makes it trivial.  I just point at the interface and tell the IDE to implement it.  Voila!  It gives me a dummy.  Then I just make a simple modification and turn it into a stub or a spy.  So I seldom need the mocking tool._

So it's just a matter of convenience?
>_Yes, and the fact that I don't like the strange syntax of mocking tools, and the complications they add to my setups.  I find writing my own test doubles to be simpler in most cases._

OK, well, thank you for the conversation.
>_Any time._

-----
[1] [xUnit Test Patterns](http://www.amazon.com/xUnit-Test-Patterns-Refactoring-Code/dp/0131495054)


