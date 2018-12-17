---
layout: post
title: FP vs. OO List Processing
tags: ["Software"]
---
While writing [spacewar](https://github.com/unclebob/spacewar) over the last few months, I came across an interesting difference between functional programming, and OO programming.

Let's imagine that we've got two lists.  A list of klingons, and a list of shots traveling through space.  The shots might be phaser beams, or photon torpedos, or balls of iron called "kinetics".  In all cases they have a position and a velocity.  So do each of the klingons.

We have a frame rate of `f` frames per second.  At each frame we look at the positions of the shots and compare them to the positions of the klingons.  We are looking for a hit.  

If we find a hit, we delete the shot from the list of shots, and we add the corresponding hit to the klingon, and an explosion to the list of explosions in the world.

In Java, the code might look like this:

	void updateHits(World world){
	  nextShot:
	  for (shot : world.shots) {
	    for (klingon : world.klingons) {
	      if (distance(shot, klingon) <= type.proximity) {
	        world.shots.remove(shot);
	        world.explosions.add(new Explosion(shot));
	        klingon.hits.add(new Hit(shot));
	        break nextShot;
	      }
	    }
	  }
	}
	
Note the labelled break.  That's the first time I've ever been tempted to use one of those.  

Anyway, this code is pretty straightforward, isn't it?  But it's not at all functional because it alters the hit klingons and the world.

A functional program that does the same thing must not alter the klingons, or the world, in any way.  What it must do instead is create a new world, with new klingons, and new shots, explosions, and hits.  Remember, that's what functional programming is all about.  You aren't allowed to change the value of any existing variables.  

You might ask how you can change the state of the game if you can't change the state of any of the game variables?  The answer is pretty straightforward: You use tail recursion.

Essentially, you have a function that transforms the world though one step in time.  The result of that function is an entirely new world.  Then you call that function in a recursive loop like this:

	void updateWorld(World world) {
	  drawWorld(world);
	  updateWorld(transformWorld(world));
	}
	
If you are a Java programmer, you'd be worried about the stack overflowing.  But in functional languages (and in most other modern languages other than Java) there is a lovely little trick called [_Tail Call Optimization_](http://wiki.c2.com/?TailCallOptimization) that eliminates that problem if the recursive call is the very last operation of the function.  

So how would one write the `update-hits` function in a functional language like Clojure?

Take a look at this:

<img src="/assets/update-hits.clj.png">

That looks a bit different from the Java function, doesn't it?  It should.  It's doing a lot more than that Java function had to do.  So let's walk through it.

 * First we get all the relevant shots.  These are the three kinds of shots that our ship can fire at targets like Klingons or Romulans.  I didn't include this is the Java example above, so this is just extra code.
 
 * Next we get the targets. For our purposes, this is just the list of all the Klingons.  Though it could also be the list of all the Romulans.
 
 * Next we get all the pairs of targets and shots along with their distance from each other.
 
 * Then we filter the pairs for those whose distance is less than the proximity of the weapon.  These are the pairs that represent hits.
 
 * Then we convert those pairs into a list of just the hit targets, and another list of just the shots that did the hitting.
 
 * Next, and this is the interesting bit, we get the list of all the Klingons that were NOT hit.  We also get the list of all the shots that did NOT hit.  Why do we need these?  Read on.
 
 * Next we create the hit-targets. This updates each hit target with the hit itself.
 
 * Then we create a list of all the new explosions added to all the other explosions in the world.
 
 * Finally, we build the world.  The world is a copy[1] of the old world, but with:
   -  the Klingons replaced by the concatenation of the hit klingons and the unhit klingons.
   -  The shots replaced by only the shots that didn't hit.
   -  And the explosions replaced by the sum of old and new explosions.
   
Now the part that I find interesting about this is that in order to build a new instance of the world, I must keep track of those parts of the world that changed, like the klingons, shots, and explosions, _and_ all the parts of the world that _did not_ change.  I must separate them.  Make the modifications.  Then add them back together again.  I can't just reach into the world and make specific changes.

At first I thought that this was extra work.  It seemed wasteful to me that I had to separate and keep track of the unchanged elements.  However, then I had a flash of insight.

Imagine that you have a dozen bicycles in your garage.  You and your partner are about to enter a race, and you need to select and prepare two bicycles.  You must separate those two bicycles from the rest, perpare them, race them, and then put them back into the garage with the rest of the bicycles.  

I'll let you ponder that for awhile.[2]

---------

[1] Actually, it's not really a copy.  Most functional languages, including Clojure, use [very clever schemes](https://hypirion.com/musings/understanding-persistent-vector-pt-1) to prevent all that copying by strategically sharing the elements that did not change. 

[2] This is just one of the points of discussion about spacewar in the [Clean Code Functional Programming series](https://cleancoders.com/videos/clean-code/functional-programming) on [cleancoders.com](http://cleancoders.com).     




