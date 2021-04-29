---
layout: post
title: "The MODE-B Imperative"
tags: ["Craftsmanship"]
---
If you follow my [twitter](https://twitter.com/unclebobmartin), [facebook](https://www.facebook.com/robertcecilmartin), or [github](https://github.com/unclebob?tab=repositories) feeds, you may have noticed that I've been writing a PDP-8 Emulator for the iPad.  

Here's a screenshot:

<img src="/assets/ModeAvsModeB/pdp8Shot.jpg"/>

My purpose in writing this emulator (other than sheer nostalgia) is to use it as a training tool for new programmers.  I think every new programmer should spend a week or two programming one of these old machines.  It seems to me that there is no better way to understand what a computer really is, than to touch a real computer and program it at the bit level, in machine language.  Once you have done that, all the magic disappears and is replaced with cold, hard reality.  And, let me tell you, programming a PDP-8 is _cold, hard, reality._  Oh boy, is it ever!

I've tried to be faithful to the machine and it's environment.  The front panel is a decent abstract representation of the original PDP8, and the lights blink appropriately, and with the correct data. (though I couldn't resist making the lights touch sensitive, like the ECP-18).

<img src="/assets/ModeAvsModeB/pdp8i_frontpanel.jpg"/>

The paper tapes in the reader and punch move at appropriate speeds, and the holes represent the true data.  They also make the right kinds of noises.  The teletype prints at the appropriate speed (though you can speed it up if you want (you will)) and behaves very much like an ASR-33, making all the appropriate noises, and responding properly to carriage return and line feed characters, etc. (Yes, you can overprint!)

I found some binary images of old PDP8 paper tapes [here](http://dustyoldcomputers.com/pdp-common/reference/papertapes/dec-08.html), and managed to get them into my emulator by munging their format with a little C program I wrote, and then transporting them to the iPad using Dropbox.  The results have been both satisfying and heart-wrenching.  That ancient code works!

I sit in front of my half-pound, $600 iPad, in its nice little case, and its bluetooth keyboard, surrounded by programming manuals, and marked up listings of a program I am working on.  And I realize that I'm using code written half a century ago by men and women (probably a lot of women back then) who hauled themselves up by their bootstraps to get that silly little machine working. A machine that weighed 500lbs, was the size of a refrigerator, and cost $20,000 in 1967.

Could those men and women ever have guessed that their code would be running in a hand-held tablet computer and would be used to train programmers in the twenty-first century?  Some -- many -- must still be alive.  I wonder what they'd think if they knew.

<img src="/assets/ModeAvsModeB/pdp8InKitchen.jpg"/> 

My emulator is written in Lua, using the [Codea](http://twolivesleft.com/Codea/) framework for the iPad.  This is a hugely convenient language for iPad development.  Lua is fast (enough), and Codea has wonderful graphics primitives, and a simple, yet very effective graphics framework for developing highly interactive animated programs.  

This made the animation (and sound generation) of the front panel, the teletype, and the paper tape reader/punch a snap.  

Emulating the PDP8 internals was a bit of a challenge since Lua only has one numeric type: floating point.  Doing 12-bit logic using floating point math is, uh, _interesting._  On the other hand, I get a huge kick out of watching `FOCAL` (FORmula CALculator: a language similar to Basic) run on my PDP-8, and do it's own floating point math, using the logical operations that I concocted from Lua's floating point math. `<grin>`  You should see those lights blink!
	
The execution speed is about 4,000 instructions per second.  While that's 1/7th the speed of a PDP8/S, it's pretty impressive for an iPad running a "byte-code interpreted" language like Lua, emulating 12 bit logic using floating point math!  I wasn't expecting that kind of performance.  It actually runs all that old DEC software at reasonable speed.  Even `FOCAL` runs fast enough to compute square roots in half a second or so.  

And, again, the blinking of the lights during a compile is deeply satisfying.  Watch that and you'll know where all those 1950s sci-fi movies got their ideas from.  
	
##MODE-B
Getting the Emulator working was really very easy.  I've probably invested 30 hours in it overall; and that includes learning Codea and Lua.

The development process was lickety-split.  Codea's Lua editor for the iPad is intuitive and powerful (though it has no refactorings `<sob>`).  The edit/test loop was, perhaps, 10 seconds long.  I could add a line or two of code, run the app, see the effect, and then hop back into the editor just like that.  It was a satisfying treat, and I had a ball doing it.
	
Of course I wrote tests for the tricky bits.  I wrote a little test framework just for that purpose, and I put a `TEST` button on the front panel of the emulator to make it easy for me to run those tests.  The emulation code itself would have been nearly impossible if I hadn't had unit tests.  And of course, I used the TDD discipline for that code.  In the end, there were over 100 tests for the various instructions and behaviors of the PDP-8.

For the GUI, on the other hand (and there's a lot of GUI code), tests were unnecessary (`<GASP>`).  My eyes were the tests.  I knew what I wanted to see, and so I spun around the edit/test loop every 10 seconds.  Writing tests in a TDD style would have been horrifically difficult, and an utter waste of time.

On the other hand, I was still using the TDD _rhythm_.  I knew what I wanted to see on the screen.  That was my test.  I simply modified the code until that test passed.  So even though I wasn't writing tests, it felt like I was -- _it felt like TDD_. 

It's true, of course, that I don't have automated regression tests for my GUI.  On the other hand, it's absurdly simple to ensure that everything is working.  So far the lack of automated GUI tests has not impacted my loop time.

Of course without refactoring tools the code got a bit messy.  I refactored when I couldn't stand it anymore; but the backlog of messiness is larger than I'd like.  I'll continue to clean that code over time; but it's much slower going without good refactoring tools.

Anyway, for the sake of this article, let's call this style of programming: `MODE-B.`  `MODE-B` is the style that allows you to edit on the screen, and see the results either on the screen, or in a passing test, in seconds.  It's a hyper-speed development loop that doesn't require listings, pencils, compile time, setup time, or any other impediment.  The time between editing the code, and seeing it run, is much less than one minute.  

##MODE-A
Having gotten the PDP8 Emulator to work.  And having gotten all the old tools, like the paper tape editor, and the Pal-3 assembler, up and running.  I set about to write a simple program.  This program would allow the user to type a simple formula on the keyboard, and then it would print the result.  For example, if the user typed: `25+32`, the computer would print `57`.  

On a PDP-8, this is a non-trivial program.  I've included it below for those of you who want to see how a pretty poor PDP-8 programmer has written it.

The process was the same as the process I used back in the late '70s when working on assembly language programs on a Teradyne `M365` (An 18 bit relative of a PDP8).  We had magnetic tape, instead of paper tape; and the computer was a bit more powerful than a PDP-8.  But the process was still the same.  It goes like this:  

Assume that you are in the middle of writing the program below.  You've already got some of it written, and you are adding to it.  Remember, this computer only has 4K words.  It can't store much in it's memory.  Remember, also, that the only mass storage you have is paper tape.  So your source code is on a single long paper tape.  

 1. Write the changes you want to make to your program on the current listing.  You'll have changes on many pages, so put paper clips in those pages if the listing is long.  
 2. Load the editor from paper tape.  This will take a few minutes so get some coffee.
 3. Set the front panel switches to `6003`: to compress spaces, and use the high speed reader/punch.  Run the editor (by toggling `0200` into the PC register and hitting the `RUN` button)
 4. Put your source code paper tape into the reader.  
 5. Read in one "page" of code from paper tape using the `R` command.  (50 lines or less. 1 minute or so).
 6. Go to that page in your listing and make any changes using the `I`, `C`, and `D` commands.  Remember you don't have a screen, so you are editing line by line, using line numbers.  Plan to spend some time at this.
 7. Print out the current page using the `L` command.  Make sure all your changes are correct.
 8. Punch the current page to paper tape using the `P` command. (a minute or so).
 9. Punch the current page and read in the next with the `N` command and if that wasn't the last page, go to step 6.  
10. Remove the new source tape from the punch and label it with a title and a version number.  _Don't ever forget the version number!_ 
11. Load the assembler into memory from paper tape (10 minutes or so).
12. Set the front panel switches to `2002`: the "pass one, output to printer" configuration.
13. Load your source tape into the reader.
14. Load `0200` into the PC register, and hit `RUN`.
15. Pass one compile will read your whole source tape and then print your symbol table.  (10 minutes or so)
16. After the computer halts, set the front panel switches to `4003`: the "pass two, output to punch" configuration.
17. Load your source tape into the reader.
18. Push `RUN`.  Pass two compile will read your whole source tape and punch your binary paper tape.  (15 min or so).  Source code errors will print during this pass.  
19. After the computer halts, if there are errors, throw away the paper tape that was punched and go to step 1.  Otherwise remove the binary tape from the punch and label it with a title and version number. (I don't need to remind you about that version number, right?)
20. Set the front panel switches to `6002`: the "pass three, output to printer" configuration.
21. Load your source tape into the reader.
22. Push `RUN`.  Pass three compile will read your whole source tape and print your program listing.  You'll need this for debugging, so don't neglect it.  (30 min or so because the printer is very slow.)  Make sure you have enough paper in the printer!
23. Tear off the listing and check it over.
24. Put your binary tape into the reader.
25. Set the PC register to `7777` (the address of the bin loader which is usually kept in core memory) and hit `RUN`.  If the bin loader is not in memory for some reason, then you'll have to toggle in the `RIM` loader and then load the bin loader paper tape before doing this step.
26. When the computer halts, your program has been loaded into memory.  Run it, and see if it works.

This process is highly abbreviated.  There are lots of littler steps in there, but you get the idea.  

This is `MODE-A`.  It's a very fragile, error-prone process that takes an hour or so to execute.  It could be a lot more for a large-ish program.  A very small program might make it around the loop in 15 minutes.  The program I was writing grew to be about 20-30 minute or so, and I cheated by allowing my "teletype" to run at 10X the normal rate.  

To get my silly little program to work, I went around that loop seven times.  It took me about a week, and about five hours total.  A lot of that time was _writing the code with a pencil_ because, without a screen editor, there was just no way to avoid hand writing, and using a lot of eraser.  

Back in the '70s I spent days, weeks, and years working in `MODE-A`.  All programmers did.  That was what programming was back then.

And here's the thing about `MODE-A`: YOU ARE CAREFUL.  Every mistake costs you an hour or so.  So you spend a lot of time going over the details, making sure your code is right; that you edited it correctly; that the switches are set correctly; that the tapes are labelled correctly; etc.  

In `MODE-A` you take _nothing_ for granted.  You do everything _deliberately_ and _carefully_.  Because that's the only way to go fast. (If "fast" is the word.)

Let's call this carefulness and deliberateness: `MODE-A` behavior.

##`MODE-A` vs `MODE-B`
`MODE-A` is a _lot_ slower than `MODE-B`.  The loop time is impossibly large, and the amount you can get done in each loop is ridiculously small.  For example, my first loop through this process was to write, and debug, the subroutine that read in a line of text from the keyboard, terminated with a CR (Carriage Return...  Yes, the teletype had a "carriage", or rather a "print head" that could be "returned".)  

`MODE-B` is fast!  Really, really, fast.  The time through the loop is very short, and you can get a lot done in each loop.  For example, it only took me a few loops around to get the paper tape animation through the reader and punch to work correctly.  Every PDP-8 machine instruction took a loop or two.  Getting the scrolling of the TTY paper took two or three loops.  

And, of course, I wasn't using listings.  I didn't write the code on paper first.  I could go anywhere in the program I wanted and edit any line I wanted in a flash.  I had syntax highlighting, automatic indenting, search and replace, scrolling, tabs, and on-line documentation.  

`MODE-B` is fast!

##The `MODE-B` Imperative!
So then why do so many programmers still work in `MODE-A`?  They do, you know.  They pile mess upon mess, and framework upon framework, until their loop time grows from seconds to minutes and longer.  They inject so many dependencies that the builds become fragile and error-prone.  They create so many unisolated external dependencies that they might as well be using paper tape.  Why would anybody do _anything_ that increased their loop time?  Why wouldn't everyone _defend their loop time with their lives_?

Isn't avoiding `MODE-A` a guilt-edged priority?  Shouldn't we all do _everything we possibly can_ to keep our development cycle in `MODE-B`?  Isn't `MODE-B` an _imperative_?

Do you want to know the secret for staying in `MODE-B`?  I know what it is.  I'll tell you.  

>The secret for staying in `MODE-B` is to use `MODE-A` behavior.  

----
##HOLY STRUCTURED METHODOLOGY BATMAN!

I just discovered who wrote the PAL III assembler for the PDP-8.  Hold on to your hats.  It was [Ed Yourdon](http://en.wikipedia.org/wiki/Edward_Yourdon).  

----

PDP8 Program to accept two numbers and a single operator, and print the result.

                *20
    0020  7563  MCR,    -215
    0021  0212  KLF,    212
    0022  7540  MSPC,   -240
    0023  7520  MZERO,  -260
    0024  7766  M10,    -12
    0025  0276  PROMPT, 276 />
    0026  0215  KCR,    215
    0027  7525  MPLUS,  -253
    0030  7523  MMINUS, -255
    0031  0277  QMARK,  277
    0032  0260  KZERO,  260
            
                /WORKING STORAGE
    0033  0000  REM,    0
            
                /CALL SUBROUTINE IN ARG
    0034  0000  CALL,   0
    0035  3046          DCA AC
    0036  1434          TAD I CALL
    0037  3047          DCA CALLEE
    0040  1034          TAD CALL
    0041  7001          IAC
    0042  3447          DCA I CALLEE
    0043  2047          ISZ CALLEE
    0044  1046          TAD AC
    0045  5447          JMP I CALLEE
    0046  0000  AC,     0
    0047  0000  CALLEE, 0

    ----------------

                *200
                /CALC A+B OR A-B
                /MAIN LOOP: PROMPT, GET CMD, PRINT RESLT
                
    0200  6046          TLS
    0201  7200  IDLE,   CLA
    0202  1026          TAD KCR
    0203  4034          JMS CALL
    0204  0425          PRTCHAR
    0205  7200          CLA
    0206  1025          TAD PROMPT
    0207  4034          JMS CALL
    0210  0425          PRTCHAR
    0211  4034          JMS CALL
    0212  0400          RDBUF
    0213  2000          BUF
    0214  4034          JMS CALL
    0215  0462          SKPSPC
    0216  2000          BUF
    0217  3222          DCA .+3
    0220  4034          JMS CALL
    0221  0477          GETNUM
    0222  0000          0
    0223  3261          DCA A
    0224  1622          TAD I .-2
    0225  3263          DCA OP
    0226  1222          TAD .-4
    0227  7001          IAC
    0230  3233          DCA .+3
    0231  4034          JMS CALL
    0232  0477          GETNUM
    0233  0000          0
    0234  3262          DCA B
    0235  1263          TAD OP
    0236  1027          TAD MPLUS
    0237  7650          SNA CLA
    0240  5254          JMP ADD
    0241  1263          TAD OP
    0242  1030          TAD MMINUS
    0243  7650          SNA CLA
    0244  5251          JMP SUB
    0245  1031          TAD QMARK
    0246  4034          JMS CALL
    0247  0425          PRTCHAR
    0250  5201          JMP IDLE
                
    0251  1262  SUB,    TAD B
    0252  7041          CIA
    0253  7410          SKP
    0254  1262  ADD,    TAD B
    0255  1261          TAD A
    0256  4034          JMS CALL
    0257  0600          PRTNUM
    0260  5201          JMP IDLE
                
    0261  0000  A,      0
    0262  0000  B,      0
    0263  0000  OP,     0

    ----------------

                *400
                /READ A BUFFER UP TO A CR
    0400  0000  RDBUF,  0
    0401  7200          CLA
    0402  1600          TAD I RDBUF
    0403  2200          ISZ RDBUF
    0404  3215          DCA BUFPTR
    0405  4216  RDNXT,  JMS RDCHAR
    0406  3615          DCA I BUFPTR
    0407  1615          TAD I BUFPTR
    0410  1020          TAD MCR
    0411  7450          SNA
    0412  5600          JMP I RDBUF
    0413  2215          ISZ BUFPTR
    0414  5205          JMP RDNXT
    0415  0000  BUFPTR, 0
                
                /READ ONE CHAR
    0416  0000  RDCHAR, 0
    0417  7200          CLA
    0420  6031          KSF
    0421  5220          JMP .-1
    0422  6036          KRB
    0423  4225          JMS PRTCHAR
    0424  5616          JMP     I RDCHAR
                
                /PRINT ONE CHAR
    0425  0000  PRTCHAR,0
    0426  6041          TSF
    0427  5226          JMP .-1
    0430  6046          TLS
    0431  3245          DCA CH
    0432  1245          TAD CH
    0433  1020          TAD MCR
    0434  7440          SZA
    0435  5242          JMP RETCHR
    0436  1021          TAD KLF
    0437  6041          TSF
    0440  5237          JMP .-1
    0441  6046          TLS
    0442  7200  RETCHR, CLA
    0443  1245          TAD CH
    0444  5625          JMP I PRTCHAR
    0445  0000  CH,     0
                
                
                /PRT A BUFFER
    0446  0000  PRTBUF, 0
    0447  7200          CLA
    0450  1646          TAD I PRTBUF
    0451  2246          ISZ PRTBUF
    0452  3215          DCA BUFPTR
    0453  1615  PRTNXT, TAD I BUFPTR
    0454  4225          JMS PRTCHAR
    0455  2215          ISZ BUFPTR
    0456  1020          TAD MCR
    0457  7640          SZA CLA
    0460  5253          JMP PRTNXT
    0461  5646          JMP I PRTBUF

    ----------------

                /SKIP SPACES AC= FIRST NON-SPACE
    0462  0000  SKPSPC, 0
    0463  7200          CLA
    0464  1662          TAD I SKPSPC
    0465  2262          ISZ SKPSPC
    0466  3215          DCA BUFPTR
                            
    0467  1615  NXTCHR, TAD I BUFPTR
    0470  2215          ISZ BUFPTR
    0471  1022          TAD MSPC
    0472  7650          SNA CLA
    0473  5267          JMP NXTCHR
    0474  7240          CLA CMA
    0475  1215          TAD BUFPTR
    0476  5662          JMP I SKPSPC
                
                /GET DECIMAL NUMBER
    0477  0000  GETNUM, 0
    0500  7200          CLA
    0501  3335          DCA NUMBER
    0502  1677          TAD I GETNUM
    0503  3215          DCA BUFPTR
                        
    0504  1615  NXTDIG, TAD I BUFPTR
    0505  1023          TAD MZERO
    0506  3334          DCA DIGIT
    0507  1334          TAD DIGIT
    0510  7710          SPA CLA
    0511  5327          JMP NONUM
    0512  1024          TAD M10
    0513  1334          TAD DIGIT
    0514  7700          SMA CLA
    0515  5327          JMP NONUM
    0516  1335          TAD NUMBER
    0517  7100          CLL
    0520  7006          RTL
    0521  1335          TAD NUMBER
    0522  7004          RAL
    0523  1334          TAD DIGIT
    0524  3335          DCA NUMBER
    0525  2215          ISZ BUFPTR
    0526  5304          JMP NXTDIG
    0527  1215  NONUM,  TAD BUFPTR
    0530  3677          DCA I GETNUM
    0531  2277          ISZ GETNUM
    0532  1335          TAD NUMBER
    0533  5677          JMP I GETNUM
    0534  0000  DIGIT,  0
    0535  0000  NUMBER, 0

    ----------------

                /DIVIDE AC BY ARG
                /Q IN AC, R IN REM
    0536  0000  DIV,    0
    0537  3033          DCA REM
    0540  1736          TAD I DIV
    0541  2336          ISZ DIV
    0542  7041          CIA
    0543  3361          DCA MDVSOR
    0544  3362          DCA QUOTNT
    0545  1033          TAD REM
    0546  1361  DIVLUP, TAD MDVSOR
    0547  7510          SPA
    0550  5353          JMP DIVDUN
    0551  2362          ISZ QUOTNT
    0552  5346          JMP DIVLUP
    0553  7041  DIVDUN, CIA
    0554  1361          TAD MDVSOR
    0555  7041          CIA
    0556  3033          DCA REM
    0557  1362          TAD QUOTNT
    0560  5736          JMP I DIV
    0561  0000  MDVSOR, 0
    0562  0000  QUOTNT, 0

    ----------------

                        *600
                        /PRINT NUMBER IN DECIMAL
                        DECIMAL
    0600  0000  PRTNUM, 0
    0601  4034          JMS CALL
    0602  0536          DIV
    0603  1750          1000
    0604  4225          JMS PRTDIG
    0605  7200          CLA
    0606  1033          TAD REM
    0607  4034          JMS CALL
    0610  0536          DIV
    0611  0144          100
    0612  4225          JMS PRTDIG
    0613  7200          CLA
    0614  1033          TAD REM
    0615  4034          JMS CALL
    0616  0536          DIV
    0617  0012          10
    0620  4225          JMS PRTDIG
    0621  7200          CLA
    0622  1033          TAD REM
    0623  4225          JMS PRTDIG
    0624  5600          JMP I PRTNUM
                
                /PRINT A DIGIT IN AC
                        OCTAL
    0625  0000  PRTDIG, 0
    0626  1032          TAD KZERO
    0627  4034          JMS CALL
    0630  0425          PRTCHAR
    0631  5625          JMP I PRTDIG

    ----------------

                *2000
    2000  0000  BUF,0

 





   
    