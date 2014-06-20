---
layout: post
title: The Humble Craftsman
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="0; url=http://blog.8thlight.com/uncle-bob/2013/02/01/The-Humble-Craftsman.html" />
There is another side to Ted Neward's [blog](http://blogs.tedneward.com/2013/01/24/On+The+Dark+Side+Of+Craftsmanship.aspx); and it's a side that I agree with.  I believe Ted's overall thesis and analysis was wrong.  Software Craftsmanship does not automatically give us a blue-collar/white-collar dichotomy; it does not automatically separate those who "get it" from those who don't.  It does not automatically create a condescending elite who lord their self-perceived superiority over the unwashed masses.  And, in case you hadn't noticed, I _really_ disliked Ted's manipulative appeal to populism.

However, Ted was not entirely wrong either.  Because there are folks (and, to my shame, I've sometimes numbered among them) who have behaved badly when pointing out problems or deficiencies in other peoples' code.  To those people (and to myself) I'd like to make the following points.

It takes a _lot_ of courage to put code out on github for everyone to see.  It requires a willingness to be exposed, ridiculed, and belittled.  It shows a desire to share, and a hunger to learn.  It is possibly the most honorable and selfless act a programmer can make.  That act should not be rewarded by haughty condescension.  No one should point the their fingers and snicker with their buddies.  The honorable act of sharing should be respected, not denigrated.

What does it take to be a craftsman?  It takes time.  It takes experience.  It takes mentoring.  And, it takes a _lot_ of trial and error.  In our industry the best, and possibly the only, way to refine your skill is to make lots and lots of mistakes; and to learn from others who have made lots and lots of mistakes.  So _thank goodness_ for those mistakes, and thank goodness for the people who made them.  Without them, we'd have learned nothing.  And _especially_ thank the _people_ who were willing to expose their mistakes to the world.

Several years ago I stumbled upon some open source code that I thought was a good example of bad code.  It was written in 2002 by David Gregory.  I wrote to David, and asked if I could use his code as an example in my book "Clean Code".  He graciously agreed.  His is one of the most courageous acts I've encountered.  

>Bob: "Excuse me, David, do you mind if I rip this code, that has your name all over it, to shreds in front of a million people?"  
  
>David: "Sure, Bob.  Go right ahead."

Would _you_ have allowed that?  Would _you_ have had the courage to let _Uncle Bob_ point out every minuscule problem in your code in front of a huge audience of young and eager programmers?  Could you have withstood that negative review as, year after year, it was published over and over again, in country after country, and language after language?  Could you have tolerated being _the_ person who wrote _the_ example of bad code?

I owe a lot to David, and so does everyone who read my book and learned anything from it.  Were it not for heroes like David, we could not advance the cause of craftsmanship at all. 

BTW, I should point out that David's code was not really all that bad.  For Java, in 2002, it was considerably better than average.  When one writes a book about clean code, the only examples that make sense to use are those where the difference is small enough to be seen.  

When someone shares their code, the respectful and honorable thing to do is to carefully critique that code.  _No one's code is above criticism._  Criticism is, after all, how we learn.  Respectfully criticizing someone's code is one of the highest honors you can pay to the author.  Just remember, you respect the _person_, not the code.  The code is fair game.

Reviewing and criticizing code is a balancing act.  To do it well requires a delicate combination of ruthlessness and humility.  You have to be able to say that certain things are just silly -- even stupid.

For example, this is stupid:
{% highlight java %}
/**
 * Default Constructor
 */
public MyClass() {}
{% endhighlight %}

Can I say that?  Can I say "stupid".  Yeah, I can.  Because I've been there.  I've been stupid.  And I'll be stupid again.  I'm the guy who wrote that code.  I'm the guy who wrote a 3,000 line C function named `gi` (which stood for Graphic Interpreter).  I'm the guy who wrote an 2,000 line O(n**3) algorithm for calculating the area of a thousand-sided polygon because I couldn't be bothered to look up the lovely 30 line, O(n) solution.  I'm the guy who got fired, while my wife was pregnant with our first child, because I couldn't be bothered to think about schedules that were _real_ important to my employer.  Yeah.  Me.  The stupid one.  So I'm allowed.  I can use that word.  

And if _you_ use that word, or any other adjective that means the same thing, just remember you are using a word that describes yourself; because the only way you can _know_ what's stupid, is to _have done_ something that stupid in the past.

And that's a good way to describe a craftsman.  A craftsman is someone who has done some really stupid things and wants to avoid doing them in the future, and to help others to avoid doing them too.

If you want to bear the title of "Craftsman", then you must respect every person who shares their code; and show them the honor that they deserve.  You treat each sharing event as a courtesy paid to you; and return that courtesy _with courtesy_.  This doesn't mean the code is above criticism.  It just means that when you criticize, you do so with courtesy, respect, and a humble acknowledgement of your own failings.

So if you see some bad code out there. There's nothing wrong with pointing it out.  Indeed, you _should_ point it out.  Just remember that the only reason you recognize it as bad code is because either you, or someone who has taught you, has written bad code like that in the past.  So be humble.  Acknowledge our shared stupidity.  Commiserate just how difficult writing good clean code is, and how easy it is to do stupid things without knowing it.

And never, ever, EVER, point your finger and snicker with your buddies like a gaggle of gossipy highschoolers.

A special thanks to Kelly Sommers (`@kellabyte`) for sending me the email that inspired this blog.





