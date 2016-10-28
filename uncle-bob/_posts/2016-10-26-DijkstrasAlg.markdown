---
layout: post
title: Dijkstra's Algorithm
tags: ["Software"]
---
I was at [SCNA](http://scna.softwarecraftsmanship.org/) the other day, and someone approached me about TDD and Dijkstra's algorithm.  He wondered if you could find a sequence of tests that would lead you to the algorithm.  This sounded like a fun little exercise, so I decided to give it a try here. 

I'll begin as usual with an executing degenerate test case.

	public class MinPathTest {
	  @Test
	  public void nothing() throws Exception {
	  }
	}
	
Dijkstra's algorithm is a simple technique for finding the minimum path through a graph with edges of arbitrary length.  Given the starting and ending nodes, the algorithm will tell you the minimum path, and the length of that path. 

So from the very start there are some interesting decisions to make.  How shall I represent the input graph?  How shall I represent the output of the algorithm?  We can probably decide much of that later.  For now, we should concentrate on the most degenerate of cases: an empty graph.

	public void noGraph_NoPathZeroLength() throws Exception {
	  Assert.assertEquals("{}0", minPath(""));
	}
	
The very first test has forced me to make some decisions about the output format.  I will represent the output path as a set of nodes between curly braces.  The length of the path will be an integer following the curly braces.

This notation is only for my tests.  I am using a technique that I call _composed asserts_.  I like to compose my assertions into statements that are easy to read.  This often means I must write simple translators in my tests that convert the composed assertions into the true API of the system under test.

Clearly I can make this test case pass with nothing more than this:

	private String minPath(String graph) {
	  return "{}0";
	}

Let's look at that test case carefully.  That call to `minPath` isn't right.  Dijkstra's algorithm finds the minimum path between two specified nodes.  So, assuming the nodes have names, the test should really look something like this:

	Assert.assertEquals("{}0", minPath("", "A", "Z"));

It's also pretty ugly.  We can refactor it to be a bit prettier:

	private void assertPath(String graph, String expected) {
	  assertEquals(expected, minPath(graph, "A", "Z"));
	}

	@Test
	public void noGraph_NoPathZeroLength() throws Exception {
	  assertPath("", "{}0");
	}

Notice that the `assertPath` method simply assumes that the test cases will all use `"A"` and `"Z"` for their start and end points.  

One last change.  I think the path and the length should be separated.  

	private void assertMinPath(String graph, 
	                           int length, String path) {
	  assertEquals(length, minLength(graph, "A", "Z"));
	  assertEquals(path, minPath(graph, "A", "Z"));
	}
	
	@Test
	public void noGraph_NoPathZeroLength() throws Exception {
	  assertMinPath("", 0, "{}");
	}

	private int minLength(String graph, String begin, String end) {
	  return 0;
	}

	private String minPath(String graph, String begin, String end) {
	  return "{}";
	}

Since there are two methods to check in my assertion, I think it's reasonable to assume that they should be methods of some class that executes Dijkstra's algorithm.  So let's use the _extract delegate_ refactoring to pull out a new class.  

	public class MinPathTest {
	  private void assertMinPath(String graph,
	                             int length, String path) {
	    PathFinder pf = new PathFinder(graph);
	    assertEquals(length, pf.minLength("A", "Z"));
	    assertEquals(path, pf.minPath("A", "Z"));
	  }

	  @Test
	  public void noGraph_NoPathZeroLength() throws Exception {
	    assertMinPath("", 0, "{}");
	  }
	}

	class PathFinder {
	  public PathFinder(String graph) {
	  }

	  public int minLength(String begin, String end) {
	    return 0;
	  }

	  public String minPath(String begin, String end) {
	    return "{}";
	  }
	}

I think it's interesting how much thought and effort have been put into the structure of this problem; for just the first degenerate test case.  But now I think we can add several more degenerate cases:

	@Test
	public void degenerateCases() throws Exception {
	  assertMinPath("", 0, "{}");   //empty graph
	  assertMinPath("A", 0, "{}");  //one node
	  assertMinPath("B1C", 0, "{}");//no start or end
	  assertMinPath("A1C", 0, "{}");//no end
	  assertMinPath("B1Z", 0, "{}");//no start
	}

These tests forced me to make another decision for the _composed assert_.  For our tests, the structure of a graph edge will be `<name>length<name>`.  So `B1C` is an edge of length 1 that connects node `B` to node `C`.

I think that's all the degenerate cases.  So let's ratchet the complexity up one notch and do the first test case that will force us to do something marginally intelligent. 

	@Test
	public void oneEdge() throws Exception {
	  assertMinPath("A1Z", 1, "{AZ}");
	}

This test fails, of course.  It also makes me uncomfortable because I am testing two things, the length and the path, that I could be testing separately.  This makes me grumble because I spent all that time setting up the composed assert; and now I want to break it apart again.  But I think there's a "clever" way to get around that.  

	private static String ANY = null;

	private void assertMinPath(String graph,
	                           Integer length, String path) {
	  PathFinder pf = new PathFinder(graph);
	  if (length != null)
	    assertEquals((int)length, pf.minLength("A", "Z"));
	  if (path != null)
	    assertEquals(path, pf.minPath("A", "Z"));
	}
	...
	@Test
	public void oneEdge() throws Exception {
	  assertMinPath("A1Z", 1, ANY);
	}

This keeps my composed assert intact, and allows me to ignore either of the two components should I desire.  So now let's make that test pass.

Now, obviously, I could do something hideous like this:

	public int minLength(String begin, String end) {
	  if (graph.equals("A1Z"))
	    return 1;
	  return 0;
	}

But this breaks a number of rules.  First, it breaks the _generality_ rule which says: _As the tests get more specific, the code gets more generic._  In other words, the production code has to become more general in order to pass a failing test.  You can't add expressions to the production code that are specific to the failing test. (I say much more about this in [Episode 19: Advanced TDD](https://cleancoders.com/episode/clean-code-episode-19-p1/show), at [cleancoders.com](cleancoders.com)) 

The second broken rule is test coupling.  We don't want the tests to become strongly coupled to the production code.  The more the coupling the more fragile the tests become.  We don't want to encourage the case that a single change to the production code breaks dozens or hundreds of tests.  In our particular case, we don't want the composed assertion to leak into the API of the production code.

This means that I should not be passing the `String graph` into the constructor of the `PathFinder`.  It also means that the `minPath` function should not return the `String` used by the composed assert.  

So, it's time to begin to decouple the tests.  The `makePathFinder` function below shows how I did that.  

	public class MinPathTest {
	  private static String ANY = null;

	  private void assertMinPath(String graph,
	                             Integer length, String path) {
	    PathFinder pf = makePathFinder(graph);
	    if (length != null)
	      assertEquals((int) length, pf.minLength("A", "Z"));
	    if (path != null)
	      assertEquals(path, pf.minPath("A", "Z"));
	  }

	  private PathFinder makePathFinder(String graph) {
	    PathFinder pf = new PathFinder();
	    Pattern edgePattern = 
	            Pattern.compile("(\\D+)(\\d+)(\\D+)");
	    Matcher matcher = edgePattern.matcher(graph);
	    if (matcher.matches()) {
	      String start = matcher.group(1);
	      int length = Integer.parseInt(matcher.group(2));
	      String end = matcher.group(3);
	      pf.addEdge(start, end, length);
	    }
	    return pf;
	  }

	  @Test
	  public void degenerateCases() throws Exception {
	    assertMinPath("", 0, "{}");   //empty graph
	    assertMinPath("A", 0, "{}");  //one node
	    assertMinPath("B1C", 0, "{}");//no start or end
	    assertMinPath("A1C", 0, "{}");//no end
	    assertMinPath("B1Z", 0, "{}");//no start
	  }

	  @Test
	  public void oneEdge() throws Exception {
	    assertMinPath("A1Z", 1, ANY);
	  }
	}

	class PathFinder {
	  private List<Edge> edges = new ArrayList<>();

	  public PathFinder() {
	  }

	  public int minLength(String begin, String end) {
	    int length = 0;
	    for (Edge edge : edges) {
	      if (edge.begin.equals(begin) && edge.end.equals(end))
	        length += edge.length;
	    }
	    return length;
	  }

	  public String minPath(String begin, String end) {
	    return "{}";
	  }

	  public void addEdge(String start, String end, int length) {
	    edges.add(new Edge(start, end, length));
	  }

	  private static class Edge {
	    public final String begin;
	    public final String end;
	    public final int length;

	    public Edge(String begin, String end, int length) {
	      this.begin = begin;
	      this.end = end;
	      this.length = length;
	    }
	  }
	}

Note that all the parsing for the composed assertion remains in the test class.  The `PathFinder` class knows nothing of the funny syntax I'm using in the tests.  Note also, in order to get the tests to pass, the production code must _assume_ that the graph has only one edge.  That's an assumption we'll be breaking within the next few test cases.  In the mean time we should get rid of that `ANY`.  

	assertMinPath("A1Z", 1, "{AZ}");

So I'm going to need to build the list of nodes in the path.  List?  Ah, there's a `toString` syntax for lists.  We should change that test; and all the tests as follows:

	@Test
	public void degenerateCases() throws Exception {
	  assertMinPath("", 0, "[]");   //empty graph
	  assertMinPath("A", 0, "[]");  //one node
	  assertMinPath("B1C", 0, "[]");//no start or end
	  assertMinPath("A1C", 0, "[]");//no end
	  assertMinPath("B1Z", 0, "[]");//no start
	}

	@Test
	public void oneEdge() throws Exception {
	  assertMinPath("A1Z", 1, "[A, Z]");
	}

Now to get this to pass we'll have to change the `assertMinPath` test helper function by adding a `toString` call.  

	...
	    if (path != null)
	      assertEquals(path, pf.minPath("A", "Z").toString());
	...

We add the `path` list to the `PathFinder` and simply load it up in the `minLength` function.  

	class PathFinder {
	  private List<Edge> edges = new ArrayList<>();
	  ...

	  public int minLength(String begin, String end) {
	    int length = 0;
	    for (Edge edge : edges) {
	      if (edge.begin.equals(begin) && edge.end.equals(end)) {
	        length += edge.length;
	        path.add(edge.begin);
	        path.add(edge.end);
	      }
	    }
	    return length;
	  }

	  public List<String> minPath(String begin, String end) {
	    return path;
	  }
	...

This works.  But I don't like the fact that `minLength` is also calculating the path.  I think we should separate the calculation from the reporting.

	  private void assertMinPath(String graph,
	                             Integer length, String path) {
	    PathFinder pf = makePathFinder(graph);
	    if (length != null)
	      assertEquals((int) length, pf.getLength());
	    if (path != null)
	      assertEquals(path, pf.getPath().toString());
	  }

	  private PathFinder makePathFinder(String graph) {
	    PathFinder pf = new PathFinder();
	    ...
	    pf.findPath("A", "Z");
	    return pf;
	  }

	class PathFinder {
	  private List<Edge> edges = new ArrayList<>();
	  private List<String> path = new ArrayList<>();
	  private int length;

	  ...

	  public int getLength() {
	    return length;
	  }

	  public List<String> getPath() {
	    return path;
	  }

	...
	
OK, that's better.  Now, let's make sure we get the length right.  

	assertMinPath("A2Z", 2, "[A, Z]");

Yeah, that works just fine.  So let's try two sequential edges.

	@Test
	public void twoEdges() throws Exception {
	  assertMinPath("A1B,B1Z", 2, ANY);
	}

This fails as expected, giving us a zero length.  To make this pass we're going to have to parse multiple edges in the `makePathFinder` helper.  That's pretty simple.  Just split the graph on comma, and put the regular expression matcher in a loop. 

	private PathFinder makePathFinder(String graph) {
	  PathFinder pf = new PathFinder();
	  Pattern edgePattern = Pattern.compile("(\\D+)(\\d+)(\\D+)");
	  String[] edges = graph.split(",");
	  for (String edge : edges) {
	    Matcher matcher = edgePattern.matcher(edge);
	    if (matcher.matches()) {
	      String start = matcher.group(1);
	      int length = Integer.parseInt(matcher.group(2));
	      String end = matcher.group(3);
	      pf.addEdge(start, end, length);
	    }
	  }
	  pf.findPath("A", "Z");
	  return pf;
	}

This still fails the test, of course, so now we're going to have to connect the two edges.  Let's assume that the edges are specified in order, so that node A starts the first edge, and node Z ends the second edge.  In that case, we should be able to do the whole connection by changing the `&&` to an `||` in the `findPath` function:

	public void findPath(String begin, String end) {
	  length = 0;
	  for (Edge edge : edges) {
	    if (edge.begin.equals(begin) || edge.end.equals(end)) {
	      length += edge.length;
	      path.add(edge.begin);
	      path.add(edge.end);
	    }
	  }
	}

Did you like that change?  `&&` to `||`.  Yeah, pretty clever.  It'll only work for two consecutive edges.  The assumptions are mounting to the sky!  And, anyway, it doesn't work.  

Oh, it passes the `twoEdges` test, and the `oneEdge` tests, but it fails the `degenerateCases` tests.  And it's no wonder, since our the last two degenerate cases match the "A" first, or "Z" last assumption.  

To get all these tests to pass, I need an implementation that produces zero length and an empty path if there is no path from A to Z.  Now since I don't know how many edges there are (it could be zero, one or two) I can't just grab the two.  Instead, I could do a case analysis for zero, one, or two edges; as follows:

	public void findPath(String begin, String end) {
	  if (edges.size() == 0)
	    return;

	  else if (edges.size() == 1) {
	    Edge edge = edges.get(0);
	    if (edge.begin.equals(begin) && edge.end.equals(end)) {
	      path.add(edge.begin);
	      path.add(edge.end);
	      length += edge.length;
	    }
	  } else {
	    for (Edge edge : edges) {
	      if (edge.begin.equals(begin) || edge.end.equals(end)) {
	        path.add(edge.begin);
	        path.add(edge.end);
	        length += edge.length;
	      }
	    }
	  }
	}
  
OK, that works, but it's truly awful.  Not only does it violate the _generality_ rule; it's also just icky. What's more, it doesn't correctly assemble the path.  E.g. `assertMinPath("A1B,B1Z", 2, "[A, B, Z]");` fails because it produces `[A, B, B, Z]`.  I could fix that by adding yet another horrible `if` statement; but I have a better idea.  Let's walk the graph from start to end.

	public void findPath(String begin, String end) {
	  List<String> p = new ArrayList<>();
	  int l = 0;
	  p.add(begin);

	  for (Edge e = findEdge(begin); 
	       e != null; e = findEdge(e.end)) {
	    p.add(e.end);
	    l += e.length;
	    if (e.end.equals(end)) {
	      length = l;
	      path = p;
	      return;
	    }
	  }
	}

	private Edge findEdge(String begin) {
	  for (Edge e : edges) {
	    if (e.begin.equals(begin))
	      return e;
	  }
	  return null;
	}  

OK, this works.  It's strange that we had to use temporary length and path variables; but that's the only way I could think of to ensure that we ignore paths that don't exist.  I also think this solution gets rid of our order dependency.

	@Test
	public void twoEdges() throws Exception {
	  assertMinPath("A1B,B1Z", 2, "[A, B, Z]");
	  assertMinPath("B1Z,A1B", 2, "[A, B, Z]");
	  assertMinPath("A1X,Y1Z", 0, "[]");
	}
	
Yes.  These all pass.  I also think three or more edges will work.  And so will some graphs with only one complete path, and with other dangling paths.  

	@Test
	public void threeEdges() throws Exception {
	  assertMinPath("A2B,B3C,C4Z", 9, "[A, B, C, Z]");
	  assertMinPath("B3C,C4Z,A2B", 9, "[A, B, C, Z]");
	}

	@Test
	public void OnlyOnePath() throws Exception {
	  assertMinPath("A1B,B2C,C3Z,B4D,D6E", 6, "[A, B, C, Z]");
	}

But this one fails because the graph walk misses the `C3Z` edge. 

	assertMinPath("A1B,B2C,C3D,C3Z", 6, "[A, B, C, Z]");

OK.  So we can't simply walk the graph in order.  What we're going to have to do instead is inspect every possible pathway that leads away from the starting node; and we'll have to record our temporary variables along the way, until we get to the end.  

As you'll see below, this required a fair bit of clerical effort.  I need to keep track of all the nodes, and the paths and lengths associated with those nodes.   But, other than that, the algorithm is about the same.

The loop iteration is different.  It starts at the beginning node, and then walks all the _unvisted_ neighbors, and stores the accumulated lengths and paths in those neighbors.

Note that I used `Integer.MAX_VALUE` as a sentinel that means "Unreached from a visited node".  We limit the search to only those nodes that have been reached, because we are still walking from begin to end.  Any node that hasn't been reached is clearly not _next_ on the path.

	class PathFinder {
	  private List<Edge> edges = new ArrayList<>();
	  private Set<String> nodeNames = new HashSet<>();
	  private Map<String, Node> nodes = new HashMap<>();
	  private Node endNode;

	  public void findPath(String begin, String end) {
	    List<String> unvisited = initializeSearch(begin, end);

	    for (String node = begin; 
		     node != null; node = getNext(unvisited)) {
	      unvisited.remove(node);
	      visit(node);
	    }
    
	    setupEndNode(end);
	  }

	  private List<String> initializeSearch(String begin, 
		                                    String end) {
	    nodeNames.add(begin);
	    nodeNames.add(end);
	    List<String> unvisited = new ArrayList<>(nodeNames);
	    for (String node : unvisited)
	      nodes.put(node, new Node(Integer.MAX_VALUE));

	    nodes.get(begin).length = 0;
	    return unvisited;
	  }

	  private void visit(String node) {
	    List<Edge> neighbors = findEdges(node);
	    Node curNode = nodes.get(node);
	    for (Edge e : neighbors) {
	      Node nbr = nodes.get(e.end);
	      nbr.length = curNode.length + e.length;
	      nbr.path = new ArrayList<String>();
	      nbr.path.addAll(curNode.path);
	      nbr.path.add(node);
	    }
	  }

	  private void setupEndNode(String end) {
	    endNode = nodes.get(end);
	    if (endNode.length != Integer.MAX_VALUE)
	      endNode.path.add(end);
	    else
	      endNode.length = 0;
	  }

	  private String getNext(List<String> unvisited) {
	    for (String name : unvisited) {
	      Node candidate = nodes.get(name);
	      if (candidate.length != Integer.MAX_VALUE)
	        return name;
	    }
	    return null;
	  }

	  private List<Edge> findEdges(String begin) {
	    List<Edge> found = new ArrayList<>();
	    for (Edge e : edges) {
	      if (e.begin.equals(begin))
	        found.add(e);
	    }
	    return found;
	  }

	  public int getLength() {
	    return endNode.length;
	  }

	  public List<String> getPath() {
	    return endNode.path;
	  }

	  public void addEdge(String start, String end, int length) {
	    edges.add(new Edge(start, end, length));
	    nodeNames.add(start);
	    nodeNames.add(end);
	  }

	  private static class Edge {
	    public final String begin;
	    public final String end;
	    public final int length;

	    public Edge(String begin, String end, int length) {
	      this.begin = begin;
	      this.end = end;
	      this.length = length;
	    }
	  }

	  private static class Node {
	    public int length;
	    public List<String> path;

	    public Node(int l) {
	      this.length = l;
	      this.path = new ArrayList<>();
	    }
	  }
	}

This passes.  So now we need to add a test that has parallel paths.  Here's a simple one that should fail:

	assertMinPath("A1B,B2Z,A1Z", 1, "[A, Z]");

And it does.

To get this to pass we are going to have to detect when two pathways converge.  That's simple.  If the length of the target node is not `Integer.MAX_VALUE` then another path has reached this node.  Since we are after the minimum, we can simply set the length on that node to the minimum of the converging pathways.  `Integer.MAX_VALUE` happens to be a very convenient value for that sentinel since it substitutes for an "infinite" length.  

	private void visit(String node) {
	  List<Edge> neighbors = findEdges(node);
	  Node curNode = nodes.get(node);
	  for (Edge e : neighbors) {
	    Node nbr = nodes.get(e.end);

	    int newLength = curNode.length + e.length;
	    if (nbr.length > newLength) {
	      nbr.length = newLength;
	      nbr.path = new ArrayList<String>();
	      nbr.path.addAll(curNode.path);
	      nbr.path.add(node);
	    }
	  }
	}

We can probably speed the algorithm up a bit by terminating the search when we visit the end node.  

	for (String node = begin; node != null && !node.equals(end); node = getNext(unvisited)) {
	  unvisited.remove(node);
	  visit(node);
	}
	
And we can likely speed the algorithm even more by preferring to search the unvisited nodes that have been reached by the shortest length.

	private String getNext(List<String> unvisited) {
	  String minNodeName = null;
	  int minLength = Integer.MAX_VALUE;

	  for (String name : unvisited) {
	    Node candidate = nodes.get(name);
	    if (candidate.length < minLength) {
	      minLength = candidate.length;
	      minNodeName = name;
	    }
	  }
	  return minNodeName;
	}

This, for all intents and purposes _is_ Dijkstra's algorithm. This implementation is not fast because I used sets and lists, and lots of inefficient structures.  Speeding it up would require a fair bit of work.  Moreover, there are some built in assumptions that would have to be fixed.  Specifically, the input graph is assumed to be directed; whereas the general algorithm makes no such assumption.  Finally, the whole thing could use a bit more refactoring.

But the goal was to see whether it was possible to use TDD to stepwise approach Dijkstra's algorithm.  I think it is; though I have to say the approach was pretty jerky.  Those first few tests drove me through several tentative algorithms that did not neatly evolve into one another.  Still, each new test exposed weaknesses in previous implementation that could be corrected in a relatively straightforward manner.  

Is there a better sequence of tests that would lead more directly to Dijkstra's algorithm, without the tentative jerkiness?  Perhaps; but if so, I haven't found them.  

Anyway, this was a fun exercise.  Thanks to the attendee of SCNA for recommending it.  

The final code, that still needs a bit of cleanup (left to the reader as an exercise ;-)

	package dijkstrasAlg;

	import org.junit.Test;

	import java.util.*;
	import java.util.regex.Matcher;
	import java.util.regex.Pattern;

	import static org.junit.Assert.assertEquals;

	public class MinPathTest {
	  private static String ANY = null;

	  private void assertMinPath(String graph,
	                             Integer length, String path) {
	    PathFinder pf = makePathFinder(graph);
	    if (length != null)
	      assertEquals((int) length, pf.getLength());
	    if (path != null)
	      assertEquals(path, pf.getPath().toString());
	  }

	  private PathFinder makePathFinder(String graph) {
	    PathFinder pf = new PathFinder();
	    Pattern edgePattern = 
	            Pattern.compile("(\\D+)(\\d+)(\\D+)");
	    String[] edges = graph.split(",");
	    for (String edge : edges) {
	      Matcher matcher = edgePattern.matcher(edge);
	      if (matcher.matches()) {
	        String start = matcher.group(1);
	        int length = Integer.parseInt(matcher.group(2));
	        String end = matcher.group(3);
	        pf.addEdge(start, end, length);
	      }
	    }
	    pf.findPath("A", "Z");
	    return pf;
	  }

	  @Test
	  public void degenerateCases() throws Exception {
	    assertMinPath("", 0, "[]");   //empty graph
	    assertMinPath("A", 0, "[]");  //one node
	    assertMinPath("B1C", 0, "[]");//no start or end
	    assertMinPath("A1C", 0, "[]");//no end
	    assertMinPath("B1Z", 0, "[]");//no start
	  }

	  @Test
	  public void oneEdge() throws Exception {
	    assertMinPath("A1Z", 1, "[A, Z]");
	    assertMinPath("A2Z", 2, "[A, Z]");
	  }

	  @Test
	  public void twoEdges() throws Exception {
	    assertMinPath("A1B,B1Z", 2, "[A, B, Z]");
	    assertMinPath("B1Z,A1B", 2, "[A, B, Z]");
	    assertMinPath("A1X,Y1Z", 0, "[]");
	  }

	  @Test
	  public void threeEdges() throws Exception {
	    assertMinPath("A2B,B3C,C4Z", 9, "[A, B, C, Z]");
	    assertMinPath("B3C,C4Z,A2B", 9, "[A, B, C, Z]");
	  }

	  @Test
	  public void OnlyOnePath() throws Exception {
	    assertMinPath("A1B,B2C,C3Z,B4D,D6E", 6, "[A, B, C, Z]");
	    assertMinPath("A1B,B2C,C3D,C3Z", 6, "[A, B, C, Z]");
	  }

	  @Test
	  public void parallelPaths() throws Exception {
	    assertMinPath("A1B,B2Z,A1Z", 1, "[A, Z]");
	    assertMinPath("A1B,A1C,A2D,C5E,B8E,C1F,D3F,F2G,G3Z,E2G",
	                   7,"[A, C, F, G, Z]");
	  }
	}

	class PathFinder {
	  private List<Edge> edges = new ArrayList<>();
	  private Set<String> nodeNames = new HashSet<>();
	  private Map<String, Node> nodes = new HashMap<>();
	  private Node endNode;

	  public void findPath(String begin, String end) {
	    List<String> unvisited = initializeSearch(begin, end);

	    for (String node = begin; 
		     node != null && !node.equals(end); 
		     node = getNext(unvisited)) {
	      unvisited.remove(node);
	      visit(node);
	    }

	    setupEndNode(end);
	  }

	  private List<String> initializeSearch(String begin, 
		                                    String end) {
	    nodeNames.add(begin);
	    nodeNames.add(end);
	    List<String> unvisited = new ArrayList<>(nodeNames);
	    for (String node : unvisited)
	      nodes.put(node, new Node(Integer.MAX_VALUE));

	    nodes.get(begin).length = 0;
	    return unvisited;
	  }

	  private void visit(String node) {
	    List<Edge> neighbors = findEdges(node);
	    Node curNode = nodes.get(node);
	    for (Edge e : neighbors) {
	      Node nbr = nodes.get(e.end);

	      int newLength = curNode.length + e.length;
	      if (nbr.length > newLength) {
	        nbr.length = newLength;
	        nbr.path = new ArrayList<String>();
	        nbr.path.addAll(curNode.path);
	        nbr.path.add(node);
	      }
	    }
	  }

	  private void setupEndNode(String end) {
	    endNode = nodes.get(end);
	    if (endNode.length != Integer.MAX_VALUE)
	      endNode.path.add(end);
	    else
	      endNode.length = 0;
	  }

	  private String getNext(List<String> unvisited) {
	    String minNodeName = null;
	    int minLength = Integer.MAX_VALUE;

	    for (String name : unvisited) {
	      Node candidate = nodes.get(name);
	      if (candidate.length < minLength) {
	        minLength = candidate.length;
	        minNodeName = name;
	      }
	    }
	    return minNodeName;
	  }

	  private List<Edge> findEdges(String begin) {
	    List<Edge> found = new ArrayList<>();
	    for (Edge e : edges) {
	      if (e.begin.equals(begin))
	        found.add(e);
	    }
	    return found;
	  }

	  public int getLength() {
	    return endNode.length;
	  }

	  public List<String> getPath() {
	    return endNode.path;
	  }

	  public void addEdge(String start, String end, int length) {
	    edges.add(new Edge(start, end, length));
	    nodeNames.add(start);
	    nodeNames.add(end);
	  }

	  private static class Edge {
	    public final String begin;
	    public final String end;
	    public final int length;

	    public Edge(String begin, String end, int length) {
	      this.begin = begin;
	      this.end = end;
	      this.length = length;
	    }
	  }

	  private static class Node {
	    public int length;
	    public List<String> path;

	    public Node(int l) {
	      this.length = l;
	      this.path = new ArrayList<>();
	    }
	  }
	}
