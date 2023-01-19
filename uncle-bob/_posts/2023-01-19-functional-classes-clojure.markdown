---
layout: post
title: Functional Classes in Clojure
tags: ["Software"]
---
My previous [blog](http://blog.cleancoder.com/uncle-bob/2023/01/18/functional-classes.html) seemed only to continue the confusion regarding classes in Functional Programming.  Indeed, many people got quite irate.  So perhaps a bit of code will help.

__Trigger Warning__:
 
 - Object Oriented Terminology. 
 - Dynamically Typed Language.
 - Mixed Metaphors.
 - Distracting Animations.

>To all the adherents of the _Statically Typed_ Functional Programming religion:  I know that you believe that _Static Typing_ is an essential aspect of Functional Programming and that no mere dynamically typed language could ever begin to approach the heights and glory of _The One True and Holy TYPED Functional Apotheotic Paradigm_.  But we lowly programmers quivering down here at the base of _Orthanc_ can only hope to meekly subsist on the dregs that fall from on high.  

<iframe width="560" height="315" src="https://www.youtube.com/embed/1KRqeDEQcYk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

(R.I.P. Kirstie Ally)

OK, so, once again...

>_A class is an intentionally named abstraction that consists of a set of narrowly cohesive functions that operate over an internally defined data structure._

We do not need the `class` keyword.  Nor do we need polymorphic dispatch.  Nor do we need inheritance.  A class is just a description, whether in full or in part, of an object.  

For example -- it's time we talked about clouds (which I have looked at from both sides now; and do, in fact, understand pretty well).  

So... Here come your father's parentheses!

<img src="https://i.pinimg.com/originals/4f/1e/26/4f1e261d1afa9d58fd1125db5a5a4a12.jpg">

	(ns spacewar.game-logic.clouds
	  (:require [clojure.spec.alpha :as s]
	            [spacewar.geometry :as geo]
	            [spacewar.game-logic.config :as glc]))

	(s/def ::x number?)
	(s/def ::y number?)
	(s/def ::concentration number?)

	(s/def ::cloud (s/keys :req-un [::x ::y ::concentration]))
	(s/def ::clouds (s/coll-of ::cloud))

	(defn valid-cloud? [cloud]
	  (let [valid (s/valid? ::cloud cloud)]
	    (when (not valid)
	      (println (s/explain-str ::cloud cloud)))
	    valid))

	(defn make-cloud
	  ([]
	   (make-cloud 0 0 0))
	  ([x y concentration]
	  {:x x
	   :y y
	   :concentration concentration}))

	(defn harvest-dilithium [ms ship cloud]
	  (let [ship-pos [(:x ship) (:y ship)]
	        cloud-pos [(:x cloud) (:y cloud)]]
	    (if (< (geo/distance ship-pos cloud-pos) glc/dilithium-harvest-range)
	      (let [max-harvest (* ms glc/dilithium-harvest-rate)
	            need (- glc/ship-dilithium (:dilithium ship))
	            cloud-content (:concentration cloud)
	            harvest (min max-harvest cloud-content need)
	            ship (update ship :dilithium + harvest)
	            cloud (update cloud :concentration - harvest)]
	        [ship cloud])
	      [ship cloud])))

	(defn update-dilithium-harvest [ms world]
	  (let [{:keys [clouds ship]} world]
	    (loop [clouds clouds ship ship harvested-clouds []]
	      (if (empty? clouds)
	        (assoc world :ship ship :clouds harvested-clouds)
	        (let [[ship cloud] (harvest-dilithium ms ship (first clouds))]
	          (recur (rest clouds) ship (conj harvested-clouds cloud)))))))

	(defn update-clouds-age [ms world]
	  (let [clouds (:clouds world)
	        decay (Math/pow glc/cloud-decay-rate ms)
	        clouds (map #(update % :concentration * decay) clouds)
	        clouds (filter #(> (:concentration %) 1) clouds)
	        clouds (doall clouds)]
	    (assoc world :clouds clouds)))

	(defn update-clouds [ms world]
	  (->> world
	       (update-clouds-age ms)
	       (update-dilithium-harvest ms)))
	
Some years back I wrote a nice little [spacewar game](http://blog.cleancoder.com/uncle-bob/2021/11/28/Spacewar.html) in Clojure.  You can play it [here](http://spacewar.fikesfarm.com/spacewar.html).  While playing, if you manage to blow up a Klingon, a sparkling cloud of _Dilithium Crystals_ will remain behind, quickly dissipating.  If you can guide your ship into the midst of that cloud, you will harvest some of that _Dilithium_ and replenish your stores.  

The code you see above is the _class_ that represents the _Dilithium Cloud_.  

The first thing to notice is that I defined the _TYPE_ of the `cloud` _class_ -- _dynamically_.  

<img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fyt3.ggpht.com%2Fa%2FAATXAJxJ07NzOxzlMLuiV6SGv808JXSCrALLJMXJ1w%3Ds900-c-k-c0xffffffff-no-rj-mo&f=1&nofb=1&ipt=c5ef9c40d05f13c2ce88c8b8e19a2d8e99040bafec62426d6124c5c141a9fbce&ipo=images" width="150">

A `cloud` is an object with an `x` and `y` coordinate, and a `concentration`; all of which must be numbers.  I also created a little type checking function named `valid-cloud?` that is used by my unit tests (not shown) to make sure the _TYPE_ is not violated by any of the _methods_.

Next comes `make-cloud` the _constructor_ of the `cloud` _class_.  

<iframe src="https://giphy.com/embed/vyTnNTrs3wqQ0UIvwE" width="480" height="400" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/theoffice-the-office-tv-frame-toby-vyTnNTrs3wqQ0UIvwE">via GIPHY</a></p>

There are two overloads of the _constructor_.  The first takes no arguments and simply creates a `cloud` at (0,0) with no _Dilithium_ in it.  The second takes three arguments and loads the _instance variables_ of the _class_.

<iframe src="https://giphy.com/embed/2yP1jNgjNAkvu" width="480" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/monty-python-2yP1jNgjNAkvu">via GIPHY</a></p>

There are two primary _methods_ of the `cloud` _class_: `update-clouds-age` and `update-dilithium-harvest`.  The `update-clouds-age` _method_ finds all the `cloud` _instances_ in the `world` _object_ and decreases their concentration by the `decay` factor -- which is a function of the number of milliseconds (`ms`) since the last time they were updated. The `update-dilithium-harvest` _method_ finds all the `cloud` _objects_ that are within the `ship` _object_'s harvesting range and transfers _Dilithium_ from those `cloud` _objects_ to the `ship` _object_.

Now, you might notice that these _methods_ are not the traditional style of method you would find in a Java program.  For one thing, they deal with a list of `cloud` _objects_ rather than an individual `cloud` _object_.  Secondly, there's nothing polymorphic about them.  Third, they are _functional_, because they return a new `world` _object_ with new `cloud` _objects_ and, in the case of `update-dilithium-harvest`, a new `ship` _object_.

So are these really _methods_ of the `cloud` _class_?  Sure!  Why not?  They are a set of narrowly cohesive functions that manipulate an internal data structure within an intentionally named abstraction.  

For all intents and purposes `cloud` is a °°°°°° °°°°°°° _class_.

<iframe src="https://giphy.com/embed/TcdpZwYDPlWXC" width="480" height="240" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/reaction-laughing-lotr-TcdpZwYDPlWXC">via GIPHY</a></p>

So there.






