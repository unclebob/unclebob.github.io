---
layout: post
title: REPL Driven Design
tags: ["Software"]
---
If you follow me on facebook you know that I've been publishing daily CoronaVirus statistics. I generate these statistics using the daily updates in the Johns Hopkins github [repository](https://github.com/CSSEGISandData/COVID-19).  

At first I just hand copied the data into a spreadsheet.  But that became tedious quite rapidly.

Then, in late March, I wrote a little Clojure program to extract and process the data.  Every morning I pull the repo, and then run my little program.  It reads the files, does the math, and prints the results.  

Of course I used TDD to write this little program.

But over the last several weeks I've made quite a few small modifications to the program; and it has grown substantially.  In making these adaptations I chose to use a different discipline:  REPL Driven Design.  

REPL Driven Design is quite popular in Clojure circles.  It's also quite seductive.  The idea is that you try some experiments in the REPL to make sure you've got the right ideas.  Then you write a function in your code using those idea.  Finally, you test that function by invoking it at the REPL.  

It turns out that this is a very satisfying way to work.  The cycle time -- the time between a code experiment and the test at the REPL -- is nearly as small as TDD.  This breeds lots of confidence in the solution.  It also seems to save the time needed to mock, and create fake data because, at least in my case, I could use real production data in my REPL tests.  So, overall, it felt like I was moving faster than I would have with TDD.

But then, in late April, I wanted to do something a little more complicated than usual.  It required a design change to my basic structure.  And suddenly I found myself full of fear.  I had no way to ensure that those design changes wouldn't leave the system broken in some way.  If I made those changes, I'd have to examine every output to make sure that none of them had broken. So I postponed the change until I could muster the courage, and set aside the dedicated time it would require.

The change was not too painful.  Clojure is an easy language to work with.  But the verfication was not trivial, which led me to deploy the program with a small bug -- a bug I caught 4 days later.  That bug forced me to go back and correct the data and graphs that I generated.

Why did I need the design change?  Because I was not mocking and creating fake data.  My functions just read from the repo files directly.  There was no way to pass them fake data.  The design change I needed to make was precisely the same as the design change that I'd have needed for mocking and fake data.

Had I stuck with the TDD discipline I would have automatically made that design change, and I would not have faced the fear, the delay, and the error.  

Is it ironic that the very design change that TDD would have forced upon me was the design change I eventually needed?  Not at all. The decoupling that TDD forces upon us in order to pass isolated inputs and gather isolated outputs is almost always the design that fascilitates flexibility and promotes change.  

So I've learned my lesson.  REPL driven development feels easier and faster than TDD; but it is not.  Next time, it's back to TDD for me.  
		