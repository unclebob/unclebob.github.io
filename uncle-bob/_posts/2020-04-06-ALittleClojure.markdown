---
layout: post
title: A Little Clojure
tags: ["Software"]
---
So let's learn just a little bit of clojure.

This expression: `(1 2)` represents the list containing the integers 1 and 2 in that order.  If you want an empty list, that's just `()`.  And the list of the first five letters of the alphabet is just `(\a \b \c \d \e)`.  

Now you know a lot about the syntax of clojure.  Perhaps you think there's a lot missing.  Well, there are a _few_ things missing; but far fewer than you'd think.  

You might be wondering how you add two numbers.  That's easy, that's just `(+ 1 2)`.  As it happens that's also just the list of the function named `+` followed by a 1 and a 2.  You see, a function call is really just a list.  The function is the first element of the list, and the arguments are just the other elements of that list.  When you want to call a function, you simply invoke the list that represents that function call.  

There are quite a few built-in functions in clojure.  For example there's `+, -, *, and /`.  They do precisely what you'd think.  Well, perhaps not precisely.  `(+ 1 2 3)` evaluations to `6`.  `(- 3 2 1)` evaluates to zero.  `(* 2 3 4)` evaluates to `24`.  And `(/ 20 2 5)` evaluates to 2.  `(- 5)` evaluates to `-5`.  `(* 5)` evaluates to `5`.  And, get ready for this, `(/ 3)` evaluates to `1/3`.  That last is the clojure syntax for the rational number one-third.

`(first 1 2 3)` evaluates to `1`, `(second 1 2 3)` evaluates to 2, and `(last 1 2 3)` evaluates to -- you guessed it -- `3`.  

If you'd like to see this in action you'll need to start up a clojure REPL.  You can google how to do that.  The word REPL stands for Read, Evaluate, Print Loop.  It's a very simple program that reads in an expression, evaluates that expression, prints the result of that expression, and then loops back to the read.  

If you start a REPL you'll get some kind of a prompt, perhaps like this `user=>`.  Then you can type an expression and see it evaluated.  Here are a few from my REPL

	user=> (+ 1 2 3 4)
	10
	user=> (- 5 6 7 8)
	-16
	user=> (* 6 7 8)
	336
	user=> (/ 5 6 9)
	5/54

If you try the expression at the very start of this article: `(1 2)` you'll get a nasty surprise.

	user=> (1 2)
	ClassCastException java.lang.Long cannot be cast to clojure.lang.IFn  user$eval1766.invokeStatic (:1)
	
That's because the digit `1` is not a function; and the REPL believes that if it reads a list, that list ought to be evaluated as a function call.  If you just want the list `(1 2)` at the REPL you can convince the REPL not to call the list as a function by quoting it as follows:

	user=> (quote (1 2))
	(1 2)
	user=> '(1 2)
	(1 2)
	user=> (list 1 2)
	(1 2)

The first invokes the `quote` function which prevents its argument `(1 2)` from being evaluated and just returns it.  The second is just a little syntax shortcut for calling the `quote` function.  The third invokes the function that constructs lists.

Lists are implemented as linked lists.  Each element contains a value and points to the next element.  That makes it very fast to add an element to the front of the list, or to walk the list one element at a time.  But it makes it slow to index into the list to find the Nth element.  So, for that, clojure uses the _vector_ data type.  Here is a vector of the first three integers: `[1 2 3]`.  That's right, it's the square brackets that do the trick.  

A vector is a lot like a growable array. It's easy to add to the end of it, and it's easy to index into it.  Lists make good stacks. Vectors make good queues.

Now let's define a function.  `(defn f [x] (+ (* 3 x) 1))`  this defines the function named `f`.  It takes one argument named `x`.  And it calculates the formula: `3x+1`.

Now let's take this apart one token at a time.  This looks like a call to the function `defn`.  We'll let that stand for the moment, but it's not exactly right; `defn` is a bit more special than that. The next token is the name of the function: `f`.  Names are alphanumeric with a few special characters allowed.  For example `+++` is a valid name.  Following the name is a vector that names the function arguments.  Again, these are names.  Those names will be bound to the argument values when the function is called.  And following the argument vector is the expression that is evaluated by the function.  That expression can use the argument names.

