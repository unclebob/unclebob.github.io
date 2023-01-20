---
layout: post
title: Service Oriented Agony
tags: ["Architecture", "Craftsmanship"]
old_tags: ["Architecture", "Craftsmanship",  "SOA"]
---

I sat down with a group of developers today to do a retrospective on a project. They told me that project management has been complaining about their velocity. It wasn't serious, but the developers felt bad because things *do* seem to take longer than they should. When I asked them why, they said that the system was very complex, and making changes to it was time consuming.

I asked them to draw the system on the whiteboard. Here's what they drew.

<img src="http://dl.dropbox.com/u/4730299/blog_images/service-oriented-agony.jpg" width="80%"/>

The system has several front end processes, each is its own Rails app. They talk to each other by throwing URLs at each other, and loading cookies, etc. Pretty normal.

Each of the front ends talks to some gems which act as interfaces for the services. The services are individual Rails apps that communicate through HTTP using JSON. These services each manage a backend database.

I suspected the answer, but I asked the developers why this structure slowed them down. They told me that every time there is a new feature to add, they have to add it in three different places. They have to change the front end to add the UI gestures. They have to add a new function to one of the gems in order to provide the UI with the new service interface. Then they have to add a new MVC triplet to one of the services that the gem will use to access the data.

I asked them if this was true even if there weren't any schema changes; and they told me that it was because the gems and the services had evolved with the whole application, and only had functions for the current features. New features often require new functions even though there's no schema change.

This is a pretty typical structure. I've seen it many times before. I've seen it in Java apps, C++ apps, and now even Rails apps. The structure seems obvious to system designers who have grown tired of single monolithic systems and want to break those systems up into components and services. What could be more natural than to break the system along the lines of data base management?

Unfortunately this is a huge violation of the Single Responsibility Principle -- or its big brother the Common Closure Principle. These principles tell us to group together things that change together, and keep apart things that change for different reasons. Unfortunately the above design separates things that change for the same reasons, and groups together things that change for different reasons. No wonder the developers feel like they are going slow!

When you separate things that change for the same reasons, you have to make changes in many different places in the system. If those places are in different applications, then you have to switch mental contexts for each change. Testing the change is hard because you have to have all the components running to test it end-to-end. And you have to make sure that all the interface points work as desired. So it's a lot of work just to get *anything* working.

Moreover, when you group together things that change for different reasons, you expose the components of the system to collateral damage, thrashing, CM collisions, and a whole host of other problems. Changing one of those gems, for example, has an impact on all the front ends, even though some of those front ends won't use the part of the gem that was changed.

So what's the solution? First of all, I question whether the system needed to be partitioned into services. Services are expensive and complicated, you should only create them if you *absolutely* need to. It's always easier to live in a single process. Remember Martin Fowler's first law of distributed objects: *Don't distribute your objects.*

Let's assume, however, that the system *did* need to be partitioned into services. Are these the right services? Dividing a system into UI and DB services is a *physical* partitioning. What we should be looking for is families of *business rules* that are separate from each other, and can be partitioned into true business services.

Finally, let's assume that the physical partitioning was necessary for some reason. Then we need to find a way to make the interfaces of the gems and DB services generic so that every new feature doesn't require a change throughout the whole system.

Like I said, I see this kind of partitioning a lot; and it always has the same outcome. It slows development because it smears features through many different layers. Many systems could be streamlined, and development made much faster, if the system designers paid more attention to the Single Responsibility Principle.
