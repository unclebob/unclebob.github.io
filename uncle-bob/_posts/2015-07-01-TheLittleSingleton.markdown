---
layout: post
title: "The Little Singleton"
tags: ["Craftsmanship"]
---
<meta http-equiv="refresh" content="3; url=http://blog.8thlight.com/uncle-bob/2015/06/30/the-little-singleton.html" />
Do you recognize this:

    public class X {
      private static X instance = null;

      private X() {}

      public static X instance() {
        if (instance == null)
          instance = new X();
        return instance;
      }

      // more methods...
    }

>_Of course.  That's the Singleton pattern from the GOF book.  I've always heard we shouldn't use it._

Why shouldn't we use it?

>_Because it makes our systems hard to test._

It does?  Why is that?

>_Because you can't mock out a Singleton._

You can't?  Why not?

>_Well, because, the only class that can touch that private variable is the Singleton itself._

Do you know the rule about encapsulation and tests?

>_Uh, no.  What rule is that?_

Tests trump Encapsulation.

>_What does that mean?_

That means that tests win.  No test can be denied access to a variable simply to maintain encapsulation.

>_You mean that if a test needs access to a private variable..._

...the variable shouldn't be private.  Yes.  

>_That just doesn't sound right.  I mean, encapsulation is, er, important!_

Tests are more important.

>_Wait.  What?_

What good is encapsulated code if you can't test it?

>_OK, OK, but what does this have to do with testing singletons._

Look at this code.
    
    public class X {
      static X instance = null;

      private X() {}

      public static X instance() {
        if (instance == null)
          instance = new X();
        return instance;
      }

      // methods.
    }

    class TestX {
	  @Before
	  public setup() {
	    X.instance = new XMock();	
	  }
    }

    class XMock extends X {
	    // overide methods
    }

>_Oh, you made the instance variable "package" scope._

Right.  

>_And that allows you to mock the singleton._

Right.

>_And that means that singletons are easy to mock._

Right.  Now consider this:

    public class X {
      public static X instance = new X();

      private X() {}

      // methods.
    }
    
>_Wait!  Where did the instance method go?_

I don't need it.

>_Ah, the instance variable is public.  You can just use it directly._

Right.

>_But... But...  Someone might over-write it?_

Who would do that?

>_I dunno.  Uh.  Someone bad._

Do you have bad people on your team?

>_No.   But.   This just doesn't feel safe._

Well, if this were part of a public API, I'd agree with you.  But if this is just code that's used by our team then...

>_We trust our team?_

Of course.   

>_And this is pretty easy to mock, isn't it?_

Of course. 

>_So then I guess we could use Singleton if we wanted to._

Sure.  Although most of the time I don't want to.

>_After all this, and now you're telling you you don't want to use Singleton anyway?_

Well, I think it's important to understand why.

>_OK, so why don't you use Singleton?_

I do sometimes.  Especially in public APIs.  

>_You mean it's a trust issue again?_

Right.  In a public API if I want to ensure that only one instance is being created, then I'll use a Singleton.

>_OK, but then what if it's not in a public API, but you still just want one instance created?_

Well, then, I simply create one.




