---
layout: post
title: Types and Tests
tags: ["Software"]
---
Mark Seeman (@ploeh) and I have been having a fun debate on twitter.  It began with this tweet from me, which is part of a much longer tweet thread:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">There is no escape from this. Whether you use static, or dynamic typing, you must still demonstrate correctness by executing tests. Static typing does not reduce that number of tests, because those tests are behavioral and empirical.</p>&mdash; Uncle Bob Martin (@unclebobmartin) <a href="https://twitter.com/unclebobmartin/status/1135894376222265345?ref_src=twsrc%5Etfw">June 4, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

As is usual, on social networks, many people posted rude and/or insulting comments.  Those people aren't important.  Mark, on the other hand, came back with respectful and substantive replies which you can see by following the whole thread.  It began this way:

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">I respectfully disagree with this. Some type systems allow null references. In those type systems, you must write tests that demonstrate how the SUT interacts with null input.<br><br>In other type systems (e.g. Haskell) nulls don&#39;t exist. You can&#39;t meaningfully write an equivalent test</p>&mdash; Mark Seemann (@ploeh) <a href="https://twitter.com/ploeh/status/1135896304561901570?ref_src=twsrc%5Etfw">June 4, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

A spirited debate ensued.  You may find it entertaining and educational to follow all the various threads.  However, I want to focus on one of Mark's replies.  He tweeted about a [blog](https://blog.ploeh.dk/2018/07/09/typing-and-testing-problem-23/) he had written back in 2018.  I encourage you to read it.  You'll learn something about testing, static typing, and Haskell.  You'll also learn something about how to debate with someone in the future by posting your refutation in the past. `;-)`

The problem Mark proposed in his blog is a simple function: `rndselect(n,list)`.  It returns a list of `n` elements randomly selected from the input list.  He walked through his strategy for developing this in Haskell, using types, `QuickCheck`, and after-the-fact tests.  

This piqued my interest.  How would the process, and the end result, be different using a dynamically typed functional language like Clojure, and strict TDD disciplines.

Let's find out.

We begin with the degenerate tests.  We return an empty list if the input list is empty, or if the number of requested elements is zero.

	(deftest random-element-selection
	  (testing "degenerate case"
       (is (= [] (random-elements 0 [])))
       (is (= [] (random-elements 0 [1])))
       (is (= [] (random-elements 1 [])))
	   ))
	
	(defn random-elements [n xs]
	  [])
	  
Early in Mark's blog he noted that a consequence of [The Free Theorem](https://ttic.uchicago.edu/~dreyer/course/papers/wadler.pdf) is that he would not have to test that the elements in the result list came from the elements in the source list.  On the other hand, since I'm not using a static type system my tests make no sense if I don't assume that the elements in the result set come from the source list. 
	  
There is one trivial case.  Let's pull one element out of a list that has one element.

    (testing "trivial cases"
      (is (= [1] (random-elements 1 [1]))))
	
	(defn random-elements [n xs]
	  (if (or (< n 1) (empty? xs))
	    []
	    [(first xs)]))
		
Note that I am following the rule of gradually increasing complexity.  Rather than worrying about the whole problem of random selection, I'm first focusing on tests that describe _the periphery_ of the problem.  

We call this: _Don't go for the Gold"_.  Gradually increase the complexity of your tests by staying away from the center of the algorithm for as long as possible.  Deal with the degenerate, trivial, and simple administrative tasks first.  

