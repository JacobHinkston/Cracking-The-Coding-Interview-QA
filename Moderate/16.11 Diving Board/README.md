<h2>16.11 Diving Board</h2>

<p> You are building a diving board by placing a bunch of planks of wood end-to-end. There are two types of planks, one of length shorter and one of length longer. You must use exactly K planks of wood. Write a method to generate all possible lengths for the diving board. </p>

<h3>Solution</h3>

<p>One way to approach this is to think about the choices we make as we're building a diving board. This leads us to a recursive algorithm. </p>

<h4>Recursive Solution</h4>

<p>For a recursive solution, we can imagine ourselves building a diving board. We make K decisions, each time choosing which plank we will put on next. Once we've put on K planks, we have a complete diving board and we can add this to the list (assuming we haven't seen this length before). </p>

<p>We can follow this logic to write recursive code. Note that we don't need to track the sequence of planks. All we need to know is the current length and the number of planks remaining.</p>


```java
HashSet<Integer> allLengths(int k, int shorter , int longer) { 
    HashSet <Integer> lengths = new HashSet<Integer>( );
    getAllLengths(k, 0, shorter, longer, lengths); 
    return lengths;
}

void getAllLengths(int k, int total, int shorter, int longer, HashSet<Integer> lengths) { 
    if (k == 0) { 
        lengths.add(total); 
        return; 
    }
    getAllLengths(k - 1, total + shorter, shorter, longer, lengths); 
    getAllLengths(k - 1, total + longer, shorter, longer, lengths); 
}

```

<p>We've added each length to a hash set. This will automatically prevent adding duplicates. </p>
<p>This algorithm takes O(2K) time, since there are two choices at each recursive call and we recurse to a depth of K. </p>

<h4>Memoization Solution</h4>
<p>As in many recursive algorithms (especially those with exponential runtimes). we can optimize this through memorization (a form of dynamic programming). </p>
<p>Observe that some of the recursive calls will be essentially equivalent. For example, picking plank 1 and then plank 2 is equivalent to picking plank 2 and then plank 1. </p>
<p>Therefore, if we've seen this (total, plank count) pair before then we stop this recursive path. We can do this using a HashSet with a key of (total, plank count). </p>

<article>
    <b>
        Many candidates will make a mistake here. Rather than stopping only when they've seen (total, plank count), they'll stop whenever they've seen just total before. This is incorrect. Seeing two planks of length 1 is not the same thing as one plank of length 2, because there are different numbers of planks remaining. In memoization problems, be very careful about what you choose for your key. 
    </b>
</article>

<p>The code for this approach is very similar to the earlier approach.</p>

```java
HashSet<Integer> allLengths(int k, int shorter, int longer) {
    HashSet <Integer> lengths = new HashSet<Integer>();
    HashSet <String> visited = new HashSet<String>();
    getAllLengths(k, 0, shorter, longer, lengths, visited); 
    return lengths;
}
void getAllLengths(int k, int total, int shorter, int longer, HashSet<Integer> lengths, HashSet<String> visited) { 
    if (k == 0) { 
        lengths.add(total); 
        return; 
    } 
    String key = k + " " + total;

    if (visited.contains(key)) { 
        return; 
    } 
    getAllLengths(k - 1, total + shorter, shorter, longer, lengths, visited); 
    getAllLengths(k - 1, total + longer, shorter, longer, lengths, visited); 
    visited.add(key); 
}
```

<p>For simplicity, we've set the key to be a string representation of total and the current plank count. Some people may argue it's better to use a data structure to represent this pair. There are benefits to this, but there are drawbacks as well. It's worth discussing this tradeoff with your interviewer. </p>

<p>The runtime of this algorithm is a bit tricky to figure out. </p>
<p>One way we can think about the runtime is by understanding that we're basically filling in a table of SUMS x PLANK COUNTS. The biggest possible sum is K * LONGER and the biggest possible plank count is K. Therefore, the runtime will be no worse than O(K2 * LONGER). </p>
<p>Of course, a bunch of those sums will never actually be reached. How many unique sums can we get? Observe that any path with the same number of each type of planks will have the same sum. Since we can have at most K planks of each type, there are only K different sums we can make. Therefore, the table is really KxK, and the runtime is a (K^2). </p>

<h4>Optimal Solution</h4>

<p>If you re-read the prior paragraph, you might notice something interesting. There are only K distinct sums we can get. Isn't that the whole point of the problem-to find all possible sums? </p>

<p>
    We don't actually need to go through all arrangements of planks. We just need to go through all unique sets of K planks (sets, not orders!). There are only K ways of picking K planks if we only have two possible types: {O of type A, K of type B}, {1 of type A, K -1 of type B}, {2 of type A, K - 2 of type B}, ... 
</p>
<p>
    This can be done in just a simple for loop. At each "sequence'; we just compute the sum. 
</p>

```java
HashSet<Integer> allLengths(int k, int shorter, int longer) { 
HashSet<Integer> lengths = new HashSet<Integer>(); 
    for (int nShorter = 0; nShorter <= k; nShorter++) { 
        int nLonger = k - nShorter; 
        int length = nShorter * shorter + nLonger * longer; 
        lengths.add(length); 
    } 
    return lengths; 
}
```

<p>
    We've used a Ha shSet here for consistency with the prior solutions. This isn't really necessary though, since we shouldn't get any duplicates. We could instead use an Array List.lf we do this, though, we just need to handle an edge case where the two types of planks are the same length. In this case, we would just return an ArrayList of size 1.
</p>











