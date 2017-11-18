---
layout: post
title: Two old programmers write a Binary Search
tags: ["Software"]
---
Andrew Koenig (ARK) wrote to me and said: 

>_"I have an idea for an essay, and wondered if you'd like to collaborate on it.  A simple case study, really: How would you use TDD to write a binary-search function?"_ 

"That sounds like fun." I replied; and began to write this article.  

Andy continued:

>_Sounds easy, but..._

>_Once upon a time, I had dinner with a fellow named Luther Woodrum, who had been introduced to me as IBM's leading export on sorting. He told me that he was in the habit of asking people in lectures, etc., to write a binary search, and almost no one could do it correctly, not even experienced professional programmers._

>_And yet it's obviously a simple concept. So the question is how to translate that simple concept into analogously simple code in a way that gives assurance that it actually works._

My first thought was to write the binary search in Java.  This simplifies a lot of the memory management and pointer arithmetic issues.  

You can follow along with [this](https://github.com/unclebob/BinarySearch) github project.

Since we know the algorithm already, we don't need to use TDD, or the [TPP](http://blog.cleancoder.com/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html) to derive it.  So we'll just write our tests to verify that the algorithm is correct.

We begin, as usual, with an empty test, just to get an execution environment working.

    package binarySearch;
    import org.junit.Test;

    public class BinarySearchTest {
      @Test
      public void nothing() throws Exception {
      }
    }

Next we'll write the test that forces us to create the class for the binary search.

    public class BinarySearchTest {
      @Test
      public void createSearcher() throws Exception {
        long array[] = new long[2];
        BinarySearcher searcher = new BinarySearcher(array);

      }
    }
    
----

    package binarySearch;

    public class BinarySearcher {
      public BinarySearcher(long[] array) {
      }
    }

As you can see, we've made the decision to include the array to be searched in the constructor of the `BinarySearcher` class.  

The next few tests make sure we properly handle an invalid input array.

    @Test(expected = BinarySearcher.InvalidArray.class)
    public void nullInputThrowsException() throws Exception {
      BinarySearcher searcher = new BinarySearcher(null);
    }

    @Test(expected = BinarySearcher.InvalidArray.class)
    public void zeroSizedArrayThrowsException() throws Exception {
      BinarySearcher searcher = new BinarySearcher(new long[0]);
    }

----

    public class BinarySearcher {
      public BinarySearcher(long[] array) {
        if (array == null || array.length == 0)
          throw new InvalidArray();
      }

      public class InvalidArray extends RuntimeException {
      }
    }

As we continue, you will note that we are following the principle of "Avoiding the gold".  We will write tests around the _outside_ of the problem first; making sure that all the validation and organization is working before we head into the actual problem of doing a binary search.

The next tests check that the input array can be verified as ordered.  We used this approach because the check is expensive.  Programmers who use this class may already know that their input is properly ordered.  Other programmers may need the check. 

>_That's what Dijkstra described as wearing flotation vests during training and then discarding them for the actual voyage..._ 

    @Test
    public void checkInOrder() throws Exception {
      long[][] inOrderArrays = { {0}, {0, 1}, {0, 1, 2, 3} };
      for (long[] inOrder : inOrderArrays) {
        searcher = new BinarySearcher(inOrder);
        searcher.validate();
      }
    }

    @Test
    public void outOfOrderThrowsException() throws Exception {
      long[][] outOfOrderArrays = { {1, 0}, {1, 0, 2}, {0, 2, 1}, {0, 1, 2, 4, 3} };
      int exceptions = 0;
      for (long[] outOfOrder : outOfOrderArrays) {
        searcher = new BinarySearcher(outOfOrder);
        try {
          searcher.validate();
        } catch (OutOfOrderArray e) {
          exceptions++;
        }
      }
      assertEquals(outOfOrderArrays.length, exceptions);
    }

----

    public class BinarySearcher {
      private long[] array;

      //...

      public void validate() {
        for (int i=0; i<array.length-1; i++)
          if (array[i] > array[i+1])
            throw new OutOfOrderArray();
      }

      //...

      public class OutOfOrderArray extends RuntimeException{
      }
    }

That concludes the perhiphery of the system.  Now we can start to go for the gold.  

One of the internal calculations that must be made for a binary search, is the calculation of the midpoint.  Given two positions in the array (`l` and `r`), this function must find the position that is the appropriate midpoint between them.  This is typically done with `floor((l+r)/2)`.

    @Test
    public void findsProperMidpoint() throws Exception {
      assertEquals(0, findMidpoint(0, 0));
      assertEquals(0, findMidpoint(0, 1));
      assertEquals(1, findMidpoint(0, 2));
      assertEquals(1, findMidpoint(0, 3));
      assertEquals(Integer.MAX_VALUE/2, findMidpoint(0,Integer.MAX_VALUE));
    }

----

    public static int findMidpoint(int l, int r) {
      return (l+r)>>1;
    }
    
This works so long as the sum `l+r` is `<` `Integer.MAX_VALUE`.  But for very large arrays that may not be the case.  So we should try those cases.

`Integer.MAX_VALUE` is the largest positive integer.  If you increment it you get a negative number.  So you have to be careful how you do the math.  The cases below explore that math.  Make sure you understand them.

    @Test
    public void findsProperMidpoint() throws Exception {
      assertEquals(0, findMidpoint(0,0));
      assertEquals(0, findMidpoint(0, 1));
      assertEquals(1, findMidpoint(0, 2));
      assertEquals(1, findMidpoint(0, 3));
      assertEquals(Integer.MAX_VALUE/2, findMidpoint(0,Integer.MAX_VALUE));
      assertEquals(Integer.MAX_VALUE/2+1, findMidpoint(1, Integer.MAX_VALUE));
      assertEquals(Integer.MAX_VALUE/2+1, findMidpoint(2, Integer.MAX_VALUE));
      assertEquals(Integer.MAX_VALUE/2+2, findMidpoint(3, Integer.MAX_VALUE));
      assertEquals(Integer.MAX_VALUE/2, findMidpoint(1, Integer.MAX_VALUE-2));
    }

The solution is to divide the two indexes by two _first_ and then add them together.  However, you also have to remember any possible carry from the low order bit, and add that in too.  Thus:

    public static int findMidpoint(int l, int r) {
      int carry = ((l&1)+(r&1))>>1;
      return (l>>1)+(r>>1)+carry;
    }

Now we can write some simply tests to check the search results.  

    private void assertFound(int target, long[] domain) {
      BinarySearcher searcher = new BinarySearcher(domain);
      assertTrue(searcher.find(target));
    }

    private void assertNotFound(int target, long[] domain) {
      BinarySearcher searcher = new BinarySearcher(domain);
      assertFalse(searcher.find(target));
    }

    @Test
    public void simpleFinds() throws Exception {
      assertFound(0, new long[]{0});
      assertFound(5, new long[]{0,1,5,7});
      assertFound(7, new long[]{0,1,5,7});

      assertNotFound(1, new long[]{0});
      assertNotFound(6, new long[]{1,2,5,7,9});
    }

We can test these tests by writing a simple linear search.

  public boolean find(int element) {
    for (int i=0; i<array.length; i++) {
      if (array[i] == element)
        return true;
    }
    return false;
  }
  
And now, finally, we can write the test that forces us to implement the binary search.  The test counts the number of comparisons and ensures that number is always O(Log2 N) or less.  We use a simple spy to count the comparisons.

      long[] makeArray(int n) {
        long[] array = new long[n];
        for (int i=0; i<n; i++)
          array[i] = i;
        return array;
      }

      private void assertCompares(int compares, int n) {
        long[] array = makeArray(n);
        BinarySearcherSpy spy = new BinarySearcherSpy(array);
        spy.find(0);
        assertTrue(spy.compares > 0);
        assertTrue(spy.compares <= compares);
      }

      @Test
      public void logNCheck() throws Exception {
        assertCompares(1,1);
        assertCompares(5,32);
        assertCompares(16,65536);
      }
    }

    class BinarySearcherSpy extends BinarySearcher {
      public int compares = 0;

      public BinarySearcherSpy(long[] array) {
        super(array);
      }

      protected boolean find(int l, int r, int element) {
        compares++;
        return super.find(l, r, element);
      }
    }
    
