---
layout: post
title: Functional Duplications
tags: ["Software"]
---
I broke out my old [Space War](https://github.com/unclebob/spacewar) game a few days ago and decided to make a few changes to speed the game up and make it more fun to play.  In so doing I discovered a very interesting bug.

One of the changes I made was to populate the initial space with a few random bases scattered here and there.  This would allow the player some extra resources with which to battle the Klingons while building up a network of more bases.

While I was playing the modified game, it crashed.  Hard.  

Now I wrote this with TDD, and I was very disciplined about the cleanliness of the code, and the test coverage.  So this was unexpected.  So I dug up all my old debugging skills from the pit in which I had buried them, and started to work out what was going on.

It wasn\'t long before I realized that crash was occuring because a transport was being launched between two bases, but the angle of the velocity vector of the transport was `:bad-angle`.  This can only happen if the two bases exist at the exact same location.

Bases don\'t move around in this game, so there\'s no chance that two bases will accidentally slide on top of each other.  There is a very (very) minor chance that the random number generator will put two bases on top of each other at the start of the game; but the odds are so miniscule that didn\'t worry about it. In any case, this crash happened well into the game I was playing, so initial values could not have been the cause.

Fortunately it\'s pretty easy to hunt and peck around in the game, so I was quickly able to discover that the two bases in question were duplicates of each other.  Something in my code was duplicating bases!

Well now that should\'t be too hard to find.  So I wrote a litte function that would examine the world and halt with a message if the world contained two bases at the same location.  I called this function in the main update loop, and sure enough after 20 minutes of play the program halted with my message.

Unfortunately being able to detect _that_ the duplication occurred did not tell me _where_ it occurred.  So I laced the code with calls to my `check-for-duplicate-base` function.  

It took me a few tries because the problem was not in any of the obvious places.  So over a few hours I added more and more calls to `check-for-duplicate-base`.  

Eventually I found the culprit in a low frequency function named `klingons-steal-antimatter`.

This function is called once per second.  It checks to see if any klingons are within `docking-distance` of a base, and if so it steals antimatter from that base.  

This explained why the crash took so long to create.  Most of the time it takes 20 minutes or so for a Klingon to move close enough to a base to start stealing.

Anyway, I looked at the code and didn\'t see any obvious duplication.  So I wrote a unit test to check whether that function duplicated bases.  My test positioned a klingon near a base, called the `klingons-steal-antimatter` function, and then checked the number of bases in the world.  The result: No duplication.

Now, before I continue, let me describe the process I used in the `klingons-steal-antimatter` function.

The function created a list of _thefts_.  A theft is a `[thief victim]` pair.  It used those pairs to create lists of all the thieves and victims, and separate lists of all the innocent klingons and all the unvictimized bases.

Why?  Because this is a purely functional program.  In a purely functional program you cannot update the status of an object.  Instead you transformm old objects into new objects.  So when stealing antimatter from a base you must create a new base with less antimatter, and you must create a new klingon with more antimatter.  When you are done processing all the thefts you are left with a list of all the updated klingons, and a list of all the updated bases.  

The `world` contains a list of all the klingons and a list of all the bases.  In order to update the `world` after processing the thefts you have to concatenate the updated bases with the unvictimized bases, and you have to concatenate the updated klingons with the innocent klingons.  

Got it?  Understand?  Good. 

As I pondered the code I realized that a base could be robbed by more than one klingon.  Klingons tend to slowly migrate towards bases and then steal from them.  Two or three or more could eventually manage to slide over to a base, like a pack of coyotes squabbling over a carcass.

Now I already had a unit test that checked for this condition.  It created two klingons near one base and made sure that each klingon was able to steal from that base.  What that test did not do, however, was count the number of bases in the world when it was done.  

So I added a check.  `base-count => 1`.  Whoops, it came back with `2`.  

Now maybe you\'ve already figured out why this happened.  But let me walk you through it.  My function identified two thefts: `[[k1 b] [k2 b]]`  It returned with the results of each theft. Let\'s say that `k1` stole `a1` antimatter from `b`, and `k2` stole `a2` from `b`.  What the function returned was `[[k1+a1 b-a1] [k1+a2 b-a2]]`.  Note that the second theft in the list was _not_ `[k2+a2 b-a1-a2]`.  

You\'ve probably guessed the rest.  When I reassembled the world, I added all the bases that had been victims; and \-\- of course \-\- I added `b-a1` _and_ `b-a2`.  

Fortunately I had lots of unit tests to fall back on.  Changing the algorithm was actually quite challenging, and required me to put all the klingons and bases into hashmaps keyed by their positions.  I won\'t bore you with the details.  

So I added unit tests to check for the duplications, saw them fail, and then gradually made them pass.  The unit tests allowed me to be sure that I was not breaking something else along the way.  

Now you might think this is just an esoteric little problem that you\'ll never encounter.  However, if you are writing functional programs, you _will_ face this issue, and you\'ll likely face it a lot.  Dealing with immutable lists of objects means that when you update such a list you must recreate it.  If you are only updating `m` out of `n` elements of the list, you have to partition the original list into the `m` elements you are changing and the `n-m` elements you are not changing; and then you have to concatenate the `m` changed elements with the `n-m` unchanged elements in order to create the updated list.  

Anyway, I thought you might find that interesting.