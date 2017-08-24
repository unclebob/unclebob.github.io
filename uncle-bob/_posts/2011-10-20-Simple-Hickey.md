---
layout: post
title: Simple Hickey
tags: ["Testing", "Tools"]
old_tags: ["TDD", "Simplicity", "Agile"]
---


Rich Hickey gave a great [talk](http://www.infoq.com/presentations/Simple-Made-Easy) at [Strange Loop](https://thestrangeloop.com) entitled *Simple Made Easy*. I strongly recommend you spend an hour and listen to this talk. It's worth every second you give it. There will be some things in this talk that you will disagree with. When that happens, stop and think - think real hard - you probably don't actually disagree. And if you do, you probably shouldn't.

For instance, Rich says some seemingly disparaging things about TDD and Agile and Refactoring - the sacred cows of the Agile Community. If you are wedded to this community, you might react negatively. Don't. Rich is not disparaging the practices. He *is* disparaging the religion - the mindlessness - the *thoughtlessness*.

Rich compares unit tests to guard rails. Then he makes a very good point. He says, when you have a bug, that bug got past your tests. And now what? Now you have to find the bug. And if the system isn't simple, that's not going to be easy. (Note I used the words simple and easy here. The start of Rich's talk is about the very different definitions that these words have. I suggest you stop at this point and listen to the first ten minutes of his talk and then come back to this paragraph again.)

Rich makes the point that sprinters run fast, but not long. Then he says that Agile "solved" this problem by just firing the starting gun over and over again in quick succession. He grins, and the audience laughs. Then he goes on to say that continuous sprinting does not necessarily makes systems simple, and simplicity is the real key to speed.

He's right of course. This is the same point that Martin Fowler made in his [Flaccid Scrum](http://martinfowler.com/bliki/FlaccidScrum.html) article. And it's the point that many of us in the Agile community have been making. That short iterations, without good technical practices, does not lead to fast development. Rather, it leads to a mess.

Rich makes fun of the idea that a suite of tests let's you change the code. He says that tests are a safety net, nothing more. We TDDers know that a suite of tests is *essential* if we want to fearlessly change the code. But Rich is right about the safety net. A safety net can help you keep a system simple, if it's already simple. But a safety net below a big ball of mud is going to be of marginal assistance in detangling the mess. Oh, don't get me wrong. I want those tests! But the job ain't gonna be easy. (again, that word).

Here's another talk from Rich: [Hammock Driven Development](http://blip.tv/clojure/hammock-driven-development-4475586), in which he encourages us to *think* instead of just writing gobs and gobs of code.

So here's the deal. Rich is concerned, and rightly so, that we have a culture of complexity. That when programmers are given a task, they race ahead and write masses of tangled code, using "easy" frameworks and tools, without giving the problem *due thought*. That we confuse easiness, with simplicity. (e.g. Rails is easy, *it is not simple*.) His complaint about tests is that we used them to replace thought. That we feel good about ourselves because we've written tests, and yet we haven't actually given the time to the problem that the problem deserves. We haven't made the problem simple. We've just done what was easy.

Now, truth be told, the Agile community, and the entire software community *is* infected with this disease. All too often we do what's easy, at the expense of what's simple. And so we make a mess. But that is not now, nor was it *ever*, a value of agile development. And it was *certainly not* a value of software craftsmanship! Indeed, doing what is simple as opposed to what is easy is one of the *defining* characteristics of a software craftsman.

In the end, I think Rich's perception of TDD is skewed by what he sees out in the industry. Frankly, I think he's missing a bet. I imagine he'd find that the practice was as helpful to him as it has been to me. Not as a way to avoid thinking and rushing towards a mess; but rather as a disciplined way of being thorough, careful, and thoughtful.

Now, ask yourself what TDD means to you. Is TDD a discipline you use to make things easy? Or is it a discipline you use in order to be thoughtful, careful, and to keep things simple?
