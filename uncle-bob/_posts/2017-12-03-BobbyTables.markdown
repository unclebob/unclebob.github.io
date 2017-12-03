---
layout: post
title: Bobby Tables
tags: ["Software"]
---
> _`SQL` is demon spawn, and no self-respecting software developer should ever use it._

OK, that's a little hyperbolic.  Demons did not create `SQL`.  Indeed, the folks who created it were filled with nothing but good intentions.  

But you know what they say about the road to hell. 

I want you to think about just what a supremely bad idea it is to use a textual data access language.  Such a language can pass through the user interface of a system and provide unauthorized access to all the data contained within.

Now, of course, we all know that we are supposed to scan all our inputs for potential `SQL` injection (`SQLi`) attacks.  And yet, hundreds of thousands, if not millions of users have had their data stolen by just this mechanism.  Why?  Because it is unreasonable to expect that every single user input of every single system is going to have the protections required.

The problem, of course, could have been eliminated at the source, decades ago.  Back in 1998 `rain.forest.puppy` described, in _Phrack_, how to slip `SQL` statements in through a user interface and execute them.  The instant that article was published _every single programmer in the world should have ceased to use `SQL` on the spot!_

I find it absolutely amazing that `SQL` is still used.  Did we learn nothing from Equifax, or Yahoo, or...  Well, I mean, it's been just about everybody hasn't it?  

And here we all are, comforting ourselves that our current frameworks will handle the issue.  Hibernate will handle the issue.  JPF will handle the issue.  Rails will handle the issue.  Poppycock!  Frameworks don't handle the issue; programmers do!  And programmers haven't been handling that issue all that well; have they?

Here's an idea:

>_**STOP USING SQL!**_

`SQL` is the ultimate security breech.  `SQL` is a portable, universal, textual language that can be transmitted through the user interface of a system and, if passed to the `SQL` engine, can provide absolute access and control to _all_ the data in the system.  

The very idea that `SQL` statements might come in through the user interface and be held in RAM ought to fill you with unmitigated _terror_!  All it takes is for some poor idiot programmer to fail to remember the exact arcane gestures that offer the appropriate protections.  

For example, can you tell which of these statements is vulnerable to a `SQLi` attack?  The language is Ruby, and the framework is Rails (circa 2015).  

    # Unsafe
    User.where("email = '#{email}'")
    User.where("email = '%{email}'" % { email: email })

    # Safe
    User.where(email: email)
    User.where("email = ?", email)
    User.where("email = :email", email: email)
    
Can you see the vulnerability?  Do you understand just what combinations of question marks, hash marks, parentheses, and percent signs makes a statement vulnerable?

Do you understand that these kinds of statements appear thousands of times in a typical application?  Do you realize that if even one such statement has the wrong combination of question marks and parentheses it opens the system to a `SQLi` attack?  Isn't it obvious that, so long as there is a SQL engine in the system, there is simply no reliable way to guarantee that such an attack can be prevented?

Oh, don't get me wrong.  A good clean architecture can absolutely prevent `SQLi` attacks.  If you put your `SQL` engine below an architectural boundary, and you make absolutely sure that all source code dependencies cross that boundary pointing away from the `SQL` engine.  And if you make absolutely certain that there is no `SQL` above that boundary.  And if you make absolutely certain that no text that crosses that boundary going towards the `SQL` engine has any `SQL` in it.  Then you will absolutely prevent `SQL` attacks.  

But there are too many "absolutelys" in that paragraph.  You never know when some 22 year old programmer, working at 3AM under a horrific schedule pressure, will forget to use just the right `?` and `#` in just the right positions.  

The solution.  The only solution.  Is to eliminate `SQL` from the system entirely.  If there is no `SQL` engine, then there can be no `SQLi` attacks.  

What would replace `SQL`?  An API of course!  And _NOT_ an API that uses a textual language.  Instead, an API that uses an appropriate set of data structures and function calls to access the necessary data.