You now know the vast majority of Clojure syntax.  There's more, of course, but you already know enough to write significant programs.  

So let's write a simple one.  Let's write the factorial function.<br> 
`(defn fac [x] (if (= x 1) 1 (* x (fac (dec x)))))`  

Let's walk through this.  The function is named `fac` and it takes one argument named `x`.  The `if` function takes three arguments. If the first evaluates to something _truthy_ it returns the second, otherwise it returns the third.  The `=` function does exactly what you'd think: it is a test for equality.  If `x` is 1, then the `if` statement, and therefore the function, will return 1.  Otherwise the `if` statement will return `x` times the factorial of the decrement of `x`.

Let's try it:

	user=> (fac 3)
	6
	user=> (fac 4)
	24
	user=> (fac 10)
	3628800
	user=> (fac 20)
	2432902008176640000
	user=> (fac 30)

	ArithmeticException integer overflow  clojure.lang.Numbers.throwIntOverflow (Numbers.java:1501)

That works nicely, until we exceed 64 bits of precision.  Clojure likes to use 64 bit integers for efficiency.  But if you'd rather have unlimited precision you can use the `N` notation.  

	user=> (fac 1000N)
	402387260077093773543702433923003985719374864210714632543799910429938512398629020592044208486969404800479988610197196058631666872994808558901323829669944590997424504087073759918823627727188732519779505950995276120874975462497043601418278094646496291056393887437886487337119181045825783647849977012476632889835955735432513185323958463075557409114262417474349347553428646576611667797396668820291207379143853719588249808126867838374559731746136085379534524221586593201928090878297308431392844403281231558611036976801357304216168747609675871348312025478589320767169132448426236131412508780208000261683151027341827977704784635868170164365024153691398281264810213092761244896359928705114964975419909342221566832572080821333186116811553615836546984046708975602900950537616475847728421889679646244945160765353408198901385442487984959953319101723355556602139450399736280750137837615307127761926849034352625200015888535147331611702103968175921510907788019393178114194545257223865541461062892187960223838971476088506276862967146674697562911234082439208160153780889893964518263243671616762179168909779911903754031274622289988005195444414282012187361745992642956581746628302955570299024324153181617210465832036786906117260158783520751516284225540265170483304226143974286933061690897968482590125458327168226458066526769958652682272807075781391858178889652208164348344825993266043367660176999612831860788386150279465955131156552036093988180612138558600301435694527224206344631797460594682573103790084024432438465657245014402821885252470935190620929023136493273497565513958720559654228749774011413346962715422845862377387538230483865688976461927383814900140767310446640259899490222221765904339901886018566526485061799702356193897017860040811889729918311021171229845901641921068884387121855646124960798722908519296819372388642614839657382291123125024186649353143970137428531926649875337218940694281434118520158014123344828015051399694290153483077644569099073152433278288269864602789864321139083506217095002597389863554277196742822248757586765752344220207573630569498825087968928162753848863396909959826280956121450994871701244516461260379029309120889086942028510640182154399457156805941872748998094254742173582401063677404595741785160829230135358081840096996372524230560855903700624271243416909004153690105933983835777939410970027753472000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000N

OK, one last thing.  Let's add up all the numbers in a list.  We want `(sum [1 2 3 4 5])` to evaluate to `15`.  First we'll do it the hard way:<br>
`(defn sum [l] (if (empty? l) 0 (+ (first l) (sum (rest l)))))`

The `empty?` function does just what you'd think, it returns true if the list is empty.  The `rest` function returns all but the first element of a list.  

Of course we could have written `sum` like this: `(defn sum [l] (apply + l))`.  The `apply` function -- um -- applies the function passed in it's first argument to the list in its second. 

We could also have written the function like this: `(defn sum [l] (reduce + l))`.  But that takes us to the `reduce` function which (as George Carlin used to say) might go a bit too far.  At least for this article.


