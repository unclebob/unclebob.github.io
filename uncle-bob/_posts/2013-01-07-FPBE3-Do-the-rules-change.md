---
layout: post
title: FP Basics E3
tags: ["Tools"]
old_tags: ["Functional Programming"]
---

h4. Do all the Rules Change?

Whenever we start to use a new paradigm, we confront the problem of our old rules and our old habits. We ask ourselves whether all those rules and habits are valid in the new paradigm. Consider, for example Test Driven Development. Is it valid in Functional Programming? If so, how do you do it?

The best way to figure this out it to try it; so lets try writing a simple functional program: `Word Wrap`. The requirements are simple. Given a string of words separated by single spaces, and given a desired line length, insert line-end characters into the string at appropriate points to ensure that no line is longer than the length. Words may be broken apart only if they are longer than the specified length.

So, if we take the Gettysburg Address as a string:

`“Four score and seven years ago our fathers brought forth upon this continent a new nation conceived in liberty and dedicated to the proposition that all men are created equal”`

And we wrap it so that no line is longer than 10 characters, the result should be:

`“Four score\nand seven\nyears ago\nour\nfathers\nbrought\nforth upon\nthis\ncontinent\na new\nnation\nconceived\nin liberty\nand\ndedicated\nto the\npropositio\nn that all\nmen are\ncreated\nequal”`

We’ll use Brian Marick’s lovely test framework: `Midje`, and begin by specifying the first test.

    (facts
      (wrap "" 1) => "")

This just says that it is a fact that the call `(wrap "" 1)` will return `""`. We can make this pass by defining the `wrap` function and giving it a degenerate implementation as usual:

    (defn wrap [s n]
      "")

The next test forces us away from the degenerate implementation, but not by much:

    (wrap "x" 1) => "x"

    (defn wrap [s n]
      s)

The next test forces us to split a word that’s too long:

    (wrap "xx" 1) => "x\nx"

    (defn wrap [s n]
      (if (<= (count s) n)
        s
        (str (subs s 0 n) "\n" (subs s n))))

The `if` statement in clojure is like the `?:` operator in `C` and `java`. It returns the first expression if the predicate is true; otherwise it returns the second expression. The `subs` method should be self-explanatory, it’s similar to the `subString` method in `Java`. The `str` method simply takes all it’s arguments and concatenates them into a single string

The next test forces us to iterate, which we do by recursing:

    (wrap "xxx" 1) => "x\nx\nx"

    (defn wrap [s n]
      (if (<= (count s) n)
        s
        (str (subs s 0 n) "\n" (wrap (subs s n) n))))

The next test checks the case where the character after the line break is a space. We don’t want a space at the front of the next line, so we skip over that space.

    (wrap "x x" 1) => "x\nx"

    (defn wrap [s n]
      (if (<= (count s) n)
        s
        (let [trailing-space (= \space (get s n))
              new-line-start (if trailing-space (inc n) n)
              head (subs s 0 n)
              tail (subs s new-line-start)]
          (str head "\n" (wrap tail n)))))

The `let` clause in clojure simply allows us to create local symbols that hold values that will be used in the body of the function. These are not variables, because their values do not vary.

The `get` function returns the character at position `n` within the string. The `\space`, is a character constant in clojure, representing a space.

The next test forces us to search backwards to find the last space before we have to break the line. If no such space is found, we have a word longer than `n`. Otherwise we split the line at that space.   That backwards search is done by the `.lastIndexOf` function which is a direct call into the `Java` string library. It’s the `.` that indicates the direct `java` call.

    (wrap "x x" 2) => "x\nx"

    (defn wrap [s n]
      (if (<= (count s) n)
        s
        (let [space-before-end (.lastIndexOf s " " n)
              this-line-end (if (neg? space-before-end) 
                                n 
                                space-before-end)
              trailing-space (= \space (get s this-line-end))
              new-line-start (if trailing-space 
                                 (inc this-line-end) 
                                 this-line-end)
              head (subs s 0 this-line-end)
              tail (subs s new-line-start)]
          (str head "\n" (wrap tail n)))))

