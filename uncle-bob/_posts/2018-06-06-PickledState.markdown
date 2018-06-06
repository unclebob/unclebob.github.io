---
layout: post
title: Pickled State
tags: ["Software"]
---

By now everyone is familiar with BDD (Behavior Driven Development) and its emblematic adjective/adverb/adverb triplet: `GIVEN/WHEN/THEN`.  There is something satisfying about using these three words to describe a requirement.  

    GIVEN That the system is waiting for login.
    WHEN the user provides a valid username and password.
    THEN the user is presented with the welcome page.
 
The impact that this triplet has had on our industry is significant.  It began as a way to describe high level tests, and has become a general means for specifying requirements.  It has even acquired a name: _Gherkin_.  

Nowadays business analysts and programmers are encouraged to write suites of Gherkin scenarios to describe the systems they want to build.  An entire consulting industry has grown up around this idea.  Behavior Driven Development, which used to be a testing dialect, has become a management consulting strategy.

> **Spock:** _Fascinating._

Why has this proven to be so effective?  One reason, that has recently occurred to me, is the connection between Gherkin and Finite State Machines.  If you look closely at a Gherkin triplet you will see that it specifies a state transition.

    GIVEN that we are in state S1
    WHEN we recieve event E1
    THEN we transition to state S2

>_This means that a suite of Gherkin scenarios is a specification of a finite state machine._  

Let that last sentence sink in for a second or two.  The formalism that business analysts find most conducive to specifying system _requirements_ just happens to be the same formalism that programmers frequently use to specify system _behavior_.  What's more, that formalism started as, and continues to be, a popular language for specifying _tests_.  

If the Gherkin requirements are complete, then they describe the complete state machine of the system, _and_ the complete test suite for the system.  

> **Spock:** _Logical._ 

Now hold that thought and let's consider _unit tests_.[1]

Well written unit tests always follow the _AAA_ pattern:  `Arrange/Act/Assert`.

First the test _arranges_ the system so that it is in the appropriate state for the test.  Next the test executes the _action_ to be tested.  Finally the test _asserts_ that the state of the system has been appropriately changed by the action. 

I used the word _state_ in that last paragraph intentionally.  It should be clear to you that the _AAA_ pattern is also a description of a transition in a state machine.  Indeed, _AAA_ and _GWT_ are completely isomorphic:

    GIVEN that we have ARRANGED the system for the test.
    WHEN we perform the tested ACTION.
    THEN we can ASSERT the conditions that pass the test.

This, of course, means that the suite of unit tests produced through TDD specifies the finite state machine that describes the system behavior.  If the three laws of TDD are followed, then test coverage will be at (or very near) 100%, and the described finite state machine will be complete.

Now, let's say we have a complete suite of Gherkin scenarios that specify all the acceptance and integration[2] tests for the system.  Let's also say that we have a complete suite of unit tests produced by the three laws of TDD.  How do the state machines specified by these two test suites differ?

At the level of the business they shouldn't differ at all.  All the states and transitions specified in the Gherkin scenarios ought to be present in the unit tests.  However, there will be states and transitions in the unit tests that are not present in the Gherkin.  These will be all the little programming details that the business has no visibility of.  For example, the state transitions that deal with the fact that lines of text sometimes end in `\n\r` but sometimes only end in `\n`.  

Thus the Gherkin is a proper _subset_ of the unit tests.

> **Spock:** _Indeed._ 

Now, let's say that the system is written in F# or Clojure -- i.e. a _functional_ language.  The combination of the Gherkin, unit tests, and code is a living example of the Church-Turing thesis -- that state machines are equivalent to predicate calculus.

I think that's cool. 

> **Spock:** _Way cool._ 

Anyway, back to the Gherkin.  Given all the different means by which people have specified requirements in the past; what is it about Gherkin that is so much more conducive to specification?  

Here's my theory.  Gherkin is a language that sits perfectly between, and speaks equally to, the three disciplines of specification, testing, and programming.  Each of those disciplines gets precisely what they need from the language.  It is the common language that bridges that three-way divide and allows the practitioners to unambiguously communicate.

On top of that, there is no better mechanism for exploring the behavior of a system than a well specified state machine. 

Perhaps that last statement needs some justification.  So consider a typical state machine.  It is composed of a list of transitions; each of which is a triplet.

    CURRENT_STATE : EVENT : NEXT_STATE

The transition reads as follows:  

    GIVEN that we are in the CURRENT_STATE, 
    WHEN we get the EVENT, 
    THEN we go to the NEXT_STATE.  

This means that the criterion for to transitioning to `NEXT_STATE` is the pair: `{CURRENT_STATE, EVENT}`.  

So consider this simple state machine that represents a subway turnstile.  

    LOCKED     COIN  UNLOCKED
    UNLOCKED   PASS  LOCKED

We read these two transitions as follows:

    GIVEN we are in the LOCKED state,
    WHEN the user drops a COIN,
    THEN we go to the UNLOCKED state.
    
    GIVEN we are in the UNLOCKED state,
    WHEN the user PASSes through the gate,
    THEN we go to the LOCKED state.

Do these two transitions fully describe the behavior of the subway turnstile?  Clearly not.  There are two states and two events.  That means there are four `{state, event}` pairs.  Therefore there should be four transitions as follows.

    LOCKED   COIN   UNLOCKED
    LOCKED   PASS   LOCKED (ALARM)
    UNLOCKED COIN   UNLOCKED (REFUND)
    UNLOCKED PASS   LOCKED  

The ALARM and REFUND notations are actions that must be executed as part of the respective transitions.  These extra transitions are the ones that business people tend not to think about.  They are well off the happy path of the system. After all, nobody expects a turnstile user to deposit a coin once the turnstile is unlocked.  

How often have you found, and fixed, bugs in systems that were due to some event happening in a state that didn't expect it?  If you are like me, it happens all the time.  That's because these situations are hard see.

However, when a system is described in state transition form, and when the states and transitions are well identified, then the hunt for the missing `{state, event}` pairs becomes trivial.  I have personally had a great deal of success in finding missing `{state,event}` pairs simply be being careful to identify and isolate states and events.

> **Spock:** _A reasonable discipline._

Anyway, the next time you are using BDD and/or Gherkin to specify a system, remember that what you are really doing is specifying a finite state machine.  If you are careful to identify the states and events, you will make it a lot easier to find the missing `{state,event}` pairs and create a more complete specification.

----
[1] The word "unit" is entirely inappropriate.  No one knows what a _unit_ is when applied to tests.  Some people call them _programmer tests_ instead.  Others call them _micro-tests_.  Whatever they are called, they are the tests that programmers produce when practicing TDD (Test Driven Development).  They are the tests that are used by programmers to describe the behavior of the system.

[2] "Acceptance" and "Integration" are inappropriate words here.  Some people call them _Customer_ tests.  They are the tests that the business uses to describe the behavior of the system.  

