---
layout: post
title: "Is TDD Dead?<br/>Final Thoughts about Teams."
tags: ["Testing"]
---
The five episodes of the [_Is TDD Dead?_ hangout](https://www.youtube.com/watch?v=z9quxZsLcfo), are now over.  The chatter has died down.  David, Kent, and Martin have had their says. The audience has asked their questions and gotten some answers.  Now we can put the whole thing to bed  and get on about our business.  

Seldom has a keynote talk created such a big splash.  I don't ever recall a keynote, and a simple blog, generating such a loud fuss.  Indeed, beyond any other argument, I think the volume of that fuss exposed the flaw in the original thesis.  If TDD were a dead topic, the brouhaha that we've all watched would not have happened.

The conclusion of the hangout was amicable, respectful, and agreeable.  Martin's and Kent's position was that TDD worked for them in many circumstances; but not all.  David's position was that TDD worked for him in fewer circumstances but still some.  If there was a disagreement, it was simply a matter of degree.  All parties agreed that programmers should try TDD and then tune their use of it to what works _for them_.

###Individuals.
Those last two words: "_for them_", suggest that Martin, Kent, and David were thinking of TDD as an _individual_ practice that some individuals may find more useful than others.  Indeed, the notion of _individual_ was prominent throughout all the episodes.  It seems pretty clear that David, Kent, and Martin tend to work individually on software and not as part of long term teams.  At no point in the hangouts did any of them talk for long about TDD in the context of a _team_.

To be fair, I'm sure that all three of them have worked on teams before.  I'm sure they all _interact_ with teams now.  Still, the fact that teams were barely mentioned in the episodes is striking.  The overriding message of the hangouts was that programmers should do what is right in their own eyes _as individuals_; with nary a word about how they should behave in teams.

###Teams.
Yet most software is built by teams.  Well functioning teams are essential if large software projects are to be successful.  Indeed, one of the founding goals of the Agile movement is to enable the creation of high-functioning teams.  And high-functioning teams must have a shared set of values.

Teams that don't enjoy a shared set of values are unstable.  If each member of the team does what is right in their own eyes, without considering the values of the team; then they don't actually comprise a team.  Instead they will behave chaotically, and work at cross purposes to each other.

### A Team Divided.
Imagine a team of programmers working together on a project.  Half of them (call them the "K" faction) value TDD the way Kent and Martin do.  They create many small unit tests in very short cycles.  The other half (The "D" faction) use David's approach of relying on integration tests in long cycles and very few unit tests.   The "K" faction's tests run fast.  The "D" faction's tests run slow.  The "K" faction has very high test coverage.  The "D" faction has lower test coverage.  The "K" faction isolates themselves from peripherals by using mocks across significant architectural boundaries.  The "D" faction binds more tightly to those peripherals, and considers the isolation to be "design damage".

How can such a team work together?  How can such a team _stay_ together?  

The fast suite of tests that the "K" faction depends upon in order to refactor is poisoned by the "D" faction's slower integration tests.  The coverage that the "K" faction relies upon for the confidence to refactor is denied to them by the "D" faction.  And the "D" faction's vision of design is distorted by all the isolation and mocking created by the "K" faction.  

This team is divided; and unless they can somehow come to terms they will continue to work at cross purposes.  Eventually they are bound for divorce.  

### Divorce.
The divorce isn't a fast process.  Frustration builds amongst the individuals until they start to look for other teams to join; teams that share their values.  Bit by bit one of the factions will grow to dominate.  The other faction will shrink by attrition.

I've seen this happen through internal transfers within a company; but it is also common for people to leave their current company and find a new company to work for.  

The bottom line is simple.  TDD is a team discipline, not simply an individual discipline.  Team members and team leaders need to be very careful to ensure that any new members that they recruit share the values of the team.  Thus TDD teams will grow with more TDDers; and non TDD teams will grow with more non-TDDers.  To the extent that there is a mismatch, attrition will change the compositions of the teams until their values match.

### Evolution.
As time goes on these two values will continue to separate.  They will separate within companies; creating TDD and non-TDD factions within organizations.  As employees move from company to company, they will gravitate towards companies that share their values; creating TDD and Non-TDD companies.  

This process is already taking place.  There are now whole companies who declare the TDD value.  As this process continues, and the differences become ever more stark, Natural Selection and Survival of the Fittest, will determine which companies, and which values, thrive.

Of course this is vastly oversimplified.  If the hangout episodes showed anything it was that TDD is not a boolean value.  The disagreement in the hangout was more a disagreement of degree and less a disagreement of kind.  

Even so, teams can't tolerate a huge difference in degree.  So the separation and sorting will continue and the best set of values will eventually prevail.

Perhaps you think this is a prediction without basis.  Perhaps you think I'm just woolgathering and staring at clouds in my coffee.  But consider:

* Structured Programming was a huge controversy in the '70s. The idea that `Goto` was "harmful" caused wars and rumors of wars in the editorial pages of the trade journals.  Nowadays, however, we look askance at any use of `goto`.  The factions separated, and the structured programming faction won out.

* Objects were wildly controversial in the '80s.  The internet newsgroups were alive with flame wars over the topic.  Proponents and detractors flamed each other with intensity and venom.  Nowadays however, we use objects as a matter of course.  The factions separated, and the object faction won out.

* Agile was wildly controversial in the late '90s and early '00s.  Whole books were published about how Agile could not possibly work.  Nowadays, Agile has become mainstream and is rapidly gaining momentum.  The factions separated and Agile won out.  

In 2014 I'm betting on the TDD faction.  
