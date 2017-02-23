---
layout: post
title: Necessary Comments
tags: ["Software"]
---
It is well known that I prefer code that has few comments.  I code by the principle that good code does not require many comments.  Indeed, I have often suggested that every comment represents a failure to make the code self explanatory.  I have advised programmers to consider comments as a last resort.

I recently came across that last resort.  

I was working with a member of our `cleancoders.com` team, on a particular issue with our site.  We were trying to implement a feature that would inform a customer who bought one of our videos, what other videos they might want to purchase as well.  The feature would find all the videos that had been purchased by others who had bought that video, and would pick the most popular and recommend it to the customer.

The algorithm to do that can take several seconds to run; and we didn't want the customer to have to wait.  So we decided to cache the result and run the function no more often than every N minutes.  

Our strategy was to wrap the long running function in _another_ function that would return the previous result from the cache and if more than the N minute had passed, would run the algorithm in a separate thread, and cache the new result.  We called this "choking".

The long running function was called the "Chokeable Function", and the wrapping function was called the "Choked Function".  The Choked Function had the same signature and return value as the Chokeable function; but implemented our choking behavior.

Trying to explain this in code is very difficult.  So we wrote the following comment at the start of the choking module:

	; A Choked function is a way to throttle the execution of a long running
	; algorithm -- a so-called "Chokeable Function".  The Choked function
	; returns the last result from the Chokeable Function; and only allows
	; the Chokeable function to be called if more than the Choke-time has 
	; elapsed since its last invocation.
	
Now imagine the unit tests!  At first, as we were writing them, we tried to name those tests with nice long names.  But the names kept getting longer and more obscure.  Moreover, as we tried to explain the tests to each other, we found that we needed to fall back on diagrams and pictures.  

In the end we drew a timing diagram that showed all the conditions that we'd have to deal with in the unit tests. 

We realized that nobody else would understand our tests unless they could see that timing diagram as well.  So we drew that timing diagram, along with explanatory text, into the comments in the test module.  

	; Below is the timing diagram for how Choked Functions are tested.
	; (See the function-choke module for a description of Choked Functions.)
	;
	; The first line of the timing diagram represents calls to the choked
	; function.  Each such call is also the subject of a unit test; so they
	; are identified by a test#
	;
	; In this test, the chokeable function counts the number of times it has
	; been invoked, and the choked function returns that counter.  The expected
	; return value is the below the first horizontal line in the timing
	; diagram.

	; 1  2  3   3.5  4  5    6  Test#
	;    aaaaa
	;---------------------------
	; n  n  1    1   1  1    2
	;---------------------------------------------
	;AAAAA           AAAAA
	;     CCCCCCC         CCCCCCC
	;
	; Where: A is the algorithm time. (The chokeable function run time)
	;        C is the choke time
	;        n is nil
	;        1 is "result-1"
	;        2 is "result-2"
	
The names of the tests then became, simply, `test1` and `test2`, up to `test6`; referring back to the diagram.  

We were pretty pleased with the result; both the code and the comments. 

So, this is one of those cases where there was no good way for the code to be self documenting.  Had we left the comments out of these modules, we would have lost the intent and the rationale behind what we did.  

This doesn't happen all the time.  Indeed, I have found this kind of thing to be relatively rare.  But it _does_ happen; and when it does nothing can be more helpful than a well written, well thought through, comment.




