---
layout: post
title: "The First Micro-service Architecture"
tags: ["Craftsmanship"]
---
In the early 1960s IBM created the [1401 computer](http://ibm-1401.info/1401GuidePosterV9.html).  It was wildly popular and exceeded all revenue projections.  IBM built 2,800 of them in the first year (1960), and built over 15,000 of them overall.  They typically rented for $7,000 per month (~$42,000 in todays dollars) and would have sold (if selling had been an option) for several million dollars.  

The 1401 had a clock rate of around 88khz, and as much as 16,000 bytes of core memory.  Not 16,384, mind you, _16,000_.  The machine addressed it's memory in _decimal_.  Some installations had magnetic tape drives; but many had nothing more than a card reader, card punch, and a line printer.   The card reader could read 800 cards per minute.  The punch could punch 250 cards per minute.  The line printer could print 600 lines per minute.

A 1401 with 8,000 bytes, and no tape drive, could run the FORTRAN compiler.  The compiler would read in your FORTRAN program from the card reader, and then punch a self-loading binary executable deck of cards.   

If you had 8000 bytes, and had to write a FORTRAN compiler, how would you do it?  Remember that compilers generally have to make multiple passes over the source code in order to do their work.  So how would you structure the program so that it could pass over the code several times?

### Typical Multi-pass Compiler

The obvious choice might be to load the whole compiler into memory, leaving just enough space for some working storage for the symbol table and IO buffers.  Thus, if you had a three pass compiler, then on pass one, you would read in the FORTRAN source code, and then punch an intermediate deck that was (hopefully) somewhat smaller than the first deck.  Then you'd start pass two, and read in the intermediate deck, and punch another (hopefully smaller) intermediate deck.  Finally, you'd start pass three, and read in the final intermediate deck in order to punch the loadable binary deck.  ...And that's if there were just three passes.  What if there were 5? or 10?

Think of the limitations of this system.  How big is the compiler?  How much space do you need in order to hold the symbol table?  How many cards (80 bytes each) can you read in to your input buffers.  What size output buffer do you need to punch the intermediate cards?  And how much _time_ is going to be spent by the operator, shuttling cards from the punch back to the reader, over and over again, for each pass?  

Let's say you've got a thousand line FORTRAN program.  How long is pass 1?  Let's assume that the intermediate deck is 80% the size of the source deck.  So you've got to punch 800 cards at 250 per minute.  You also have to read 1000 cards at 800 per minute.  You can do that math.  It's just over 4 minutes of IO time.  And remember, with an 88khz clock rate, computation time was likely significant.  Each pass would be a bit shorter than the one before it; but given operator time, and the possibility of a handing error (dropping the cards) we can estimate that our 1,000 line FORTRAN program would require several minutes per pass, keeping the operator busy the whole time.  A five pass compilation might require an hour of dedicated operator time; and a lot of wasted intermediate cards.  Not to mention wear and tear on the card punch.

### Microservices

But that's not what the plucky folks at IBM did.  They used an entirely different approach.  They read the source code in _once_, and held the entire source program in memory!  They made _Sixty three passes_ over the source code; and they did that by using _microservices_.  They called them _phases_. 

Almost all the memory in the computer was used to store the source code.  They held back a few hundred bytes for the executable phases. If you get rid of all the extra blanks, the comments, and other non-essentials, you can hold a pretty big program in 7000+ bytes.  

Each compiler phase averaged 150 instructions.  Each was read in from the card reader (or mag tape if you had it).  Each phase would make a pass over the source code, replacing that source code with smaller intermediate results.  So from phase to phase, as the source code took up ever less space, there was ever more working storage for symbol tables and variables.  When each phase was complete, the next phase would be read in from the card reader (or tape) and executed.  The final phase punched the output result (or wrote it to tape).  

What did these phases do?  You can read about the details [here](http://ibm-1401.info/1401-IBM-Systems-Journal-FORTRAN.html).  In short, these phases swept through the source code, reorganizing it, eliminating redundancy, shortening keywords, replacing variables with addresses, replacing function and subroutine references with addresses, and gradually, inexorably grinding that source code down into binary code.  

I have just one word for that.  _Incredible._  

Imagine breaking down the problem of compilation into 62 different programs, each of which can run only once.  Each of which must run in the memory vacated by the previous program.  Each of which must consume the output of the previous, and prepare the input to the next.

Of course these little programs were microservices, and the compiler used a microservice architecture -- in 1960.  

Which just goes to show that there's nothing new under the Sun.  


