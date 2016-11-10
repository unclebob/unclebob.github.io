---
layout: post
title: TDD Doesn't Work
tags: ["Software"]
---
TDD Doesn't work.

>_It doesn't?  That's odd.  I've always found it to work quite well._

Not according to [a new study](http://people.brunel.ac.uk/~csstmms/FucciEtAl_ESEM2016.pdf).

>_Another study?_

Yeah, an in-depth study that repeated another study that was done a few years back.  Both showed that TDD doesn't work.  The new one uses a multi-site, blind analysis, approach.  It looks conclusive.

>_Do the authors consider it conclusive?_ 

The authors recommend more study.  But they're probably just being humble.  The data is pretty convincing.

>_What is the data?_

The study shows that the claims about TDD are false.  TDD doesn't make you go faster; it doesn't reduce defects; and it doesn't help you to write better code.

>_That's very strange.  I think TDD does make me go faster, improve my code, and my accuracy.  I know others who have said the same.  So I'm puzzled about why this study would show something different._

Well, it did.  DHH was right.  [TDD is Dead](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html).  

>_Hmmm.  OK, so what exactly did the authors study?  How did they come to this conclusion?_

I don't know, I just know there was a study.

>_How did you find out about the study?_

I read a [blog](http://neverworkintheory.org/2016/10/05/test-driven-development.html) about it.  At the end the author said that the study has made him reconsider TDD.  He used to think it worked.

>_OK, well, let's look at the study.  Hmmm.  Yes, right here it says that they compared TDD to TLD._

What's TLD?

>_Test LAST development.  That's when you write your unit tests _after_ you write your code._

See? So the study showed that it's better to write your tests last!

>_Hmmm.  No, that doesn't seem to be what the study showed.  In fact, the study found that there was no significant difference._

OK, fine.  So if I write my code, and then write my tests it's just as good as TDD.  

>_Well, no, not quite.  At least that's not what the study showed.  The study asked the folks doing TLD to work in "small chunks"._

Small Chunks?

>_Yes.  The folks doing TLD would write a little bit of production code, followed by a little bit of test code._

Oh.  I see.  So they'd write production code for 10 minutes and then write unit tests for ten minutes or something like that.

>_Well, maybe.  But, see here, it says that all the participants were trained in TDD.  And then some of them were asked to do TLD in small chunks._

Right.  OK.  So, my statement still holds.  They wrote production code, then they wrote unit tests; and it didn't matter.

>_So let me ask you how you would write unit tests, _after_ production code; but in small chunks._

I'd write some production code -- enough to pass a test or two -- and then I'd write those tests.

>_How would you know how much code would pass a test or two?_

I'd think of a couple of tests to pass, then I'd write the code that would pass those tests.  Then I'd write the tests.

>_And since you had been trained in TDD; that kind of thought process would be natural to you; wouldn't it?_

Um.  Hmmm.  I think I see your point.  The TLDers were doing TDD in their heads, and then just reversing the order.

>_Right.  In order to work in small chunks, they had to imagine the tests that they'd be writing; so that they could write production code that was _testable.

So maybe this study wasn't studying what they thought they were studying.

>_It seems to me that they were trying to study the _order_ of writing the tests, more than the process of TDD.  In their effort to reduce the number of variables they inadvertently eliminated them all.  They forced the participants doing TLD to use the TDD _process_ of short cycles, and that forced the participants to drive the production code by thinking about tests first._

OK.  Maybe.  But still, those TLDers _did write their tests last_.  So at least the study showed that you don't really have to write the tests first -- so long as you work in very short cycles.

>_Sure.  The really effective part of TDD is the size of the cycle, not so much whether you write the test first.  The reason we write the tests first is that it encourages us to keep the cycles really short._

So what the study showed is that people who work in short cycles don't have to worry about writing tests first, so long as they continue to work in short cycles.

>_That's probably a fair statement.  However, look here.  The problem that the participants were solving was _The Bowling Game_. This is a very small problem.  In fact, they said the entire programming session took three hours._

Is that important?

>_Sure.  The benefit of writing the tests first is _disciplinary_.  Writing the test first keeps your cycles short; and keeps your coverage high, over long periods of time._

OK, but if you had enough internal discipline to keep your cycles short, then the study shows that it doesn't matter if you write your tests first.

>_That's a big "if"; but sure.  The study shows that if you take a group of people, trained in TDD, and then tell them to keep everything the same, including the size of their cycles, and just change the ordering of the tests, then in three hours of programming you won't see much difference._

Yeah.  Yeah.  That's what the study shows.

>_So, really, the study was making a distinction without a difference._

Well..  Heh, heh, they _found_ no difference, so I guess that's right.

>_So the study didn't show that TDD doesn't work, did it?_

No, I guess not.

>_What _did_ it show?_

I think it showed that you can't interpret the conclusions of a study without _reading_ the study.









