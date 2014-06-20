---
layout: post
title: Where is the Foreman?
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="0; url=http://blog.8thlight.com/uncle-bob/2014/02/21/WhereIsTheForeman.html" />
The foreman on a construction site is the guy who is responsible for making sure all the workers do things right.  He's the guy with the tape-measure that goes around making sure all the walls are placed properly.  He's the guy who examines all the struts, joists, and beams to make sure they are installed correctly, and don't have any significant defects.  He's the guy who counts the screws in the flooring to make sure it won't squeak when you walk on it.  He's the guy -- the guy who takes responsibility -- the guy who makes sure everything is done _right_.

Where is the foreman on our software projects?  Where's the guy who makes sure all the tests are written.  Where's the guy who makes sure that all the exceptions are caught.  Where's the guy who makes sure all the errors are checked, and that references can't be null, and that variables are thread-safe?  Where's the guy who makes sure that the programmers are pairing enough, talking enough, planning enough? Where's the guy who keeps the floors from squeaking?

Without a good foreman, a construction site would fall apart into chaos.  The walls wouldn't line up. The doors would hang askew. The cold water would come out the hot faucet, and the hot out the cold.  Without a good foreman the basement and the roof would both leak, and the fireplace would spew smoke into the living room.  Without a good foreman the construction would be delivered very late, way over budget, and have abysmal quality.    

Without a foreman, the floors would squeak.  

What would the foreman do on software project?  He'd do the same thing he does on a construction project.  He'd make sure everything was done, done right, and done on time.  He'd be the only one with commit rights.  Everybody else would send him pull requests.  He'd review each request in turn and reject those that didn't have sufficient test coverage, or that had dirty code, or bad variable names, or functions that were too long.  He'd reject those that, in his opinion, did not meet the level of quality he demands for the project.

I imagine that many programmers recoil in horror from the idea that someone else would have the power to judge their code and reject their commits.  After all, how can you get done on time if the code has to be _right_?  How can you possibly meet your schedule if you have to write all those tests?   I mean, if there's a guy who's actually going to _look_ at the code, then there's no way to make yourself look good by saying that the code is done when it's not.  It'd be awful.

Awful or not, it's what most industries do.  If you want to get a project done, done right, and done on time, you need a foreman.  And that foreman has to be so technically astute that he can check the work of all the workers.  He has to have the authority to reject any work he considers sub-standard.  And he also has to have the power to say "No" to the unreasonable demands of the customers and managers.  

Where is the foreman on our software projects?  Where is the guy with the commit rights?  Where is the guy who makes sure all the tests are written, and all the concerns are separated, and all the right dependencies are inverted?  

Why don't we have this guy?  

Is it any wonder that our floors squeak?
