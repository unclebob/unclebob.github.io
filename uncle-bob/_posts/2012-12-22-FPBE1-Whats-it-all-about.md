---
layout: post
title: FP Basics E1
tags: ["Tools"]
old_tags: ["Functional Programming"]
---

<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2012/12/22/FPBE1-Whats-it-all-about.html" />
h4. What's Functional Programming all about?

By now you've almost certainly heard of functional programming. I mean, how could you miss it? Everybody's talking about it. There are all these new functional languages coming out like Scala, F\# and Clojure. People are talking about older languages too, like Erlang, Haskell, ML, and others.

So what's this all about? Why is functional programming The Next Big Thing ™? And what in blazes is it?

Firstly, it’s almost certainly true that functional programming is the next big thing. There are good solid reasons for this that we'll explore towards the end of this article. But ﬁrst, in order to understand those reasons, we need to know what functional programming is. I'm going to upset a lot of people with this next statement because I'm going to resort to extreme minimalism. I'm going to reduce functional programming down to it's simplest core; and this isn't really fair because the topic is rich and expressive and full of wonderful concepts. We'll explore all those concepts in future articles. You'll even see hints of some of them here. But for now, I'll simply deﬁne functional programming this way:

> Functional programming is programming without assignment statements.

Oh no! Now I've gone and done it. The functional programmers out there are gathering their pitchforks and torches. They want my head for uttering such minimalist blasphemy. Meanwhile all the folks who hoped to learn what functional programming really is are about to stop reading because the above statement is so blatantly absurd. I mean: how in the world can you program without assignment? The best way to explain that is to show an example. Let's look at a very simple program in Java: The squares of integers.

    public class Squint {
      public  static  void  main(String  args[])  {
        for  (int  i=1;  i<=25;  i++)
          System.out.println(i*i);}
    }

Who hasn't written that program, or some simple variant of it? I must have written it many hundreds of times. It's often the second program I write in a new language, and the second or third program I teach new programmers to write. Everybody knows the good old squares of integers!

But let's look at it closely. It's just a simple loop with variable named `i` that counts up from 1 to 25. Each loop through the program causes the variable `i` to take on a new value. This is assignment. A new value is being assigned to the variable `i` every pass through the loop. If you could somehow peer into the memory of the computer and stare at the memory location that held the value of `i`, you would see the value held by that memory change from one iteration of the loop to the next.

If the previous paragraph seemed to have belabored an obvious point, let me point out that whole papers have been written on this topic. The concepts of identity, value, and state may seem intuitive to us; but they are actually a very rich topic in and of themselves. But I digress.

Now let's look at a functional program for the squares of integers. We'll use the language Clojure for this; though the concepts we're going to explore work the same in any functional language.

    (take 25 (squares-of (integers)))

Yes, you are reading that correctly; and yes, that is an actual program that produces actual results. If you must see the results they are:

    (1 4 9 16 25 36 49 64 ... 576 625)

There are three words in that program: `take`, `squares-of`, and `integers`. Each of those words refers to a function. The left parens in front of those words simply mean: *call the following function and treat everything up to the closing right paren as it's arguments.*

The `take` function takes two arguments, an integer `n`, and a list `l`. It returns the ﬁrst `n` items of `l`. The `squares-of` function takes a list of integers as it's argument and returns a list of the squares of those integers. The `integers` function returns a list of integers in sequential order, starting at 1. Thats it. The program simply takes the ﬁrst 25 elements of a list of the squares of sequential integers starting at 1.

Read that sentence again; because I did something important there. I took the three separate deﬁnitions of the functions and combined them into a single sentence. That's called: (are you ready for the buzzword?)

> **Referential Transparency.**

*\[cue: Fanfare, glitter, ticker tape, cheering crowds\].*

Referential Transparency simply means that, in any given sentence, you can replace the words in that sentence with their deﬁnitions, and not change the meaning of the sentence. Or, more importantly for our purposes, it means that you can replace any function call with the value it returns. Let's see this in action.

