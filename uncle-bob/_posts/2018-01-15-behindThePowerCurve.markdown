---
layout: post
title: Operating Behind the Power Curve
tags: ["Software"]
---
I promise that this blog is about software.  So bear with me for a bit.

What happens when you increase the throttle in an airplane?  You go faster, right?  More power to the engine means more thrust which means more speed.

Most of the time this is true; but there's a different mode you can get the aircraft into which reverses this relationship.  It's called: _the region of reversed command_.  

Remember the four forces that govern flight: gravity, lift, thrust, and drag.  Lift opposes gravity, and thrust opposes drag.  An airplane, in straight and level flight, balances all these forces perfectly.

Thrust, of course, comes from the engine.  Lift comes from the air flowing around the wings, and drag...?  Well, drag comes from two different sources.

Parasitic drag is simply the cost of plowing through the air.  It is the air resisting the movement of the plane.  But there's another kind of drag called _induced drag_.

Induced drag is caused by the pilot.  It happens when the pilot raises the nose of the aircraft.  Raising the nose, changes the angle of the wings causing the lift vector, which is always pependicular to the wings, to point a bit _backwards_, thereby _opposing_ the forward motion of the aircraft.

If you raise the nose just a little, the induced drag is small, and so increased thrust still causes increased speed.  But if you raise the nose a lot, then the induced drag can cancel out the thrust.  This is called getting _behind the power curve_.

<img src="https://i2.wp.com/aviationglossary.com/wp-content/uploads/2015/08/region-of-reversed-command.png?ssl=1" width="600" aligh="center">

The graph[1] shows an airplane in straight and level flight.  To the right, you can see that the speed and power have a positive relationship.  The more power, the more speed.  But to the left, in the region of reversed command, _it takes more and more power to go slower and slower_.  

In other words, the pilot has the nose so high that the thust vector is being defeated by the backwards pointing lift vector; and the plane is kind of _mushing_ through the air on raw power, barely making any headway.

>_Can you see where I'm going with this?_

Except during the final moments of landing, pilots don't usually operate their aircraft behind the power curve.  It's a bit dangerous back there.  The slower you go, the more power you need.  If you go slow enough, you'll max out your power and descend with the stall warning screaming in your ears.  So pilots stay in front of the power curve by watching their airspeed, and keeping it above the inflection point.  

So what does this have to do with software?  (As if you haven't already guessed.)

Too many software teams operate behind the power curve _all the time_.  Rotten code is _induced drag_.  These teams have created so much induced drag that it takes a huge effort to make any forward progress.  The team mushes forward at full power, barely making any headway.   Indeed, many teams have maxed out their power and are in a slow uncontrollable descent.

How does a pilot get out from behind the power curve?  By lowering the nose.  This brings the lift vector to vertical allowing the thust vector to dominate, and the plane screams off into the wild blue yonder.

How does a software team get out from behind the power curve?  By lowering their noses, cleaning up the messes, and reducing the induced drag.  With that drag gone, and all the power they have, the wild blue yonder is theirs to explore.

Wouldn't it be great if we could invent an airspeed indicator and a stall warning horn for software teams?  Oh, yeah, we did!  It's called the velocity chart.  Good Agile teams operate in front of the power curve, because the velocity chart allows them to see their speed, and keep it in front of the inflection point.  When the velocity starts going down, good agile teams increase their refactoring to eliminate the induced drag. 

Startup culture in the U.S. _believes_ in operating behind the power curve.  That's where they think they _want_ to be.  They are so focussed on fast progress, and so convinced that high quality means low speed, that they abandon discipline and principles for the sake of the goal. This is a tradgedy.  

They start out believing that power and speed are related without paying any attention to drag.  So they haul back on the yoke, put their noses into the sky, ram the throttle forward, and then burn fuel madly while going nowhere in a hurry.  They don't understand that when you make a mess, you induce drag, and you cancel out your power.

-----

[1] https://i2.wp.com/aviationglossary.com/wp-content/uploads/2015/08/region-of-reversed-command.png?ssl=1

 