In that spirit, the next least complex thing to worry about is repetition.  So let's test that we can pull more than one element out of a single element list.

	(testing "repetitive case"
	    (is (= [1 1] (random-elements 2 [1]))))
		
	(defn generate-indices [n]
	  (repeat n 0))

	(defn random-elements [n xs]
	  (if (or (< n 1) (empty? xs))
	    []
	    (map #(nth xs %) (generate-indices n))))

Some of you might complain that I got a bit ahead of myself there.  I used that `nth` call instead of keeping the `first` call. Why did I change it if I didn't have a test for that?  _Shrug._

The next least complex thing to worry about are those indices.  Right now they are all just zero.  It would be good to make sure that indices other than zero work properly too. Testing this forces me to mock the function that generates the indices; and that forces me to change the design a bit.  So I'll break the `generate-indices` function up so that I can mock out a single index.  

The strange `with-bindings` call below temporarily replaces the implementation of the `index` function, such that it always returns the the index `1`.  The odd `^:dynamic` attribute is necessary, in Clojure, if you want to mock out (re-bind) a function.

    (testing "singular random case"
      (with-bindings {#'index (fn [] 1)}
        (is (= [2] (random-elements 1 [1 2])))))
		
	(testing "repeated random case"
	    (with-bindings {#'index (fn [] 1)}
	      (is (= [2 2] (random-elements 2 [1 2])))))
	
	(defn ^:dynamic index []
	  0)

	(defn generate-indices [n]
	  (repeatedly n index))

	(defn random-elements [n xs]
	  (if (or (< n 1) (empty? xs))
	    []
	    (map #(nth xs %) (generate-indices n))))

Let's check that `rand-int` is being called. We can do this by making sure that the sum of 10 random elements selected from `[0 10 20 30]` are greater than zero and less than 300.  

    (testing "random selection"
      (let [ns (random-elements 10 [0 10 20 30])
            sum (reduce + ns)]
        (is (< 0 sum 300))))
		
	(defn ^:dynamic index [limit]
	  (rand-int limit))

	(defn generate-indices [n limit]
	  (repeatedly n (partial index limit)))

	(defn random-elements [n xs]
	  (if (or (< n 1) (empty? xs))
	    []
	    (map #(nth xs %) (generate-indices n (count xs)))))
		
The odds that this test will fail are on the order of a million to one.  I could better those odds if I thought it necessary.  If I wanted to eliminate those odds I could write a spy that ensures that the `random-int` function is being called correctly.  But I didn't think any of those machinations were worth it.		
		
So far I've used only integers in the tests.  Most of the tests don't really care about the type of the elements.  So why not change those particular tests to make sure that many different types are handled properly. 

    (testing "trivial cases"
      (is (= [1] (random-elements 1 [1]))))

    (testing "repetitive case"
      (is (= [:x :x] (random-elements 2 [:x]))))

    (testing "singular random case"
      (with-bindings {#'index (fn [_] 1)}
        (is (= ['b'] (random-elements 1 ['a' 'b'])))))

    (testing "repeated random case"
      (with-bindings {#'index (fn [_] 1)}
        (is (= ["two" "two"] (random-elements 2 ["one" "two"])))))
		
And, with that, I think we are done.
		
There are 8 assertions in my tests.  However, most of those assertions were added _in the process_ of TDD.  Now that the function is working, how many assertions are actually necessary?  Probably just the degenerate cases and the final test of randomness.  Let's call that 4 assertions.  I'll leave all the others in for documentation purposes; but they aren't strictly necessary.

What about invalid arguments?  What if someone calls: `(random-elements -23 nil)`?  Should I write tests for those cases?

The function already handles negatives by returning an empty list for any count less than one.  This isn't tested; but the code is pretty clear.  In the case of the nil, an exception will be thrown.  That's OK with me.  This is a dynamically typed language.  You get exceptions when you mess up the types.

Is that a risk?  Sure, but only if some other part of the system was written without tests.  If you call this function from a module that you wrote with tests, using something as effective as the TDD discipline, _you won't be passing `nil` or negative numbers, or any other form of invalid arguments_.  So I'm not going to worry about that.  It's on you.

### The Crux
This is the crux of the argument between Mark and I.  I claim that the number of tests required are only those tests that are necessary to describe the correct behavior of the system.  If the behavior of each element of the system is correct, then no element of the system will pass invalid arguments to any other.  Invalid states will be _unrepresented_.

Of course no suite of tests can completely show that a system is correct.  Dijkstra said it best: "Testing shows the presence, not the absence of bugs."  Still, we have to at least try.  So we demonstrate _practical_ correctness with as comprehensive a suite of tests as we can muster.

I assert that the set of tests that show _practical_ correctness would be the same irrespective of whether the system was written in a statically, or a dynamically typed language.  

Mark asserts that you'll need more tests when using a dynamically typed language because static type checking makes all illegal states _unrepresentable_, thus eliminating them from consideration.  If you have a type that cannot be `nil` (for example) then you don't have to (and simply cannot) write a test that checks for `nil`.  

My answer to that is that even using a dynamically typed language I don't have to write the test for `nil` because I know that `nil` will never be passed.  I don't need to make states unrepresentable, if I know those states will never be represented.

### Anyway...
Back to `random-elements`:

If you compare the two functions that Mark and I wrote (and if you understand Haskell, which I don't) I think you'll find that the two implementations are somewhat similar.  The testing strategies we employed were, however, _very different_.  

The tests I wrote show that I was concerned almost entirely with the desired behavior of the function.  The tests walk that behavior forward one small step at a time using the TDD style.  

Mark, however, was far more concerned with expressing correctness in terms of generic types.  The vast majority of his thought process was consumed with the type structure that properly represented the problem.  He then verified behaviors with property-based `QuickCheck` style tests, and after-the-fact tests. 

Which is better?  

> "Answering _that_ question with specificity is above my pay grade." - Barack Obama.

Which of us wound up with fewer tests?  In either case I think the four base assertions are adequate.  However, as part of the process I wrote 8 assertions and Mark wrote two property-based `QuickCheck` tests, and three after-the-fact regression tests.  Does that amount to 5 vs. 8?  Or do those `QuickCheck` tests count for more?  I don't know.  I'll let you decide.  
   
	
	
	
	

