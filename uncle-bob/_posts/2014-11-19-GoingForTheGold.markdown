---
layout: post
title: "Thorns around the Gold"
tags: ["Craftsmanship"]
---
This article was inspired by [The IsNullOrWhiteSpace trap](http://blog.ploeh.dk/2014/11/18/the-isnullorwhitespace-trap/) by Mark Seemann (@ploeh).  Mark's article is well written and quite brief.  Please read it before continuing.

The trap that Mark points out is a special case of a much more general trap that I call _Grabbing for The Gold_.  I can demonstrate this trap by referring back to Mark's article.  

Notice that the first tests that Mark wrote were: 

    [InlineData("Seven Lions Polarized"  , "LIONS POLARIZED SEVEN"  )]
    [InlineData("seven lions polarized"  , "LIONS POLARIZED SEVEN"  )]
    [InlineData("Polarized seven lions"  , "LIONS POLARIZED SEVEN"  )]
    [InlineData("Au5 Crystal Mathematics", "AU5 CRYSTAL MATHEMATICS")]
    [InlineData("crystal mathematics au5", "AU5 CRYSTAL MATHEMATICS")]

He's already fallen into the trap.  Why?  Because he grabbed for _The Gold_.

### Gold and Thorns
The core functionality that Mark is trying to describe is one in which words are alphabetized.  So, naturally, his tests reflect that core functionality.  The core functionality is _The Gold_; and he grabbed for it.  

The problem is that _The Gold_ is protected by an invisible hedge of thorns that will entangle any unwitting programmer who, dazzled by _The Gold_, incautiously grabs for it.  What is that hedge of thorns?  In Mark's case it is the `null` and blank input cases.

I have been following the TDD discipline for fifteen years now.  In that time I've learned a lot about that invisible hedge of thorns.  I've learned that it's _always_ there.  I've learned that if you grab for _The Gold_ too early, that invisible hedge will thwart your progress and tear your efforts to ribbons[1].  So the strategy I've learned to follow is to keep my eyes averted from "The Gold", while probing for the hedge and clearing it away.  

### Probing and Clearing
Before I approach the core functionality, I write as many tests as I can that _ignore_ the core functionality and deal instead with exceptional, degenerate, and ancillary behaviors; in that order.  One by one, I write those tests, and then make them pass.  

 * Exceptional Behaviors

    These are behaviors that detect invalid inputs that the core functionality should never see.  These behaviors return error codes, log error messages, and/or throw exceptions.  

    In Mark's case handling the `null` input is the only exceptional behavior.  But in more complex applications detecting exceptional cases can be a lot more challenging.  Of course they include input validations.  But they also include semantic violations such as deleting records that don't exist; or adding records that already exist.

 * Degenerate Behaviors

    These are inputs that cause the core functionality to do "nothing".  I put "nothing" in quotes because sometimes "nothing" can be relatively complicated.  

   In Mark's case, the empty and blank strings are degenerate inputs.  He eventually solved the problem of blanks and empties with a complex set of conditions and operations that returned an empty string if a sole blank or empty string was the input, and eliminated all blanks in every other case[2].

    In general, degenerate conditions are things like blanks, empty strings, empty collections, zero length arrays, etc.  In more complicated applications, a degenerate input can be quite complicated and require a great deal of processing.  Consider, for example, a Java compiler processing a source file that contains thousands of lines containing nothing but semicolons and comments.  What should it's output be?

 * Ancillary Behaviors

    These are sometimes the hardest to find.  Ancillary behaviors are those that surround and support the core functionality; but are not part of it.  For example, the `getSize()` function of a `Stack`.  Reporting the size of a `Stack` is not related to its core LIFO functionality.  

   The thing about ancillary behaviors is that they frequently turn out to be useful to the core functionality in some in-obvious way.  For example, it turns out that the size of a `stack` is the array index used for `push` and `pop` operations in fixed-length stacks.  I often find that if all the ancillary behaviors are in place before I approach the core functionality, then the core functionality is much easier to implement. 

These are the first tests I write, and make pass.  I shy away from any tests that are close to the core functionality until I have completely surrounded the problem with passing tests that describe everything _but_ the core functionality.  Then, and only then, do I go get _The Gold_.

-------------
[1] Indeed just the day before yesterday I wasted four hours entangled in thorns that I had missed, and had not cleared away properly.  In the end `git reset --hard` was my only escape.

[2] Did he cover all the possible conditions?  What about tab, newline, backspace, and un-printables?