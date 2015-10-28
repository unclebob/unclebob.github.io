---
layout: post
title: "Monogamous TDD"
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2014/04/25/MonogamousTDD.html" />
When a [blog](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html) begins like this...

>"Test-first fundamentalism is like abstinence-only sex ed: An unrealistic, ineffective morality campaign for self-loathing and shaming."

... you have to wonder if the rest of the post can recover its credibility, or whether it will continue as an unreasoned rant.

Take the first two words: "_Test-first fundamentalism_".  Fundamentalism is a term that _used_ to mean: "back to basics"; but since 911 has taken on the connotation of violent extremism.  I have yet to see any test-driven developers flying airplanes into buildings while repeatedly hollering: "Kent Beck is great!", so I must entirely reject the connotation.  

Are there test driven developers who are passionate about their discipline?  Absolutely, myself included; but I have yet to hear about hordes of TDDers rampaging through the countryside, conquering by the sword.  So, I must conclude that the author used the word "fundamentalism" as a snide way of casting those of us who practice Test Driven Development in a deeply pejorative light.

Take the next sentence fragment: "_abstinence only sex ed_".  To my knowledge no one seriously teaches abstinence only sex education.  What _some_ folks teach is monogamy: the practice of keeping sexual contact within the context of a committed relationship e.g. marriage.  

Now it's true that in certain religious groups monogamy is coupled to morality.  However, there are secular schools of thought (my own) in which monogamy is simply thought of as a good personal strategy without deep moral consequence.  Given the Herpes epidemic of the 70s that was overshadowed by the ongoing HIV epidemic that began in the 80s and 90s; and given that single motherhood is among the strongest factors correlated with poverty, perhaps the strategy of monogamy should not be used as a pejorative adjective.

Of course people might think that, given the divorce rate of 50%, the strategy of monogamy is less than optimal. However, when one considers that 70% of first marriages last until the death of one partner; and that a similar fraction of second marriages last almost as long, the strategy of monogamy starts to look a bit better. 

And, after all, from a purely practical point of view, there is simply no better _personal_ strategy for preventing disease and unwanted children.  (And, at least in my experience, a happy and rewarding life.)

In that light, the second half of the author's statement: "_An unrealistic, ineffective morality campaign for self-loathing and shaming._" starts to look pretty, well... ignorant.

Of course I understand what the author was trying to say.  There is a stridence in the preaching of TDD that makes him uncomfortable.  I have used that stridence myself; and I believe the stridence is called for.  The reason is simple.  As an industry, we suck.  If you aren't doing TDD, or something as effective as TDD, then you _should_ feel bad.

Why do we do TDD?  We do TDD for one overriding reason and several less important reasons.  The less important reasons are:

1. We spend less time debugging. 
2. The tests act as accurate, precise, and unambiguous documentation at the lowest level of the system.
3. Writing tests first requires decoupling that other testing strategies do not; and we believe that such decoupling is beneficial.

Those are ancillary benefits of TDD; and they are debatable.  There is, however, one benefit that, given certain conditions are met, cannot be debated:

* If you have a test suite that you trust so much that you are willing to deploy the system based solely on those tests passing; and if that test suite can be executed in seconds, or minutes, then you can quickly and easily clean the code without fear.

Now there are two predicates in that statement, and they are _big_ predicates.  But, given those predicates are met, then developers can quickly and easily clean the code without fear of breaking anything.  And _that_ is power.  Because if you can clean the code, you can keep the development team from bogging down into the typical [_Big Ball of Mud_](http://en.wikipedia.org/wiki/Big_ball_of_mud).  You can keep the team moving fast.

Indeed, the benefit of keeping the code clean, and keeping the team moving fast, is so great, that those two predicates begin to pale in comparison.  _Yes!_ If I can keep the team moving fast, then I _will_ find a way to trust my test suite, and I _will_ keep those tests running fast.

Anyway, that's where the stridence comes from.  Those of us who have experienced a fast and trustworthy test suite, and have thereby kept a large code base clean enough to keep development going fast, are _very_ enthusiastic.  So enthusiastic, in fact, that we exhibit a stridence that the author has unfortunately, and inaccurately, dubbed  as "fundamentalism"; claiming it to be ineffective and unrealistic.

What does the author suggest as an alternative?  As someone who writes systems in Rails he suggests integration tests that use the database and operate through the GUI (using Capybara).  

My response to this is: If you can meet my two predicates of trustworthiness and speed, go for it!  If you trust those integration tests so much that you are willing to deploy when they pass; and if they execute so quickly that you can continuously and effectively refactor and clean the code, then you aren't doing any better than me.  Do it. 

But (and this is a big "but"), it seems to me that integration tests have very little chance of meeting my two predicates.  

First I doubt they can attain the necessary trustworthiness because they operate through the GUI; and you can't reach all the code from the GUI.  There's lots of code in a normal system that deals with exceptions, errors, and odd corner cases that cannot be reached through the normal user interface.  Indeed, I reckon you can only cover a bit more than half the code that way.  It seems unlikely to me that anyone would be willing to deploy a system based on tests that leave such a large fraction of the code uncovered.  

Second, it seems very unlikely to me, despite the ability to spin up hundreds of servers in the cloud, that you can get those tests executed in anything like the speed you'd need to effectively and continuously refactor the code. Databases and GUIs are _slow_. 

Now, I could be wrong.  I'd love to be proven wrong.  And perhaps, despite the author's poor reasoning at the start of his blog, he _really can_ trust his tests enough, and execute them quickly enough, to make them effective for keeping the code clean.  If so, then I'll holler: "Amen, Brother, and Hallelujah!" and will become a strident convert to his particular brand of fundamentalist polygamy.