This has gotten a little ugly, so let’s refactor it a bit.

    (defn find-start-and-end [s n]
      (let [space-before-end (.lastIndexOf s " " n)
            line-end (if (neg? space-before-end) n space-before-end)
            trailing-space (= \space (get s line-end))
            line-start (if trailing-space (inc line-end) line-end)]
        [line-start line-end]))

    (defn wrap [s n]
      (if (<= (count s) n)
        s
        (let [[start end] (find-start-and-end s n)
              head (subs s 0 end)
              tail (subs s start)]
          (str head "\n" (wrap tail n)))))

You might not be convinced that those are all the test cases. If so, I encourage you to hunt for a new test case that will fail. Indeed, I encourage you to study those six simple test cases very carefully. Most people wouldn’t write them that way. It took me a lot of time with TDD before I realized the value of well-ordered, carefully considered, extremely minimal test cases.

Anyway, it would appear that, at least in this case, the rules of TDD apply just as much to functional programming as they do to any other kind of programming. So, as you start to explore functional programming, don’t throw out the baby with the bathwater. *Keep your TDD discipline!*

Some folks out there might complain that this algorithm isn’t functional; but I assure you that it is. There are no side effects. It is representationally transparent. It uses persistent data structures. It’s functional.

It does, however, lack one of the traits that many people associate with functional programs. It is not composed of a series of transformations.

Consider the squares of integers program again:

`(take 25 (squares-of (integers))`

The sheer elegance of this code comes partially from the fact that it is a series of transformations. The first transformation is the `integers` function. It transforms nothing into a lazy sequence of integers starting at 1. The second transformation is the `squares-of` function. It transforms any list of integers into a list of their squares. The final transformation is the `take` function, which transforms a lazy sequence of indefinite size into a sequence of exactly 25 elements.

Can we rewrite the word wrap problem as a series of transformations? Sure. Consider this:

    (defn wrap [s n]
      (join "\n" 
            (make-lines-up-to n 
                              (break-long-words n (split s #" ")))))

We’ll read this the way we read the squares of integers program; from the inside out. First we split the input string into a sequence of words. Then we break any words longer than `n` into two or more words. Then we append those words to lines that are no longer than `n`. Finally we join those lines together with `‘\n’`. Simple eh?

Well, it looks simple when it’s written like that. But here’s the implementation of the whole program:

    (defn break-long-words [n words]
      (if (empty? words)
        []
        (let [word (first words)]
          (if (>= n (count word))
            (cons word (break-long-words n (rest words)))
            (cons (subs word 0 n) 
                  (break-long-words 
                     n 
                     (cons (subs word n) (rest words))))))))

    (defn make-lines-up-to
      ([n words]
        (make-lines-up-to n words [] []))
      ([n words line lines]
        (if (empty? words)
          (conj lines (join " " line))
          (let [new-line (conj line (first words))]
            (if (>= n (count (join " " new-line)))
              (make-lines-up-to n (rest words) new-line lines)
              (make-lines-up-to n 
                                words 
                                [] 
                                (conj lines (join " " line))))))))

    (defn wrap [s n]
      (join "\n" (make-lines-up-to 
                    n 
                    (break-long-words n (split s #" ")))))

There are plenty of folks out there who will eagerly tell you that I’m not the world’s best functional programmer. They are likely correct. And so this program may be woefully inadequate as a demonstration. Still, it’s hard for me to believe that my first solution to the word wrap is inferior to the above code.   This doesn’t mean that sequences of transformations are bad. On the contrary, I think they are very powerful, and we’ll be studying them in much more detail later. My point is that not all functional programs need to be sequences of transformations. The old rule of “simpler is better” still applies. And for the word wrap algorithm, at least as far as I can see (which is admittedly not very far) a transformational solution is not best.

By the way, I *did not* use TDD to create the transformational solution you see above. I suppose I could have; but instead I used the method that many functional programmers prefer. I thought about it for a long time, and then I used manual testing at the console (often called the `REPL` for: Read Evaluate Print Loop) to piece the whole thing together. The entire process took at least three times longer than the TDD approach, involved a *lot* of nasty debugging and print statements, and caused many infinite recursive loops that blew the stack and crashed my console. In short, it was a pain. I don’t recommend it.  

So, to wrap things up: the old rules still apply. Functional programming may be a change - an important change; but it doesn’t change *everything*. Programming is still programming. Discipline is still discipline. And TDD works just as well in functional programming as in any other style of programming. There may be some rules that need to be discarded when you adopt functional programming; but so far I haven’t found any.