And, of course, the algorithm that passes these tests is:

    public boolean find(int element) {
      return find(0,array.length,element);
    }

    protected boolean find(int l, int r, int element) {
      if (l>=r)
        return false;
      int midpoint = findMidpoint(l,r);
      if (array[midpoint] == element)
        return true;
      else if (array[midpoint] < element)
        return find(midpoint+1, r, element);
      else
        return find(l, midpoint-1, element);
    }

I showed this to Andy, and he said: 

>_Why do you consider it an error to be asked to search a zero-length array?_

That was an arbitrary decision on my part.  Perhaps I was a bit hasty.  A zero length array is simply the most degenerate form.  By the rules of TDD, that should have been the first thing I tested. 

    @Test
    public void zeroSizedArrayFindsNothing() throws Exception {
      searcher = new BinarySearcher(new long[0]);
      assertFalse(searcher.find(1));
    }

>_I don't see any test that would detect a validate function that incorrectly fails on two adjacent equal elements._

Hmmm.  Good catch.  (See, this pair programming stuff really works. ;-)

    @Test
    public void checkInOrder() throws Exception {
      long[][] inOrderArrays = { {0}, {0, 1}, {0, 1, 2, 3}, {0,1,1,2,3 } };
      for (long[] inOrder : inOrderArrays) {
        searcher = new BinarySearcher(inOrder);
        searcher.validate();
      }
    }

