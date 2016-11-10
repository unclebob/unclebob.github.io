---
layout: post
title: Three Paradigms
tags: ["Tools"]
old_tags: ["Functional Programming"]
---

<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2012/12/19/Three-Paradigms.html" />
In the last 40 years computer hardware technology has increased the computing power of our machines by well over twenty orders of magnitude. We now play Angry Birds on our phones, which have the computing power of the freon cooled supercomputer monsters of the 70s.

But in that same 40 years software technology has barely changed at all. After all, we still write the same if statements, while loops, and assignment statements we did back in the '60s. If I took a programmer from 1960 and brought him forward through time to sit at my laptop and write code; he'd need 24 hours to recover from the shock; but then he'll be able to write the code. The concepts haven't changed that much.

But three things have changed about the act of writing software. I'm not talking about the hardware now, or the computer speed, or the incredible tools we have. I'm talking about the code itself. Three things have changed about that code. We could call these things -- paradigms. And they were all "discovered" in a single decade more than 40 years ago.

\* 1968 -- *Structured Programming*. Edsger Dijkstra wrote his classic paper: "Go To Statement Considered Harmful" and a number of other papers and articles suggesting that we abandon the use of unbridled Go To, replacing it with structures such as if/then/else and while loops.

\* 1966 - *Object Oriented Programming*. Ole-Johan Dahl and Kristen Nygaard, fiddling around with the Algol language, "discover" objects, and create the first Object Oriented Language: Simula-67. Though there are many far-reaching implications of this advance it did not add any new capabilities to our code. Indeed, it removed one. For with the advent of polymorphism, the need for pointers to functions was eliminated; and indeed deprecated.

\* 1957 - *Functional Programming*. John McCarthy creates Lisp: the first functional language. Lisp was based on the Lambda Calculus formulated by Alonzo Church in the '30s. Though there are many far-reaching implications of functional programming, all functional programs are dominated by one huge constraint. They don't use assignment.

Three paradigms. Three constraints. Structured Programming imposes discipline on direct transfer of control. Object Oriented Programming imposes discipline on indirect transfer of control. Functional programming imposes discipline upon assignment. Each of these paradigms took something away. None of them added any new capability. Each increased discipline and decreased capability.

Can we afford another paradigm? Is there anything left to take away?

There hasn't been a new paradigm in 40 years; so perhaps that's a good indication that there aren't any more to find.

Must we use all these paradigms, or can we pick and choose?

Over time we have decided to enforce them. First structured programming was enforced by effectively eliminating the Go To statement from our languages (as Dijkstra recommended in his paper). OO has also been effectively enforced by removing pointers to functions from our most modern languages and replacing that functionality with polymorphism (e.g. Java, C\#, Ruby). So for at least these two, the answer to that question seems to be that we MUST use them. All other options have been eliminated; or at least severely constrained.

So what about functional programming? Are we to be consigned to using languages that have no assignment operator? Likely so! We are already consigned to writing code that must run well on multi-cores; and those cores are multiplying like Gerbils. My laptop has 4 cores. My next will likely have 8. The one after that 16. How are you going to write reliable code with 4096 processors contending for the bus? We can barely get two concurrent threads to work properly, let alone 2^n literal processors.

Why is functional programming important to solving the multi-core problem? Because functional programs don't use assignment, and therefore don't have side effects, and therefore don't have concurrent update problems -- at least that's the theory.

We'll talk more about the details of functional programming in later blogs. What fascinates me about the three paradigms mentioned above are their dates. They are *old*; almost older than I am. And there have been no new ones since I turned 16, 42 years ago.
