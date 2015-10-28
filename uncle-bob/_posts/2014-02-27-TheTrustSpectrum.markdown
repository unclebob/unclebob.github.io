---
layout: post
title: "A Spectrum of Trust"
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2014/02/27/TheTrustSpectrum.html" />
The response to my two previous blogs: [Where's the Foreman]({% post_url 2014-02-21-WhereIsTheForeman %}) and [Oh Foreman Where Art Thou]({% post_url 2014-02-23-OhForemanWhereArtThou %}) continues to be mixed; and has gotten quite loud.  That's a good thing; because we need to have this discussion.

###The Perfect Agile Team

At one end of the spectrum of trust within a team is the _perfect_ agile team. Such a team is composed of 6-12 experienced developers with good design sense and a deep commitment to craftsmanship. They all sit around a single table.  They all start and end work at the same time.  They all pair with each other 100% of the time; changing pair partners several times per day.  They have an on-site customer who perfectly balances the backlog.  Their velocity is consistent and flat from iteration to iteration.  They all practice TDD perfectly.  Their code coverage is 100%.  They all refactor mercilessly.  Their code is clean.  They are the _perfect_ agile team.

Does this team need a foreman[1]?  The question is meaningless because the team is composed of _nothing but_ foremen.  Everyone on the team is trusted; and every line of code is seen by a trusted pair partner over and over again.  No single individual needs to take on a special inspection role, because all members are taking that role.  

This team also does not need a coach or a scrum master (certified or otherwise).  The team is perfect.  No coaching is necessary.  They are _all_ coaches.

There are likely some teams out there that approach this ideal; but in my experience they are few and far between.

###Open-Source Single-Committer

At the other end of the trust spectrum is the lone programmer who creates an successful open source project.  Let's call her Rachel.  Rachel finds herself at the focal point of a community of willing and eager programmers, all grimly determined to be of service to her.  She is inundated by a steady stream of pull requests and patches.  Of course Rachel welcomes the help; but the contributions are all over the map as far as quality is concerned.  Some of the contributors share her values, and have done a nice job.  She has no problem committing their code.  Other contributors, however, have done all the wrong things, and have made a mess.  She can't have those contributions contaminating her code base.

So Rachel cannot afford to trust all the contributing programmers because they don't all share her values.  And so Rachel reviews the pull requests she thinks are important; and only commits those that rise to her values and meet her standards of quality.  

Rachel is the foreman.  

The more success Rachel's open source project enjoys, the more help she needs managing the backlog, pull requests, and patches.  To avoid being overwhelmed she chooses a trusted ally from among the most prolific and astute of the contributors.  We'll call her Betty.  

In contribution after contribution Betty has proven that she shares Rachel's values.  So Rachel gives Betty commit rights and the two of them work together on the backlog, and on reviewing the stream of contributions from the user community.  They are now sharing the foreman role.

Of course this process will continue until the core team of highly trusted individuals rises to a level that can deal with both the backlog and the contributions.  This core team has shared values and mutual trust.  They all act as foremen over the contributions.  

We could view the combination of the core team and the community of contributors as an extended team. The trust level in this extended team is low because the contributors don't necessarily share the values of the core team.  Rachel, Betty, and the rest of the core team trust each other well enough; but the community at large is another matter.  In order to maintain the quality of the project, and keep the standards of excellence high, the core team must review each request and reject all those that don't meet their standards.

There are many projects that use a model like this.  Junit, FitNesse, NUnit, Linux, git, etc.  Some of the very best software out there uses this approach.  Indeed, much of the software revered within the Agile community, was produced, and is maintained, using a foreman model.

###How do I convince the rest of my team?
In my travels I encounter many different software teams. Very few are high functioning Agile teams. Most fall pretty low on the trust scale.  To one extent or another, most of these teams pay lip-service to Agile principles; but do not pair, do not consistently write tests, do not refactor, do not have an on-site customer, etc..  They are, in fact, regular old dysfunctional software teams with all the problems you'd expect.

In most cases I encounter these teams because a few members attend a course of mine; or convince their management to bring me in an teach some part of the team.   I teach them TDD.  I teach them Refactoring.  I teach them Craftsmanship.  I teach them design principles.  I teach them professionalism.  I teach them why it is important to care.  I teach them why it is important to never rush.  I teach them that the only way to go fast is to go well.  

And then, at the end; when I am done teaching and about to leave them, they ask me _the most common question I hear_ :

>How do I convince the rest of my team to do the things you've taught?

And there you go.  That's the same trust issue that Rachel and Betty had, isn't it?  The members of the team don't value the same things; and so they don't trust each other to write the tests, refactor the code, and keep things clean.  

###Enter the Coach
Lots of people try to solve this misalignment of values through coaching.  The team hires a coach, and the coach works with the team, addressing fears, allaying suspicions, guiding, teaching, cajoling, and gradually, softly, moving the team towards trust. 

When this works it is a beautiful thing. 

It doesn't work very often.  The track record for coaching dysfunctional teams is less than stellar.  The landscape is littered with some pretty spectacular failures; while the notable successes are few and far between.  

This is not to say that coaching is a bad idea; it is not.  Coaching is a very good idea _when the values of the team members are aligned_.  When you have a team that is willing to give it a try; because they truly do value the same things; then a coach can make a huge difference.  But a coach is not the answer when you have a fundamental disagreement of values inside the team.  

That's why the coaches of sports teams are also foremen.  Not only do such coaches teach, guide, advise, and cajole; they also have the power to say "no".  A coach, faced with a disagreement of values, must have the power to draw a line in the sand and say: "Only things on this side of the line may pass".

###My Answer to That Question.
So, at the end of my talks, or courses, when the team members ask me how to convince their other team mates; I tell them this:

Most often you _can't_ convince people to change their behavior.  At least not by talking.  Telling people that they are doing things wrong, and that _you_ know how they should do them right, is not a good way to convince someone.  What you _can_ do, is be a role model.  You can stick to your disciplines and values, and show the others, by example, how to behave.  Perhaps one or two others will notice what you are doing, and decide to emulate you.  

And then you'll have a problem.  Because in a team where some follow TDD (for example) and others don't, the level of conflict will rise until there is a divorce.  Usually that divorce involves developers leaving the team to find another team that they are more aligned with. 

That period of conflict can be long and painful; and the quality of the software will suffer throughout that period leaving a legacy of troubles.

###Foremen
If, however, the person asking me that question happens to be someone who has the appropriate responsibility and authority I tell them to look at the open-source model; because that model has proven effective in teams that have significant trust issues.  

I tell them to form the people they trust into a core team; and give that core team the power to draw a line in the sand, the responsibility to declare standards of quality and behavior, and the authority to enforce those standards.  i.e. a team of foremen.

###Precipitate the Crisis
Several people who read my previous articles have blogged that foremen will cause distrust and destroy the team.  However, the team I am referring to is already in a state of distrust, and is already on a slow and painful course to destruction.  The bloggers are right; the foremen will accelerate that.  They'll bring the issue to the fore and precipitate a crisis.

And that's a good thing.  I want the crisis to come quickly.  I want the period of divisiveness to be short.  I want those who are going to change to change sooner rather than later.  I want those who are going to leave, to leave sooner rather than later.  I want to get a team with shared values and mutual trust in place as quickly as possible.  And while that process is going on, I want the code to be protected by the people I trust.

###End Game
The goal, of course, is to increase the ranks of the trusted team members and decrease the ranks of those that aren't trusted.  The goal is to create a high functioning agile team in which everyone shares the same values.  The goal is to obviate the foremen because _everyone_ has become a foreman.  

But most teams aren't high functioning Agile teams. Most teams are caught in a divisive struggle over disciplines, quality, and professionalism. Most teams need to draw a line in the sand and decide what their values are, and what disciplines, behaviors, and standards they will use to support those values.  Most teams need to set, and then enforce, quality standards.  

The open-source committer model (i.e. using foremen) has been shown very effective at achieving those ends.









 











----
[1] In this paper the term 'foreman' is used in it's gender neutral form.  No gender bias is intended by the author; nor should any be imputed by the reader.




  

