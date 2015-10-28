---
layout: post
title: An Open and Closed Case
tags: ["Craftsmanship", "Coding"]
---
<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2013/03/08/AnOpenAndClosedCase.html" />
I awoke this morning to see a twitter conversation about the Open-Closed Principle.  The tweeter was complaining that it didn't make a lot of sense.  He said things like: 

>"Presumably true OCP fans barely use version control, btw. Only reason to change a source file is for bug fixes, right?"

When I read this, I had to scrape my eyebrows off the ceiling.  How could anyone come to that kind of conclusion?  So I dug deeper.  The original tweeter had apparently read [this article](http://docs.google.com/a/cleancoder.com/viewer?a=v&pid=explorer&chrome=true&srcid=0BwhCYaYDn8EgN2M5MTkwM2EtNWFkZC00ZTI3LWFjZTUtNTFhZGZiYmUzODc1&hl=en) that I wrote back in 1996.  As I re-read the article I began to understand why the original tweeter said what he said.  The article defines modules that are open and closed this way:

> 1. They are “Open For Extension”.
This means that the behavior of the module can be extended. That we can make 
the module behave in new and different ways as the requirements of the application change, or to meet the needs of new applications.
2. They are “Closed for Modiﬁcation”.
The source code of such a module is inviolate. No one is allowed to make source 
code changes to it.

OK, as an isolated sound-bite, point two is a bit overstated.  I mean: "no one is _allowed_..."? I'm not sure why I phrased it that way 17 years ago.  I was young and impressionable back then, a mere 43 years old.  So I can only chalk the stridence of that phrase up to my immaturity.  

In my defense, the article _does_ go on to explain things in much less strident terms.  In particular it says that no significant program can be 100% closed. It talks about strategic closure, and closing modules against certain kinds of changes.  So the overall picture the article paints of the OCP is more moderate than that extreme sound-bite.

So I opened up my book from 2003 [Agile Software Development: Principles, Patterns and Practices](http://www.amazon.com/Software-Development-Principles-Patterns-Practices/dp/0135974445) to see what I had written there.  I was glad to see that the intervening years had softened my tone:

>1. "Open for extension."
This means that the behavior of the module can be extended.  As the requirements of the application change, we are able to extend the module with new behaviors that satisfy those changes.  In other words, we are able to change what the module does.
2. "Closed for modification."
Extending the behavior of a module does not result in changes to the source or binary code of the module.  The binary executable version of the module, whether in a linkable library, a DLL, or a Java .jar, remains untouched.

In my next book, [UML for Java Programmers](http://www.amazon.com/UML-Java%C2%BF-Programmers-Robert-Martin/dp/0131428489/) I changed my tone completely:

> This principle has a high-falutin' definition, but a simple meaning:  You should be able to change the environment surrounding a module without changing the module itself.  

In my [Clean Coders Video Series](http://cleancoders.com), I devoted episode 10 to a very detailed exposition of this principle.  In that video I referred back to Bertrand Meyer, the creator of the Open Closed Principle, and I paraphrased Meyer's definition as:

> So Meyer wants it to be easy to change the behavior of a module, without having to change the source code of that module! 

And that's really the essence of the OCP.  _It should be easy_ to change the behavior of a module without changing the source code of that module.  This doesn't mean you will never change the source code, this doesn't mean you can stop using your version control systems (sheesh!).  What it means is that you should strive to get your code into a position such that, when behavior changes in expected ways, you don't have to make sweeping changes to all the modules of the system.  Ideally, you will be able to add the new behavior by adding new code, and changing little or no old code.

As I told the original tweeter, the OCP is a _Mom and Apple Pie_ principle.  I can't see why anybody would, or could, disagree with it.  But when I look back at my old writings, I can at least see why someone who was skimming and looking at the sound-bites might walk away confused.

There are two morals to this story:

1. When you write an article remember that people often skim; so extreme sound-bites used for rhetorical emphasis can wind up creating false impressions. 

2. When you skim an article, remember that often the meat of the article is in the text that you have bypassed, so keep your conclusions tentative until you've had a chance to dig deeper. 