>_`findsProperMidpoint` is too clever by half--in this case, literally. I would expect `Integer.MAX_VALUE` to be an odd number, which means that `Integer.MAX_VALUE/2` truncates away a half. It seems to me that `findsProperMidpoint` should require the midpoint to be exact if the bounds are an even number apart, but should permit rounding in either direction because either rounding will yield correct results. Otherwise, I think there's some overspecification going on._

Instead of rounding, I choose to use the `floor` of the midpoint simply because that's what the [wikipeadia](https://en.wikipedia.org/wiki/Binary_search_algorithm) article suggested. 

>_Is it true that if you increment `Integer.MAX_VALUE` (in Java) you get a negative number? Or do you get an overflow exception?_

Yes, at least empirically.  I'm not sure what the spec says; but my implementation went negative when I incremented `Integer.MAX_VALUE`.  

>_By the way, here's a much, much simpler implementation of findMidpoint:_

    public static int findMidpoint(int l, int r) {
        return l + (r - l)/2;
    }
    
`<blush>`  Um...  Yeah, that works fine.  `<blush>`

>_When I proposed the original problem of testing a binary search, I did not actually specify the interface to the binary search. I think that the most useful interface is probably something like the C++ lower_bound library function, augmented to return a boolean. That is, the result of a binary search might reasonably be a boolean that indicates whether the value sought is in the array, together with the index of the first element that is >= the element sought (or one off the end if there is no such element)._

Hmmm.  OK, that's an easy change (I think).

    private void assertFound(int target, int index, long[] domain) {
      BinarySearcher searcher = new BinarySearcher(domain);
      assertEquals(index, searcher.findLowerBound(target));
    }

    private void assertNotFound(int target, long[] domain) {
      BinarySearcher searcher = new BinarySearcher(domain);
      assertEquals(domain.length, searcher.findLowerBound(target));
    }

    @Test
    public void simpleFinds() throws Exception {
      assertFound(0, 0, new long[]{0});
      assertFound(5, 2, new long[]{0,1,5,7});
      assertFound(7, 3, new long[]{0,1,5,7});

      assertNotFound(1, new long[]{0});
      assertNotFound(6, new long[]{1,2,5,7,9});
    }

----

    public int findLowerBound(int element) {
      return findLowerBound(0,array.length,element);
    }

    protected int findLowerBound(int l, int r, int element) {
      if (l>=r)
        return array.length;
      int midpoint = findMidpoint(l,r);
      if (array[midpoint] == element)
        return midpoint;
      else if (array[midpoint] < element)
        return findLowerBound(midpoint+1, r, element);
      else
        return findLowerBound(l, midpoint-1, element);
    }

Now I wonder if we don't need some more test cases -- especially with duplicates in the input array.

    private void assertFound(int target, int index, long[] domain) {
      BinarySearcher searcher = new BinarySearcher(domain);
      assertTrue(searcher.find(target));
      assertEquals(index, searcher.findLowerBound(target));
    }

    private void assertNotFound(int target, int index, long[] domain) {
      BinarySearcher searcher = new BinarySearcher(domain);
      assertFalse(searcher.find(target));
      assertEquals(index, searcher.findLowerBound(target));
    }

    @Test
    public void simpleFinds() throws Exception {
      assertFound(0, 0, new long[]{0});
      assertFound(5, 2, new long[]{0,1,5,7});
      assertFound(7, 3, new long[]{0,1,5,7});
      assertFound(7, 5, new long[]{0,1,2,2,3,7,8});
      assertFound(2, 2, new long[]{0,1,2,2,3,7,8});
      assertFound(2, 2, new long[]{0,1,2,2,2,3,7,8});

      assertNotFound(1, 1, new long[]{0});
      assertNotFound(6, 3, new long[]{1,2,5,7,9});
      assertNotFound(0, 0, new long[]{1,2,2,5,7,9});
      assertNotFound(10, 6, new long[]{1,2,2,5,7,9});
    }
    
