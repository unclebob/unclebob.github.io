---
layout: post
title: Fecophiles
tags: ["Craftsmanship"]
old_tags: ["Professionalism", "Craftsmanship", "Clean Code"]
---

I got an interesting email yesterday. It contained the following paragraph describing an email he had sent to his co-workers about a refactoring he had done:

> When I originally sent this email to my co-workers, I got the following reaction; 1 team member thought it was better & 2 team members thought it was harder to understand. Of the 2 who thought it was worse; 1 thinks I should change it back, and the other is willing to put up with my change to shut me up :-)

Here's the letter he sent to them, showing them the before and after of the code he had refactored.

    Subject: FW: Refactoring of the day


    Am I missing something?  Or did I just refactor a 31 loc method to 2 loc?

    ------- before ------
    public static string GetLtsCode(IventoryBinItem item)
    {
        string ltsCode = null;
        if (!string.IsNullOrEmpty(item.ParentItemNo)) //child item
        {

            ltsCode = "PzT";
            var isNfoOrDisc = item.IsNfoItem || item.IsDiscontinuedItem;
            //if (isNfoOrDisc && item.ParentIsNfoOrDiscontinuedItem ||
            //    !isNfoOrDisc && !item.ParentIsNfoOrDiscontinuedItem)
            if (!item.ParentIsNfoOrDiscontinuedItem)
            {
                ltsCode = "PzT";
            }
            //else if (item.ParentIsNfoOrDiscontinuedItem && !isNfoOrDisc)
            else if (item.ParentIsNfoOrDiscontinuedItem) //always capture demand for children of nfo (april19 change)
            //and childitem is not, then mark it as regular, otehrwise both PzT
            {
                ltsCode = null;
            }
        }
        else //parent
        {
            ltsCode = null;
            if (item.IsNfoItem || item.IsDiscontinuedItem)
            {
                //april 19 change
                //ltsCode = "PzT";
                ltsCode = null;
            }

        }

        return ltsCode;
    }
    -----------------

    ---- after ------
    public static string GetLtsCode(IventoryBinItem item)
    {
        bool isPzT = item != null
                  && !item.ParentItemNo.IsNullEmptyOrWhiteSpace()
                  && !item.ParentIsNfoOrDiscontinuedItem;
        return isPzT ? "PzT" : null;
    }
    ------------------

    ---- And later refactored to ----
    public static string GetLtsCode(IventoryBinItem item)
    {
        return IsPzT(item) ? "PzT" : null;
    }

    private static bool IsPzT(IventoryBinItem item)
    {
        return item != null
           && !item.ParentItemNo.IsNullEmptyOrWhiteSpace()
           && !item.ParentIsNfoOrDiscontinuedItem;
    }
    ------------------

Now let me be clear. The first code is crap. Major crap. I mean it's a really stinky brown and greasy pile of poop. It looks like it came out of a very sick dog.

I was astounded that anyone might think that crap was better than the rather nice refactoring. I guess some people just like to smell poop. I call them *Fecophiles*.

I thought that perhaps there was something wrong with their noses. After all, if you live in crap, you might just stop smelling it after awhile. So I determined to shove their noses in the fetid pile by investigating the smell in all it's rich and pungent detail. So I sent the following letter back to my friend.

<hr/>
Let's just walk through that previous code step by step and really *smell* it!

1.  We start out by initializing ltsCode to null. Fine.
2.  Then we encounter a double negative (if not null/empty) and an indirection (item.ParentItemNo). We need a comment to understand it. Apparently if the ParentItemNo of the item is null or empty then it means that the item has no parent. If we negate that, then the if statement is trying to say "if this is a child". The comment tries to point this out, but the grammar is bad. It would be nicer if the if statement were if(isChild(item)) or if (item.isChild()).
3.  In the body of the if statement we are a child, so we set the ltsCode to PzT.
4.  Ignore commented out code.
5.  Set the var isNfoOrDisc to an interesting expression. Oddly, I can't see any place where that variable is used. It *used* to be used, in the commented out code, so maybe someone just forgot to comment out that var too, eh?
6.  A negative if statement (it's easy to miss that bang).
7.  The if statement checks whether the parent is not nfo or discontinued. If so, sets ltsCode to PzT if . But ltsCode is *already* PzT. Maybe someone forgot to comment out this whole if statement too…. <bah!>
8.  More commented out code. ugh.
9.  Ah! Now in the else clause of the "parent Not nfo or Discontinue" if statement we check the boolean opposite. Does this programmer not know how if/else works? Why in hell is he checking a tautology. We already KNOW, in the else clause, that the parent is nfo or Discontinued. Sheesh.
10. And in that else clause, we set ltsCode to null. Wait… Wasn't it already null?
11. (pant, pant, pant) ok, we've reached an else. What else is it? It's the else from the double negative if statement in point 2 above. Ah, so this is where we have an item with no parent. That //parent comment is strange. Does it mean that the item is a parent? How would we know that. All we know is that the item does not have non-null or empty parent item number. So what's with the //parent comment? It's a lie, or at least a wild mistruth. I think that comment needs to be commented out. (grrrr).
12. We set the ltsCode to null. Hmmm. Let's see. What was the ltsCode be before we set it to null? Why, I believe it must be null already! Is there any pathway through this horrible rats nest of code that might set ltsCode to something other than null at this point? No!
13. and now the coup-de-gras! Another if statement that checks whether the item is nfo or Disconnected. And what does this if statement do if that's so? Why, it sets the ltsCode to … NULL. Of course! NULL. Brilliant!

Summary. This code is a mess. A whole-hearted unmitigated indisputable mess. It's full of inaccurate and ambiguous comments, useless variables, if statements that have no purpose, and some truly convoluted logic. And what was the goal? The goal was:

> return PzT if the item exists and has a parent that is not nfo or Disconnected. Otherwise return null.

OR, to put it in simple code:

       
     public static string GetLtsCode(IventoryBinItem item) {
      return IsPzT(item) ? "PzT" : null;
     }

     private static bool IsPzT(IventoryBinItem item) {
      return item != null
        && item.HasParent()
        && item.ParentIsNotnfoOrDiscontinuedItem;
     }

I am ashamed of anyone who thinks the original code is simpler and easier to understand.

<hr/>
I don't know if they got the message. But even if they did, and they could suddenly smell their home for the first time, it's a drop in the bucket. There are lots and lots of fecophiles out there who need to take a step back and inhale deeply through their noses.
