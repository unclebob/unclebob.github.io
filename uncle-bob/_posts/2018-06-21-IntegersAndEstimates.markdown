---
layout: post
title: Integers and Estimates
tags: ["Software"]
---
>_What is this: `a^2 + b^2 = c^2`_

The Pythagorean Theorem.

>_Right.  What else is it?_

An equation in three unknowns.

>_Do you know some solutions to this equation?_

Sure.  (3,4,5) and (5,12,13).

>_Right.  Those are common pythagorean triplets.  Do you know others?_

Well, Google is my friend, let's see.  (typing)  It looks like (7,24,25) and (9,40,41) all satisfy the equation.

>_Have you noticed that you've only supplied integer solutions?_

Oh, right.  I suppose that there are a whole load of non-integer solutions.

>_Have you heard of Diophantus?_

Is that a Greek name?

>_Yes.  Diophantus was interested in equations that had integral solutions.  We call such equations: Diophantine equations._

So `a^2 + b^2 = c^2` is a Diophantine equation?

>_Yes.  And there are many others.  For example: `a^3 + b^3 = c^3`_

Oh, sure.  And what are some solutions?

>_There aren't any._

Really?  None?

>_Yes.  None.  That has been proven.  In fact it has been proven that `a^n + b^n = c^n` has no integral solutions for `n>2`.  This is known as Fermat's conjecture._

Huh.  OK, well this is kinda interesting I guess, but why should I care?

>_What is a digital computer?_

What do you mean?  This thing that you and I are conversing on is a digital computer.

>_Yes, but what does a digital computer do?_

Uh. It computes digitally?

>_Precisely!  And the word digitally means...?_

Um.  With digits?

>_Exactly!  And are the number of digits finite?_

Of course, though very very large nowadays.

>_...and a finite number of digits is...?_

Oh, I think I see where you are going.  A finite number of digits is an integer.

>_Right.  A digital computer is a computer that computes with integers.  Nothing but integers._

Well, wait.  What about floating point numbers and rational numbers?

>_They are represented by integers in the computer.  The computer deals with integers, only integers._

OK.  sure.  Integers.  But what does this have to do with Diophantine equations?

>_What are the inputs to a computer program?_

There are lots of kinds.  Keyboard characters, mouse movements, mouse clicks, network packets.  You name it.

>_They are all made up of integers aren't they?_

Um.  Yeah, I guess they are.  OK, so every input to a computer program is integral.  

>_And what about the outputs?_

Well, yes, pixels, characters, network packets.  They are all composed of integers too.

>_So a digital computer program takes in integers and returns integers._

Right.  That's right.  It's all integers.

>_A digital computer program, therefore, represents a Diophantine equation._

Wait. What?  

>_Integers in.  Integers out._

OK. sure.  But it's one big complicated Diophantine equation.

>_Actually, the specification of the program is the equation.  The program finds the solutions to that equation._

Yeah, yeah, ok.  That's right.  The specification of a program is a great big Diophantine equation in a bazillion unknowns, and the program that meets that specification finds solutions to that ginormous equation.  Is this useful to know?

>_Who is David Hilbert?_

You mean that guy who designed that funny recursive curve that looks like mosquito netting?

>_(Ahem.) That was one of his accomplishments yes. He also helped Einstein with the General Theory of Relativity.  He was a very great mathematician._

And he did something with Diophantine equations I'm guessing.

>_Indeed he did many, many things.  Among them was a very famous question.  The question of "Entscheidung" -- decidability._

What did he want to decide?

>_Remember Fermat's conjecture?_

You mean that equation that has no solutions.  `a^n + b^n = c^n` where `n>2`?

>_Yes, that's the one.  For a long time there was no proof that `n=2` was the only solution.  How could you disprove that conjecture if you thought it was untrue?_

I could write a program to find counter examples.  Like, maybe `n=999,999,999` might work.  

>_Right.  And if you found such a solution, you'd have disproven Fermat's conjecture.  But how long would it take to PROVE the conjecture using that method?_

The program would run forever.  I couldn't prove it that way.

>_Correct.  What Hilbert wanted was a finite algorithm to determine whether or not a solution exists.  He wanted a way to "decide" whether or not a search, such as the one you suggested, was practical._

Wait, wait.  What?  He wasn't asking for the solutions, he was asking for a way to know if there were any solutions?

>_Right.  He wanted a finite algorithm that could tell him whether a given diophantine equation had solutions or not.  That algorithm would not supply the solutions; it would just supply the decision._

That's why he called it "decidability"?

>_Entscheidung.  Yes_

Gesundheight!

>_Harumph!  Now.  Who do you think solved the problem of decidability?_

I think you're about to tell me.

>_Two people whom you've heard of.  The two great founders of modern computer science.  Alonzo Church, and Alan Turing._

Church!  That's the guy who invented functional programming, right?

>_In a manner of speaking, yes._

And Turing!  He won world war 2 right?

>_He certainly contributed.  The two of them proved, using very different techniques, that there was no general and finite solution to decidability._

That must have disappointed Hilbert. 

>_Perhaps.  But that's not the issue._

Yeah, just what is the issue here?

>_When you are given a program specification, i.e. a Diophantine equation, what is the first thing you are asked to do?_

Estimate it of course.  Folks want to know how long it will take to write the program.

>_And the program is what again, in terms of a that Diophantine equation?_

The program is the ... solution ... to the ... OH!

>_(smile)  I perceive you've gotten the point._

Yeah, like, they are asking me to DECIDE.  An estimate is a decision.

>_And is there a finite method for finding that decision in every case?_

No!  OH, that's hilarious.

>_Right.  The founding documents of computer science are documents that prove that there is no finite mechanism for deciding if a program can even be written.  The founding of computer science was based on the proof that estimates were not guaranteed._

Yeah, but we CAN estimate.

>_Yes, we can.  That's because most specifications are estimable._

So this has just been a cute little mathematical diversion with no pragmatic result.

>_I suppose you could say that.  But I enjoyed it.  And, after all, I think it's deliciously ironic that it was the proof of NOESTIMATES that founded computer science._
