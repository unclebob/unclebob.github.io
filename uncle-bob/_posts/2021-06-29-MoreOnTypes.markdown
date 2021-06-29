---
layout: post
title: More On Types
tags: ["Software"]
---
Recently I wrote a cute little program for doing _Turtle Graphics_.  For those of you who don\'t know, turtle graphics were originally added to the LOGO language by Seymour Papert in the late 1960s.  He built a robot that he called a \"turtle\" that could hold a pen.  The robot had wheels and could move forwards and backwards, and could rotate left and right.  It could also raise and lower the pen.  When placed on a sheet of paper, the turtle could be commanded to draw interesting designs.

Papert\'s goal was to teach children about programming.  As the years went by the robot got replaced with screens, and the turtle became an icon that could draw lines.  Children from the 70s until now have been enthralled by the simple commands for directing the turtle, and the elegant drawings they can make.

For example, this is how you might draw a square:

	forward 100
	right 90
	forward 100
	right 90
	forward 100
	right 90
	forward 100
	right 90.

Recently I had a need to explore some interesting geometrical designs.  Turtle graphics would be perfect for my purposes.  So I wrote a turtle graphics processor in Clojure.

I used the [`quil`](http://quil.info/) framework which is based on the [`Processing`](http://processing.org) framework in Java. This framework makes it very easy to create simple GUIs in Clojure.

Now consider the problem of the Turtle.  What is the type model for this object?  What fields does it have, and what constraints must be placed on those fields?

Here was my solution to that problem, written in `clojure/spec`.  As usual, in Clojure, you start at the bottom and read towards the top.

	(s/def ::position (s/tuple number? number?))
	(s/def ::heading (s/and number? #(<= 0 % 360)))
	(s/def ::velocity number?)
	(s/def ::distance number?)
	(s/def ::omega number?)
	(s/def ::angle number?)
	(s/def ::weight (s/and pos? number?))
	(s/def ::state #{:idle :busy})
	(s/def ::pen #{:up :down})
	(s/def ::pen-start (s/or :nil nil?
	                         :pos (s/tuple number? number?)))
	(s/def ::line-start (s/tuple number? number?))
	(s/def ::line-end (s/tuple number? number?))
	(s/def ::line (s/keys :req-un [::line-start ::line-end]))
	(s/def ::lines (s/coll-of ::line))
	(s/def ::visible boolean?)
	(s/def ::speed (s/and int? pos?))
	(s/def ::turtle (s/keys :req-un [::position
	                                 ::heading
	                                 ::velocity
	                                 ::distance
	                                 ::omega
	                                 ::angle
	                                 ::pen
	                                 ::weight
	                                 ::speed
	                                 ::lines
	                                 ::visible
	                                 ::state]
	                        :opt-un [::pen-start]))

Now don\'t freak out at all the parentheses and colons.  In fact, for the moment, just ignore them.  

So, what is a turtle?  A turtle is a map whose required keys are as follows:

 * `position` is the cartesian coordinate of the pen of the turtle.  If you look up towards the top you will see that a position is defined as a tuple containing two numbers.  
 
 * `heading` is the direction that the turtle is pointing.  It will move in that direction if told to move forward.  Again, looking up towards the top you can see that a heading must be a number between 0 and 360.  
 
 * `velocity` is a number that represents the speed at which the turtle will move across the screen.  This is used for animation, so that the user can actually watch the turtle travel across the screen.  
 
 * `distance` is a number that represents the remaining distance that the turtle must traverse before the current command (either a `forward` or `backwards` command) is complete.
 
 * `omega` is a number that represents the angular velocity of the turtle.  Again, this is for animation purposes, so that the user can watch the turtle rotate when given a `right` or `left` command.
 
 * `angle` is a number that represents the number of degrees remaining to complete the current rotation command.
 
 * `pen` is the state of the pen.  Looking up you can see that the state of the pen can be either `up` or `down`.
 
 * `weight` is a positive number that represents the thickness of the line drawn by the pen.
 
 * `speed` is a positive integer that acts as a multiplier for both the `velocity` and `omega` parameters.  This allows the user to speed up or slow down the animation.
 
 * `lines` is a list of all the lines drawn by the turtle so far.  Looking up you can see that it is a collection of lines, and that lines are maps whose required keys are `line-start` and `line-end`, both of which are tuples of two numbers. (Yes, I suppose I should have created a `point` type.)
 
 * `visible` is a boolean that determines whether the turtle itself should be visible while it is being animated.  If this is false, then all the user sees is the animated result of the turtle\'s movements.
 
 * `state` is either `busy` or `idle`.  This is used by the command processor.  When the turtle goes from `busy` to `idle` the next command is pulled from the command queue and executed.
 
 It should be clear that this is a type model.  Most statically typed languages would not be able to capture all the constraints within this type model; though there are perhaps some that could.  However, this is not a static type model.  Clojure is not a statically typed language.  `clojure/spec` is a dynamic type definition language.  
 
 What does that mean?  Probably the best way to explain that is to show you where that type model gets invoked.  Here\'s a simple example.  
 
	(defn make []
	  {:post [(s/assert ::turtle %)]}
	  {:position [0.0 0.0]
	   :heading 0.0
	   :velocity 0.0
	   :distance 0.0
	   :omega 0.0
	   :angle 0.0
	   :pen :up
	   :weight 1
	   :speed 5
	   :visible true
	   :lines []
	   :state :idle})

This is the default constructor of the turtle.  Notice that it just loads up all the required fields into a map. Notice also that there is a _post condition_ that asserts that the result conforms the the `turtle` type.

This is nice.  If I forget to initialize a field, or if I initialize a field to a value that conflicts with the type, I get an error.

Here\'s another, more complex example.  Don\'t freak out, you don\'t have to understand this in detail.  

	(defn update-turtle [turtle]
	  {:post [(s/assert ::turtle %)]}
	  (if (= :idle (:state turtle))
	    turtle
	    (let [{:keys [distance
	                  state
	                  angle
	                  lines
	                  position
	                  pen
	                  pen-start] :as turtle}
	          (-> turtle
	              (update-position)
	              (update-heading))
	          done? (and (zero? distance)
	                     (zero? angle))
	          state (if done? :idle state)
	          lines (if (and done? (= pen :down))
	                  (conj lines (make-line turtle))
	                  lines)
	          pen-start (if (and done? (= pen :down))
	                      position
	                      pen-start)]
	      (assoc turtle :state state :lines lines :pen-start pen-start)))
	  )

This is the function that updates the turtle for each screen refresh.  Again, notice the _post condition_.  If anything is calculated incorrectly by the `update-turtle` function, I\'ll get an exception right away.

Now some of you might be worried that by checking types at runtime I could end up with runtime errors in production.  You might therefore assert that static typing is better because the compiler checks the types long before the program ever executes.

However, I do not intend to have runtime errors in production, because I have a suite of tests that exercise all the behaviors of the system.  Here\'s just one of those tests: 

	(describe "Turtle Update"
	  (with turtle (-> (t/make) (t/position [1.0 1.0]) (t/heading 1.0)))
	  (context "position update"
	    (it "holds position when there's no velocity"
	      (let [turtle (-> @turtle (t/velocity 0.0) (t/state :idle))
	            new-turtle (t/update-turtle turtle)]
	        (should= turtle new-turtle)))

Again, you don\'t have to understand this in any detail.  Just notice that the `make` and `update-turtle` functions are being invoked.  Since those functions have _post conditions_ that will check the types, and since my suite of tests is exhaustive, I am quite certain that there will be no runtime errors in production and that my dynamic type checking is as robust as any static type system.

The dynamic nature of the type checking allows me to assert type constraints that are very difficult, if not impossible, to assert at compile time.  I can, for example, assert complex relationships between the values of the fields.  

To expand on that example, imagine the type model of an accounting balance sheet.  The sum of the assets, liabilities and equities on the balance sheet must be zero.  This is easy to assert in `clojure/spec` but is difficult, if not impossible, to assert in most statically typed languages.

Moreover, _I_ get to control when types are asserted.  It is up to me to decide if and when a certain type should be checked.  This gives me a lot of power and flexibility.  It allows me to violate the type rules in the midst of computations, so long as the end result ends up conforming to the types.  

One last point.  In the late 90s and the 2000s, there was a lengthy and animated (and sometimes acrimonious) debate over TDD vs DBC (Design by Contract).  What `clojure/spec` has taught me is that the two play very well together, and both should be in every programmer\'s toolkit.





