---
layout: post
title: "Oh Foreman,  Where art Thou?"
tags: ["Craftsmanship"]
---
The response to my previous blog: [Where's the Foreman](http://blog.8thlight.com/uncle-bob/2014/02/21/WhereIsTheForeman.html) has been mixed.  While the vast majority of folks seemed to agree; there was a vocal minority of people, whom I respect, who were quite negative.  The thing that most of these people hated the most was my insistence that the foreman be the only person with commit rights.  

The complaints were all based on the notion of _team_ vs. _foreman_.  Those who disagreed with my blog seem to feel that a software team is based on egalitarian rules, where all are peers, and none has authority over others.  Nearly all of these people call themselves _coaches_; which is odd because, of all the roles in a team, the role of _coach_ is the _least_ egalitarian.  The coach is _special_.

Some of these coaches pointed out to me that the role of a coach (as opposed to a foreman) is to teach, to anticipate problems and set up "learning moments", to help the team "grow".  Actually, quite a few people used the term "grow".  Is there any way to be _less_ of a peer, than to be the agent of "growth"?  

My point here is that _a coach is not a peer_.  A coach takes _responsibilities_ that the other team members do not.  And, therefore, the coach has _authority_ that the other team members do not.  

### The Perfect Team

Now, for a moment, let's imagine the perfect team.  All the team members are enthusiastic, knowledgeable, and compatible.  They write tests.  They pair.  Their code is clean.  They work from iteration to iteration, getting lots done.  The customer is happy.  The coach has nothing to do except watch, and plan for future growth.  All is well.  

In such a team, who has commit privileges?  Obviously everyone does; because everyone works well with each other and everyone trusts each other.  No one in this team commits bad code.  No one on this team commits code without tests.  So all have commit privileges.

Do such teams exist?  For brief moments, yes.  But no team stays in this state forever.  Humans are messy.  Things happen.  And corrective action must sometimes be taken.  

### The Story of Ron

Last week Ron was a functioning member of our "perfect team"; but Ron just screwed up big time.  For the last few days he didn't come in to work at all.  He worked from home instead.  He had a task to complete, and he committed it last night at 3AM.   Clearly he wasn't pairing.  The code is crap.  There are no tests.  And nobody but Ron knows this because nobody expects the problem.

Of course the coach calls Ron and asks if everything is OK.  Ron says he's a little under the weather, but should be back in the office soon.  Satisfied, the coach reports to the team; and everyone remains confident, enthusiastic, and ignorant of the time bomb that has been put into the commit stream.

As promised, Ron returns a few days later.  But something is different about him.  Oh, the old Ron shows through from time to time.  Some days are perfectly normal.  Other days, however, Ron is withdrawn, depressed.  It's obvious that something is wrong; and it's obvious that Ron is trying to overcome it.

The coach talks with Ron about it.  Ron simply says there are some personal problems that he'd rather not share; and that things are going to be OK soon.  

When Ron pairs with others, he's a bit more passive than usual; but is still helpful.  He sometimes gets very engaged, just like he used to.  Everyone expects that his troubles will pass.  

What they don't know is that when Ron works alone, he cannot muster the will to write tests, to refactor, to clean the code.  What they don't know is that all his mental energy is being expended in keeping up appearances.  He's barely got enough left to get his own tasks _working_, let alone clean and tested.  Nobody sees the stream of commits that is reducing the code coverage.  Nobody sees the crappy code that is coupling the system.  Nobody sees the disease that's beginning to eat at the structure of their system.

Because of their trust in their "perfect team", they are all but blind to the suffering of their team member.  You see, what they don't know, is that Ron's wife has been diagnosed with Cancer.  

Clearly Ron's troubles are going to continue for some time.  Unfortunately the team, in it's blind faith in it's own perfection, will not detect the corruption in the code for some time.  Gradually they will begin to see their coverage numbers decline.  They'll see strange and intermittent failures of the integration test suite.  They'll note that certain modules are becoming more and more error prone, and harder and harder to change. 

And then it will be discovered.  Someone on the team will trace back the commits that are causing the troubles, and they'll realize that they are all coming from Ron.  They also realize that repairing the mess will require weeks; and that they can't trust Ron to do it.  Indeed, they can't trust Ron at all.  Schedules have to be revised.  Customers must be notified.  There is anger.  There is recrimination.  

In disgrace, Ron resigns.

### Reviso

Now, let's play this out again.  But this time, let's imagine that the coach is just a little smarter, and a little less trusting than before.  Let's say that the coach takes seriously the responsibility for technical quality, and understands that good people sometimes do bad things.  So our coach, let's call her Jessica, reviews every commit.  This isn't a public thing.  Jessica just does this as part of her job.  There's no formal review, no document trail, no daily report.  Jessica just looks at every single commit, looking for problems.

And, of course, Jessica finds Ron's first bad commit within hours.  So she's on the phone to him, asking him why he committed code without tests, code that had not been refactored.  Ron says he's a bit under the weather but that everything will be fine soon. He promises to fix the commit before the end of the day.  

Jessica accepts this, but starts to pay closer attention to Ron's commits.  Ron, aware that Jessica is watching, tries his best; but can't muster the emotional energy to keep up appearances _and_ keep the code clean.  He never does fix that commit.  He avoids pairing.  He starts to miss deadlines.  There's no place for him to hide.

The whole team can now see that something is very wrong with Ron.  Jessica confronts Ron with the evidence.  Bad commits.  Missed deadlines.  "What's going on, Ron?"

The truth about Ron's wife comes out.  The team rallies around Ron.  Tasks are redistributed.  Ron's load is lightened.  The team survives.

Thank goodness Jessica was _watching_!    

### Coach or Foreman?

Are these two really the same role?  Of course they are!  After all, the coach of a sports team is also the foreman of that team.  The coach sets up training and practices schedules.  The coach designs the playbook.  The coach chooses the menu of plays for a particular game.  The coach chooses which players are on the field, and when.  And the coach can bench a player for infractions.  The coach has commit rights!

Now, perhaps you are a coach, but you aren't technical.  That's OK, coaches often have assistant coaches to help them.  And the coach can delegate responsibilities and authorities to those assistants.  So if you are one of those coaches who is a process expert, but not a coding expert; _you're going to need a coding expert to act as foreman_.

### Commit Rights?  Really?

Of course!  But look, in a well functioning team, the foreman/coach doesn't have to withhold commit privileges.  In a well functioning team the foreman _allows_ everyone to commit, and then simply, and silently, reviews the work.  If someone does a great job, an attaboy is appropriate.  If someone does a poor job, a private conversation, followed by remedial action, is appropriate.  In the normal case, everyone on the team can commit.

But the foreman is the only one with _the right_ to commit.  What that means is that if Angela is the foreman, she can revoke your permission to commit and reduce you to issuing pull requests.   This would, of course, be a rare occurrence; based on extreme misbehavior or malfeasance.  A good foreman would rarely use that power; but the power must be there.

New team members ought not be granted commit privilege right away.  It would be wise to have them earn that privilege by demonstrating their good work through pull requests for the first few iterations.  Once earned, and appropriately celebrated, the new team member knows they are truly part of the team.

In large teams, a foreman will have to find assistants to help.  Those assistants will review every commit.  The foreman will do spot checks.  

### Is this really so strange?

No, actually, it's not.  It's the way good work gets done.  Would you sail on a ship, or fly in an airline, without a captain?  Would you build a house without a general contractor?  Would you run a sports team without a coach who had the power to bench the players?  Would you create an orchestra without a conductor?  Would you produce a movie without a director?  Would you fight a war without a general?

No, of course not.  We've learned that lesson the hard way too many times.  The truth about teams is that teams only function well when there is a competent leader that holds the commit rights.



  

