---
layout: post
title: Giving Up on TDD
tags: ["TDD"]
---
>_Did you read Ian Sommerville's [recent blog](http://iansommerville.com/systems-software-and-technology/giving-up-on-test-first-development/) about TDD?_

You mean the one where he says that he tried it for a few months and then gave up?  Yes, I read it.

>_Well?  What did you think?_

I think he gave up too quickly and too easily.

>_Well, he said he tried it for a few months.  Isn't that long enough?_

Normally, it should be; but he said he was just using it for some home projects. I doubt he was using it 40 hours per week for a few months.  I suspect the actual number of hours he logged was relatively low; because I recognize the symptoms.

>_The symptoms?  What do you mean?_

The symptoms that most TDD novices experience in the first couple of weeks.  Sommerville spells them out pretty clearly.  His first is a classic -- it even has a name:

###The Fragile Test Problem
<div style="margin-left: 5em"><font color="#5F4C0B">* Because you want to ensure that you always pass the majority of tests, you tend to think about this when you change and extend the program. You therefore are more reluctant to make large-scale changes that will lead to the failure of lots of tests. Psychologically, you become conservative to avoid breaking lots of tests.</font></div>

>_This is a common problem?_

Sure.  And a very, very, old one.  

Forget about tests.  If you have one part of your system that breaks whenever you change another part of your system, what can you conclude about the design of that system?

>_Well, you'd probably conclude that the part that breaks is tightly coupled to the part that changes._

Right.  And that's what Sommerville is experiencing with his tests.  His tests are tightly coupled to his production code.

>_Well, sure, that's normal isn't it?  I mean, tests really do have to be coupled to the production code, don't they?_

Not so coupled that they break all the time.  If many of your tests break every time you change the production code then you have over-coupled the tests to the code.  You have a _test design problem_.

>_A test design problem?_

Yes.  You haven't properly designed the interface between your production code and your tests.

>_Wait.  What?  There's an _interface_ between the code and the tests?_

Of course there is.  And that interface has to be _designed_.  

>_But isn't the interface between the tests and the code just the low level functions inside the code that the tests are calling?_

They are if you want to have fragile tests.  But if you want to properly decouple your tests from your production code, you design an API for those tests.  

By the way, that API will also be the API that other layers of the system use to communicate.

>_Other layers?_

Yes.  The other layers of the system.  You _do_ compose your system out of layers don't you?

>_um..._

This is software design 102.  Compose your system out of independent layers that communicate through well defined interfaces.  

And this gets into another one of Sommerville's complaints...

>_Wait.  Before you rush ahead, I need to understand your current point._

OK.

>_You are saying that when you use TDD, you have to _design_ the tests?_

Not just the tests.  You have to _DESIGN_ period.  No matter what you are writing; whether a unit test, or an acceptance test, or production code, or a mock, or a stub, _you have to DESIGN._

>_But I thought TDD meant that you didn't have to design._

Yeah, and: "Love means you never have to say your sorry."  What a bunch of horse hockey!  We are _programmers_!  We _design_!  We create structures with high cohesion and low coupling.  We manage dependencies.  We isolate modules.   __WE.  DESIGN.__

>_OK, and so what you are saying is that people who start using TDD forget to design?_

Sometimes they forget.  Sometimes they've been wrongly told not to design.  But most of the time they are so focussed on the new discipline that they don't have room in their brains to think about design.  

This happens to all novices, no matter what the new discipline is.  When you first learn to drive you are so focussed on controlling the car that you can't afford the brain power required to recognize a stop sign or a stop light.  That's why we learn to drive with an experienced driver in the seat next to us.  It takes time to get comfortable enough with the controls to start engaging the parts of our brain that recognize stop signs and stop lights.

>_And you think this is what happened to Sommerville?_

I know it is.  I know this because developers who are experienced with Test Driven Development do not experience the _Fragile Test Problem_.  

>_OK, so you said this leads to another of Sommerville's complaints._

Absolutely.  

###The Design Problem
<div style="margin-left: 5em"><font color="#5F4C0B">* The most serious problem for me is that it encourages a focus on sorting out detail to pass tests rather than looking at the program as a whole. I started programming at a time where computer time was limited and you had to spend time looking at and thinking about the program as a whole. I think this leads to more elegant and better structured programs. But, with TDD, you dive into the detail in different parts of the program and rarely step back and look at the big picture.</font></div>

>_So what is the connection?_

It's really the same issue.

>_How do you mean?_

As a novice, when you are focussed on the discipline of TDD, you don't have room in your brain for a lot of design thinking.  That's one of the reasons we push the notion of refactoring so much.  The idea that TDD "encourages a focus on sorting out detail to pass tests rather than looking at the program as a whole" is simply an artifact of being a novice.  

>_But wait.  I mean, tests _are_ all about detail, aren't they?_

Sure.  _Code_ is all about detail.  But that doesn't mean you aren't thinking about the problem as a whole.  Nobody said that in order to practice TDD you have to abandon the big picture.  

