---
layout: post
title: A Little More Clojure
tags: ["Software"]
---
So let's learn just a little bit more of Clojure.

Here are a few common utility functions:  

	user=> (inc 1) ; increments argument
	2
	user=> (dec 3) ; decrements argument
	2
	user=> (empty? []) ; tests for empty
	true
	user=> (empty? [1 2])
	false

If you know Java or C# you probably know what the `map` function does.  Here's an example: `(map inc [1 2 3])` evaluates to `(2 3 4)`.<br>
The first argument of map is a function.  The second is a list.  The `map` function returns a new list by applying the function to every element of the input list.  

The `filter` function also takes a function and a list.  `(filter odd? [1 2 3 4 5])` evaluates to `(1 3 5)`.  From that I think you can tell what both the `filter` and the `odd?` functions do.  

And so with that, let's try a little challenge.  Let's find all the prime numbers between one and a thousand.

We'll use a variant of TDD to do this.  Our eyes will be the tests.  The cycle will be the same size as normal TDD; but we'll write a bit of code first and then test it.  

>_I know.  Blashphemy!  So sue me.  ;-)_

We begin like this: `(defn primes [n] )`  This returns `nil`.  

	user=> (primes 1000)
	nil

Now let's get all the numbers between 1 and `n`.

	(defn primes [n]
	  (range 1 (inc n)))

	user=> (primes 10)
	(1 2 3 4 5 6 7 8 9 10)

You've probably figured out what `range` does.  It just returns a list of all the integers between it's arguments.  

OK, so now all we have to do is filter all the primes:

	(defn primes [n]
	  (let [candidates (range 1 (inc n))]
	    (filter prime? candidates)))
	
	CompilerException java.lang.RuntimeException: Unable to resolve symbol: prime? in this context, compiling:(null:3:5)

Oh, oh.  We need to implement `prime?`

	(defn prime? [n])
	
	user=> (primes 10)
	()

OK, that makes sense.  But I should explain the `let` function.  It allows you to create names that are bound to expressions.  The names exist only within the parentheses of the `let` expression.  So it's a way to create local variables -- though the word "variable" is not quite right because they cannot be reassigned.  They are immutable.

Now how do we tell if a given integer `n` is prime?  Well, you all know how to do that, right?  The simple and naive way is to divide the integer by every number between 2 and `n`.  But, of course that's wasteful.  There's a better upper limit to try which is the square root of `n`.  I'm sure you can work out why that's true.  

	(defn prime? [n]
	  (let [sqrt (Math/sqrt n)]
	     sqrt))
	 
	user=> (prime? 100)
	10.0

OK, that's right.  Notice that we called the Java `Math.sqrt` function.  That's a good example of how Clojure can call down into the Java libraries).  Of course we don't want `prime?` to return a number; we want it to return a boolean.  But for now it's good to see the intermediate values of our computation.

So, next we'd like to get all the integers between 2 and the square root.  We already know how to do that.

	(defn prime? [n]
	  (let [sqrt (Math/sqrt n)
	        divisors (range 2 (inc sqrt))]
	     divisors))
	 
	 user=> (prime? 100)
	 (2 3 4 5 6 7 8 9 10)

Now which of the `divisors`	divide `n` evenly?  We can find out by using the `map` function.

	(defn prime? [n]
	  (let [sqrt (Math/sqrt n)
	        divisors (range 2 (inc sqrt))
	        remainders (map (fn [x] (rem n x)) divisors)]
	   remainders))
	 
	user=> (prime? 100)
	(0 1 0 0 4 2 4 1 0)
	
The `rem` function should be self-explanatory; it just returns the integer remainder of the division of `n` by `x`.  The `(fn [x]...)` business needs a little explanation.  Notice how similar it is to `defn f [x]`?  This is how we create an anonymous function.  If you know the syntax in Java or C# for anonymous functions, then this shouldn't be too much of a surprise to you.  Anyway, the remainders list is just the list of all the remainders that result from dividing `n` by the `divisors`.

Now some of those remainders were zero, and that means they divided `n` evenly.  Therefore `n` (100 in this case) is not prime.  Let's try a few others.

	user=> (prime? 17)
	(1 2 1 2)
	user=> (prime? 1001)
	(1 2 1 1 5 0 1 2 1 0 5 0 7 11 9 15 11 13 1 14 11 12 17 1 13 2 21 15 11 9 9)
	user=> (prime? 37)
	(1 1 1 2 1 2)

OK, so if the remainders list has a zero in it, then `n` is not prime.  Well, that should be easy, shouldn't it?

	(defn prime? [n]
	  (let [sqrt (Math/sqrt n)
	        divisors (range 2 (inc sqrt))
	        remainders (map (fn [x] (rem n x)) divisors)
	        zeroes (filter zero? remainders)]
	     zeroes))
	 
	user=> (prime? 100)
	(0 0 0 0)
	user=> (prime? 17)
	()	 

So now all we need to do is return `true` if the list is empty.  Right?

	(defn prime? [n]
	  (let [sqrt (Math/sqrt n)
	        divisors (range 2 (inc sqrt))
			remainders (map (fn [x] (rem n x)) divisors)
			zeroes (filter zero? remainders)]
	     (empty? zeroes)))

	user=> (prime? 100)
	false
	user=> (prime? 17)
	true
	user=> (primes 100)
	(1 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97)	

Now I want you to think carefully about how we solved this problem.  No `if` statements.  No `while` loops.  Instead we envisioned lists of data flowing through filters and mappers.  The solution was almost more of a fluid dynamics problem than a software problem.  (Ok, that's a stretch, but you get my meaning.)  Instead of imagining a procedural solution, we imagine a data-flow solution.

Think hard on this -- it is one of the keys to functional programming.  

(Special thanks to Stu Halloway @stuarthalloway for cluing me into the dataflow mindset way back in 2005)

Oh, and the primes between 1 and 1000?

	user=> (primes 1000)
	(1 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97 101 103 107 109 113 127 131 137 139 149 151 157 163 167 173 179 181 191 193 197 199 211 223 227 229 233 239 241 251 257 263 269 271 277 281 283 293 307 311 313 317 331 337 347 349 353 359 367 373 379 383 389 397 401 409 419 421 431 433 439 443 449 457 461 463 467 479 487 491 499 503 509 521 523 541 547 557 563 569 571 577 587 593 599 601 607 613 617 619 631 641 643 647 653 659 661 673 677 683 691 701 709 719 727 733 739 743 751 757 761 769 773 787 797 809 811 821 823 827 829 839 853 857 859 863 877 881 883 887 907 911 919 929 937 941 947 953 967 971 977 983 991 997)



		