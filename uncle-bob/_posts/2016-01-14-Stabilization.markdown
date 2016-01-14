---
layout: post
title: Stabilization Phases
tags: ["Craftsmanship"]
---
While sipping my morning coffee, and scrolling through facebook on my phone, I found myself inundated with updates by Tesla owners who were excited that their cars could now drives themselves into a garage.  My response to those facebook updates was perhaps a little cynical; but I told them all:

>_It will be a long time before I trust software to drive car I am in. <br/>Because I -- **know**._

What is it that I know?  I know just how hard it is to test software systems for every contingency.  And I know just easy it is to fool yourself that you have.

And that started me thinking about how you should test the software for a self-driving car.

And _that_ started me thinking about how people _do_ test software systems.  

And _that_ finally started me thinking about _stabilization phases_.

You know what a stabilization phase is, don't you?  A stabilization phase occurs at the end of a release.  Time is set aside to just _let the system run_.  For a week, or a month, everybody just watches the system run.  They treat it like a sleeping baby.  They avoid loud noises, slamming doors, and loud conversation.  They tiptoe around, peeking in on it from time to time, hoping that it won't wake-up and fail.  

OK, maybe that's a little over the top.  -- Maybe.  I imagine most teams who run stabilization phases actually work pretty hard to stress their systems.  At least I hope they do.  They should be running lots of data through the system under varying loads; including data that is malformed, and that has caused problems in the past.  

But here's the thing about Stabilization phases:

>_We run them because we are afraid.  We are afraid because we are uncertain what the system will do._  

There is a certain dissonance between calling ourselves professionals, and being so uncertain about what we have created that we fear what it might do. One would expect a team of professionals to have a high degree of confidence and certainty.   

The longer a team wants a stabilization phase to run, the less certain that team is about the system.  The teams who only need a day are much more certain about their systems than the teams who want a week, or a month.  

The logical flaw here is that _execution time indicates quality_.  But time is actually unrelated to quality.  Time simply raises _false_ confidence.  

The behavior of the system in the stabilization phase has little to no bearing on the behavior of the system in production; because the data coming into the system in production is _entirely new data_.  That new data may drive the system down pathways that the stabilization phase never executed.  

So stabilization phases are really just about creating false confidence.  They are a CYA strategy.  When the system fails in production, you can at least claim that you performed the due diligence of running the system for a month in the stabilization phase; thereby exonerating the team from the culpability of leaving a critical defect in the system.

The crux of the issue is that stabilization phases exist because the development team has produced code that they are not certain of.  So they run the system for a month in order to create enough false confidence to counter that uncertainty.

What the team _really_ needs to do is attack their uncertainty directly.  Not by uselessly running the system for a month; but by correcting the deficits in their development process that led to their uncertainty.  Consider the following checklist:

 1. Are you running coverage tools?  Do you check that every `if` statement and `while`	loop are covered?  Is the unit-test coverage close to 100%.  Do you need to drive it a bit higher by writing more unit tests?
 2. Do you have automated acceptance tests written by (or at least validated by) the business and QA?  Is the coverage of these tests high enough?  Do you need to drive it higher by asking QA to consider more corner cases?
 3. Do you have automated integration tests written by architects and development leads.  Do those tests stress the communications pathways between the components?  Do they check for corner cases, boundary issues, and timeouts?  Do they probe system behavior under varying loads?
 4. If you have multiple threads, do you have a strategy for stressing those threads during your unit tests and acceptance tests?  For example, have you implemented tools that introduce random delays and random loads so that the chances of race conditions are magnified.  Better yet, are you gradually eliminating the possibility of race conditions by eliminating mutable state between threads?  Have you drawn all the message sequence charts and examined them for potential races?

This checklist is just an example.  I'm sure you can think of more things to put on it.   The point is that it is better to be proactive about your uncertainty, than it is to be passive about it.  And stabilization phases are _passive_.

The goal of software teams who are currently using stabilization phases should be to increase their certainty over time, and thereby decrease the duration of their stabilization phases.  Decrease them from a month, to a week.  Then from a week to a day.  Then from a day to an hour.

And then, finally, increase your certainty to the point that you can eliminate the stabilization phase _once and for all_.  


---
**Anecdote:**
>_I recently test drove a Tesla.  It's a fun car to drive.  I mean, really fun.  I tried the "auto-steer" feature, which is easily engaged by double clicking a button on the steering column.  The car happily informs you that the car is now driving itself; and warns you to keep your hands on the wheel.  **Heed that warning!**_  

<p/>
>_The car did tolerably well when the road markings were visible, but seemed quite willing to plow me into a bunch of construction barricades too.  It is **not** safe to take your hands off the wheel or your eyes off the road.  To me, that makes the feature rather less than useless._

<p/>
>_The salesman was sitting next to me.  At one point we were going 45mph towards the rear end of a car stopped at a red light.  The salesman said: "Trust the car."  And I thought: "The hell I will!"  And **I** applied the brakes._

<p/>
>_This technology is cute; but dangerous.  **NEVER** "trust the car"!_

<p/>
>_Perhaps you can tell that the prospect of self-driving cars does not fill me with enthusiasm.  I shall continue to wonder just what those cars will do one second after midnight on February 29th._