On the contrary, Ron Jeffries, one of the original TDDers has repeatedly stressed: _"Act locally.  Think Globally."_  That's good advice for any programmer.

>_So Sommerville was wrong about this too?_

No!  Not wrong.  This is exactly what anyone would experience as part of the learning curve of TDD.  It takes time and experience with the discipline to get past these hurtles.  More time than I believe Sommerville gave it.  I think he just gave up too soon.

The bottom line is that _you must never abandon the big picture!_  Sommerville was right about that. He was just wrong that TDD promotes that abandonment.  It's being a novice with the discipline that promotes the abandonment of the big picture.  

>_OK, so then what about his other complaints?_

##The Testable Design Problem
<div style="margin-left: 5em"><font color="#5F4C0B">* It is easier to test some program designs than others. Sometimes, the best design is one that's hard to test so you are more reluctant to take this approach because you know that you'll spend a lot more time designing and writing tests (which I, for one, quite a boring thing to do)</font></div>
<p/>
The first part of this complaint has an element of truth to it.  Some things are harder to test than others.  GUIs are hard to test.  Device drivers are hard to test.  Indeed just about anything that interacts with an IO device is hard to test.  So we have developed strategies for dealing with that.  Strategies like _The Humble Object_ pattern.

>_The what?  The Humble what?_

The Humble Object pattern.  Michael Feathers and Gerard Meszaros wrote about this years ago.  Look it up.  

>_OK, but you said you only agreed with the first part of his complaint._

Right.  The second part is nonsense.

>_Really?  Nonsense?  Isn't that kind of, um...  harsh?_

Not at all.  The notion that: "sometimes the best design is one that's hard to test" is the highest order of drivel. 

>_I can see that you aren't backing down on your harshness._ 

No, I'm not.  This is a very simple and important point.  Let me state it much more clearly.

__Something that is hard to test is badly designed.__

>_Hmmm.  I'm not sure..._

Look.  Suppose you ask me to write an app to control your grandmother's pacemaker.  I agree, and a week later I hand you a thumb-drive and tell you to load it into her controller.  Before you do you ask me: "Did you test it?"  And my response is:  "No, I chose a design that was hard to test."

>_Hmmm.  Yeah.  OK.  I guess I see..._

Do you?  Are you sure?  Let me drive that home even more.  

Any design that is hard to test is crap.  Pure crap.  Why?  Because if it's hard to test, you aren't going to test it well enough.  And if you don't test it well enough, it's not going to work when you need it to work.  And if it doesn't work when you need it to work the design is crap.

ARE WE UNDERSTANDING THIS?

>_Yes.  I see your point.  Good designs are easy to test._

Yeah.  Forget that, and all is lost.

>_OK, well, Sommerville had one last complaint._

##The Magic Bullet Problem
<div style="margin-left: 5em"><font color="#5F4C0B">* In my experience, lots of program failures arise because the data being processed is not what's expected by the programmer. It's really hard to write 'bad data' tests that accurately reflect the real bad data you will have to process because you have to be a domain expert to understand the data. The 'purist' approach here, of course, is that you design data validation checks so that you never have to process bad data. But the reality is that it's often hard to specify what 'correct data' means and sometimes you have to simply process the data you've got rather than the data that you'd like to have.</font></div>

Of course he's absolutely correct.  My problem with this complaint is that I have no idea what it has to do with TDD.

In effect Sommerville is saying:  "TDD doesn't solve world hunger. So I'm giving up."

>_Well, I'm not sure I'd go that far._

It's true that TDD is not going to help you defend against things you didn't anticipate.  That's not a failing of TDD because that's not a promise that anyone has made about TDD.  

 * TDD will not cure cancer.  
 * TDD will not bring world peace.  
 * TDD will not protect you from drunk drivers.
 * TDD will not bring sanity to American elections.  

>_I think you should stop ranting about this._

Yeah, OK, I just find is frustrating that anyone would give up on TDD because it doesn't cure athlete's foot.

>_I said stop ranting._

OK.  OK.  Sorry.  Urgh.

>_So then is there no solution to this problem?_

I never said that.  I just said that TDD never _promised_ to solve that problem.  

>_So what can we do?  How can we protect ourselves from unanticipated data._

Well, it's not just unanticipated data.  It's unanticipated _anything_.  And the way to address that is to work hard at anticipating as much as possible.  That's one of the reasons we have other people, like business analysts and Quality Assurance testers, write acceptance tests.  

>_OK so you are saying that the solution to this problem is to get lots of people involved._

Of course.  There really isn't any other way.  And, by the way, even that will fail.

>_You aren't offering a lot of hope._

Look.  Apollo 1 caught fire.  Apollo 13 exploded half-way to the Moon.  Challenger blew up just after launch.  Columbia broke apart during reentry.  Why?  Because despite the thousands of brains trying to think of everything, something unanticipated happened.

>_So you are saying..._

Deal with it.  There will always be risk.  Don't blame TDD, and don't give up.



