---
layout: post
title: "The More Things Change..."
tags: ["Programming"]
---

In 1984 I was the technical lead on a telephone controlled voice response system called the _Craft Dispatch System_. The purpose of CDS was to allow telephone repair craftsmen to call in and get their next repair job; without having to interact with a human.  

The craftsperson, using standard touch tone (DTMF) buttons, would log in to CDS and ask for their next job.  CDS would recognize their ID, inspect their job queue, and then read the next job to them.  It would tell them the address, the complaint, the most recent status of the telephone line, and any recent and relevant history on that line.  

CDS also allowed the craftsmen to run electronic tests on the phone line and get the results spoken back to them within a few seconds.  To achieve this, CDS communicated directly with 4-TEL, our telephone test system.  

Finally, CDS allowed the craft person to close out the repair by entering the new status of the line and any recommendations or comments they might have.  They could even leave a voice message for the dispatcher if they thought it necessary.

We designed and built all the hardware for this system.  The processor was a 80286 with one megabyte of RAM.  We had an ST412 hard drive with a capacity of 10MB.  It held all our voice files, program data, and executables.  

Our telephone interface card could detect ringing and DTMF; could answer and hang up; and could emit DTMF tones if it needed to dial.  It could play back and record telephone audio; which was encoded using a one bit CVSD codec that allowed us to hold five minutes of voice per megabyte.  

Our operating system was Digital Research's MP/M; which was a multiprocessing variant of CP/M.  It could run several processes at the same time.  Those processes were usually started from the command line.

### Radical

I was responsible for the architecture of the system.  I thought long and hard about how to structure this beast; having built several other voice response systems over the previous five years.  Most of those older systems were compiled and linked into single executables.  But not CDS. For CDS I had a _radical_ new idea.

The database for the system was kept in RAM, and written to disk at critical moments.  It was a simple name/value pair data store.  We called it the 3DBB (Remember the old Mr. Wizard cartoon with the "Three Dimensional Black Board"?)  3DBB was fronted by a process that ran continuously.  Other processes would request values by passing messages to the 3DBB process.  The 3DBB would respond with the appropriate value.  

The values held in the 3DBB were a special format that I called FLD for "Field Labeled Data".  Nowadays we'd call this JSON or XML.  It had it's own text encoding scheme that, quite by accident, looked a _lot_ like JSON.  However, FLDs were actually binary tree structures that were only converted to text if they needed to be displayed for some reason -- usually for debugging purposes.

The operation of the system was a big finite state machine.  Every input from a craftsperson in the field, every result of a processing a job, and every outcome of communicating with an external system was an event to that FSM. The actions of the FSM were _command lines_.  

The FSM state was stored in the 3DBB.  The FSM logic was described as a state transition table kept in a text file.  The FSM process would read that file and convert it into a table in RAM.  Then it would accept events from message queues, look up the appropriate transitions in the table, and respond by changing the state in the 3DBB and invoking the appropriate command line action. 

We had three telephone interface cards, allowing us to listen at three phone lines at once.  So we had three FSM processes running simultaneously.  Each of these processes would accept events from the phone, or from other processes, and invoke appropriate command line tasks based on the state of the machine.

There were many different command line actions that the FSM could invoke.  One was login.  Another was to fetch a job and read it back.  Still another was to start a test, and another was to read test results back.  All in all there were over a dozen different command line actions that were driven by this finite state machine.

The command line processes were invoked with a session ID on the command line itelf.  This allowed them to go to the 3DBB and pull the session FLD to discover what was going on.  Then it would do it's job and accept inputs from the user.  When it had completed it's job it would update the session in the 3DBB, would send the next event to it's FSM, and then terminate.  That next event would depend on the outcome of it's job, and the input received from the user.

Nice huh?  Radical.  

### Back to the Future.

If I were to describe CDS in today's terminology it would sound like this:

>_We used a Micro-Service Architecture, communicating through a message buss, driven by Business Process Model (BPM) interpreted by a BPEL engine, backed by a NOSQL database holding values in JSON._

Which leads me to suggest the following:

> _The more things change, the more they stay the same._