Yeah, that fails.  OK, let's fix the algorithm a bit...

    public boolean find(int element) {
      int lowerBound = findLowerBound(element);
      return lowerBound < array.length && array[lowerBound] == element;
    }

    public int findLowerBound(int element) {
      return findLowerBound(0,array.length,element);
    }

    protected int findLowerBound(int l, int r, int element) {
      if (l==r)
        return l;
      int midpoint = findMidpoint(l,r);
      if (element > array[midpoint])
        return findLowerBound(midpoint+1, r, element);
      else
        return findLowerBound(l, midpoint, element);
    }
    
The searches all work, but the O(log n) test is failing.  That's because there are a couple of extra compares now.  I should adjust the test. 

    private void assertCompares(int compares, int n) {
      long[] array = makeArray(n);
      BinarySearcherSpy spy = new BinarySearcherSpy(array);
      spy.findLowerBound(0);
      assertTrue(spy.compares > 0);
      assertTrue(""+spy.compares ,spy.compares <= compares+2);
    }
    
Whew!  OK, everything works again.  Algorithm's a bit pretter too.  The tests could use a bit of condensing; I don't think that's the minimal set. But let's leave that for awhile.  I think there's more coming.

>_I think it's interesting that you chose a recursive implementation of binary search, because that turns what could have been an algorithm that runs in O(log N) time and O(1) space into one that runs in O(log N) time and O(log N) space._

I chose recursion because it made it simple to write the spy to test O(log N).  However, I was a bit careless since the algorithm can easily be turned into tail recursion.

    protected int findLowerBound(int l, int r, int element) {
      if (l==r)
        return l;
      int midpoint = findMidpoint(l,r);
      if (element > array[midpoint])
        l = midpoint+1;
      else
        r = midpoint;

      return findLowerBound(l, r, element);
    }

Sadly, Java does not (yet) support tail call optimization.

>_Which brings up (again) the question of how one goes about testing code that has performance requirements._

Storage is a tough one for simple unit tests like this.  We could spy on the heap calls.  We could also zero out the stack, and inspect it after the calls to check stack depth.  (Maybe the guys at Toyota wish they'd done that a bit more carefully.) 

As for execution time, our spy has managed to measure that for us by counting the number of loops (recursions).  However, there are costs.  The `findLowerBound` function is polymorphically deployed -- and must be in order for the spy to work.  Polymorphic deployment requires a nanosecond or two.  Tests are observers, and so there is some "observer effect" going on here.  

In order for our code to be testable we need to tolerate a certain amount of space and time overhead. We can work to minimize it; but eliminating it entirely would likely require extra hardware (like an emulator).   

>_So...let me suggest another strategy. I'm just going to talk about the innermost part of the search, and I want to implement it as a loop, not as a recursion. Also, as a long-time C programmer, I shall express the range asymmetrically, so I'll use `begin` and `end` as indices rather than `l` and `r`._

>_The essence of a binary search is that if the element we're seeking is in the array at all, it is in the range `[begin, end)`, so we can use that as a loop invariant. And then we can write:_

    while (begin != end) {
        mid = findMidpoint(begin, end);
        if (array[mid] < element)
            begin = mid + 1;
        else
            end = mid;
    }

>_Here, the asymmetry of the assignments reflect the asymmetry of the loop bounds. When we're done, `begin` and `end` are equal, and now, there are three cases:_

>_1) `end` is unchanged from its initial value, in which case every array element < the value sought._

>_2) The value sought < `array[end]`, in which case the value is not in the array and `array[end]` is the leftmost element greater than the value sought, or_

>_3) the value sought == `array[end]`. This is the only case in which a second comparison is necessary._

This is the beginnings of a Dijkstra style proof.  Such a case analysis proof is feasible with a small function like this.  But as the state space grows...