The function call `(integers)` returns `(1 2 3 4 5 6 ...)`. OK, I know you’ve got questions about this, right? I mean, how big is this list? The real answer is that the list is only as big as I need it to be; but let’s not think about that just now. We’ll come back to it in a later article. For the moment just accept that `(integers)` returns `(1 2 3 4 5 6 ...)`; because it does!

Now, in our program, we can replace the function call `(integers)` with it’s value. So the program simply becomes:

    (take 25 (squares-of (1 2 3 4 5 6 ...)))

Yes, I did that with copy and paste; and that’s also an important point. Referential Transparency is the same as copying the value of a function call and pasting it right on top of that function call.

Now, let’s do the next step. The function call: `(squares-of (1 2 3 4 5 6 ...))` simply returns a list of the squares of the numbers in it’s argument list. So it returns: `(1 4 9 16 25 36 49 64 ...)`. If we replace this function call with it’s value, our program simply becomes:

    (take 25 (1 4 9 16 25 36 49 64 ...))

And, of course, the value of that function call is simply:

    (1 4 9 16 25 36 49 64 ... 576 625)

Now let’s look at our program again:

    (take 25 (squares-of (integers)))

Notice that it has no variables. Indeed, it has nothing more than three functions and one constant. Try writing the squares of integers in Java without using a variable. Oh, there’s probably a way to do it, but it certainly isn’t natural, and it wouldn’t read as nicely as my program above.

More importantly, if you could peer into the computer’s memory and look at the memory locations used by my program, you’d find that those locations would be initialized as the program first used them; but then they would retain their values, unchanged, throughout the rest of the execution of the program. In other words, no new values would be assigned to those locations.

Indeed, this is a necessary condition for Referential Transparency, which depends on the fact that every time you invoke a particular function call, you will get the same result. The fact that computer memory for my program does not change while my program is executing means that the call `(f 1)` will always return the same value no matter how many times it is called. And that means I can replace `(f 1)`, wherever it appears, with its value.

Or to say this another way: Referential Transparency means that no function can have a side effect. And, of course, that means that no variable, once initialized, can ever change its value; since assignment is the quintessential side effect.

So why is this important? What’s so great about Referential Transparency? Given that it is possible to write programs without assignment, why is it important?

You are almost certainly reading this on the screen of your computer. Or if not; you have a computer nearby. How many cores does it have?I’m typing this article on a MacBook Pro with 4 real cores. (They say it has 8, but I don’t count all that “hyperthreading nonsense”. It has four). My previous laptop had two cores. And the one before that had just one. The only conclusion I can draw is that my next laptop will truly have eight cores; and the one after that will likely have 16.

The poor hardware engineers, who have carried us on their backs for the last four decades, have finally hit the speed of light limit. Computer clocks simply aren’t going to get much faster. After doubling every 18 months for longer than most programmers (except me) have lived, the runaway growth in computer speed has slammed to a halt; never to rise again.

So those hardware engineers, in an effort to give us more and more cycles per second, have resorted to adding more processors into our chips; and there seems no end to how many processors that will lead to as the years march onwards.

So let me ask you this, O skilled and competent programmer: How are you going to take advantage of every computer cycle available to you when your computer has 4096 cores in it? How will you marshal your function executions when they must run within 16384 processors all contending for the same memory bus? How will you build responsive and flexible websites when your models, views, and controllers must share 65536 processors?

Honestly, we programmers can barely get two Java threads to cooperate. And threads are baby-food compared to the meat and potatoes of real processors fighting over the bus. For over half a century programmers have made the observation that the processes running in a computer are concurrent, not simultaneous. Well, boys and girls, welcome to the wonderful world of simultaneity! Now; how are you going to deal with it?

And the answer to that is, simply: *Abandon all assignment, ye who enter here.*

Clearly, if the value of a memory location, once initialized, does not change during the course of a program execution, then there’s nothing for the 131072 processors to compete over. You don’t need semaphores if you don’t have side effects! You can’t have concurrent update (pardon me: Simultaneous Update) problems if you don’t update!

So that’s the big deal about functional languages; and it is one big fricking deal. There is a freight train barreling down the tracks towards us, with multi-core emblazoned on it; and you’d better be ready by the time it gets here.
