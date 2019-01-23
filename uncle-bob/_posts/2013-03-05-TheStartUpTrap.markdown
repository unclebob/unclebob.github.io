---
layout: post
title: The Start-Up Trap
tags: ["Craftsmanship", "Testing"]
---
* **You** have joined a new startup.
* **You** are a multi-talented mega-being.
* **You** can work 60, 70, 80 hours per week to get the job done.
* **You** are a _top-notch_ coder and designer.  
* **You** won't fall into the traps that others have fallen into.
* **You** will make sure that _this_ time will be different.
* **You** are _so_ good that the rules don't apply to **you**.
* **You** are _fucked_.

### The Start-Up Trap.
It's a sad story that we've seen over and over again.  A young programmer begins with all the best intensions, learns all the right disciplines, develops all the right skills, and then falls prey to _The Start-Up Trap_.   The Start-Up Trap is the idea that _your_ situation is different -- that everything you've learned about how to do software well somehow doesn't apply to _this_ particular job.  You think it will apply later, once you've succeeded.  But not now.  Not yet.  Not while you are in a race to succeed.  

The Start-Up trap is the thought that the start-up phase is _different_; and that while you are in that phase success depends upon _breaking_ the rules.  This is _stupid_.  The start-up phase is _not_ different.  The start-up phase is simply the first of many phases, and it sets the tone for all those other phases.  Come back to that company in five years and (if they've managed to survive) they'll still have the same attitude towards the rules that they had in the first phase -- except, perhaps, for the overtime. (giggle).  

Here's a little tip:  The disciplines that lead to successful software are always valid, no matter what phase the company is in.  It is laughable to think that good disciplines are less important during the start-up phase.  The truth is that, during the start-up phase, those disciplines are just as critical as they are at any other time.

Of course one of the disciplines I'm talking about is TDD.  Anybody who thinks they can go faster by _not_ writing tests is smoking some pretty serious shit.  Oh, I know you are a warrior-god.  I know you can write the code perfectly every time.  I know that the deadline looms and you _just don't have time to write tests_.  -- I'm sorry for your impending failures.  I'm sorry that you're going slow and just don't know it yet.  And I'm _very_ sorry that when you finally brute-force your way to some modicum of success that you will credit your bad behavior, and recommend it to others.  God help us all, because you won't.

Ask yourself this:  How does the accounting officer of a start-up behave?  This person is responsible for managing the money of the investors.  Do you think that accountant has deadlines?  Do you think he's under pressure to deliver projections, forecasts, cash-flow reports, etc?  Do you think his bosses tolerate schedule slips in his duties?  I'll tell you now that the guy managing the investors' money is under a hell of a lot more pressure than any software developer is.  

So how does this accountant behave?  Does he double check his work?  Does he practice double-entry bookkeeping?  Does he follow all his rules and disciplines?  Or are the rules different because he's in the start-up phase? 

What if it was _your_ company, and _your_ money.  What would _you_ think of a start-up accountant who didn't check his sums; who neglected the debit side of the books and trusted the health and future of _your_ company to the single unchecked sums of the credit side?

You'd fire his ass!  You'd fire it so fast that the rest of his worthless carcass would be left outside the door wondering where his ass went!

Is your code somehow less important than that account's spreadsheets?  Are errors in the code somehow more tolerable than errors in those spreadsheets?  Can errors in the code take the company down and ruin it's reputation with it's customers, and investors?  You know the answer to these questions.  And you know this:  If accountants can find a way to practice their disciplines in a start-up; _so can you._

Is neglecting TDD going to help you go fast?  To quote Captain Sulu when the Klingon power moon of Praxis exploded and a young Lieutenant asked whether they should notify Star-Fleet:  "Are you kidding?"  ARE YOU KIDDING?

NO, you aren't going to go fast.  You're going to go _slow_.  And the reasons are simple, and you already know them.  You're going to go slow because you won't be able to refactor.  The code will rot -- quickly.  It will get harder and harder to manage.  And _you will slow down._

You won't notice it at first because it still _feels_ fast.  You are working hard and spending 60, 70, 80 hours per week on the code.  The sheer effort you are applying is enormous; and that _feels_ fast. 

But effort and speed are not related.  It is easy to expend a tremendous amount of effort and make no progress at all.  _Hell_, it's easy to expend gargantuan effort and make _negative_ progress.  _Effort equates neither to speed nor direction._

As time passes your estimates will grow.  You'll find it harder and harder to add new features.  You will find more and more bugs accumulating.  You'll start to parse the bugs into critical and acceptable (as if any bug is acceptable!)  You'll create modules that are so fragile you won't trust yourself, or anyone else, to modify them; so you'll work around them.  You'll build a festering pile of code that, with every passing week, requires more and more effort just to keep running. Forward progress will slow and falter.  It may even reverse as each release becomes buggier and buggier, and less and less stable.  Catastrophes will become more and more common as errors, that should never have happened, create corruptions and damage that take huge traunches of time to repair.

You _know_ the story.  You _know_ this is where others have wound up.  If you are old enough, _you_ have probably wound up there once or twice yourself.  And yet that Start-Up Trap still sings it's siren song and lures you into destructive, slow, catastrophic behaviors.

If you want to go _fast_.  If you want the best chance of making all your deadlines.  If you want the _best_ chance of success.  Then I can give you no better advice than this:  _Follow your disciplines!_  Write your tests.  Refactor your code.  Keep things simple and clean.  _Do Not Rush!_  You hold the life-blood of your start-up in your hands.  _Don't be careless with it!_

Remember: _The only way to go fast, is to go well._






