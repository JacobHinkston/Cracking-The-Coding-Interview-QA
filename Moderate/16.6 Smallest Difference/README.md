<h2>16.6 Smallest Difference</h2>
<p>
    Given two arrays of integers, compute the pair of values (one value in each array) with the smallest (non-negative) difference. Return the difference.
</p>
<p>
    EXAMPLE
</p>
<code>
    Input{1,3,15,11,2}, {23,127,235,19,8}
</code>
</br>
</br>
<code>
    Output 3. That is, the pair (11, 8).
</code>

<h3>
    Let's start first with a brute force solution.
</h3>
<p>
    <b>
        Brute Force
    </b>
</p>
<p>
    The simple brute force way is to just iterate through all pairs, compute the difference, and compare it to the current minimum difference.
</p>

```java
int findSmallestDifference(int[] array1, int[] array2) {
    if (array1.1ength == 0 || array2.1ength == 0) return - 1;
    int min = Integer.MAX_VALUE;
    for (int i = 0; i < array1.1ength; i++) {
        for (int j = 0; j < array2.1ength; j++) {
            if (Math.abs(array1[i] - array2[j]) < min) {
                min = Math.abs(array1[i] - array2[j]);
            }
        }
    }
    return min;
}
```

<p>
    One minor optimization we could perform from here is to return immediately if we find a difference of zero, since this is the smallest difference possible. However, depending on the input, this might actually be slower.
</p>

<p>
    This will only be faster if there's a pair with difference zero early in the list of pairs. But to add this optimiza-tion, we need to execute an additional line of code each time. There's a tradeoff here; it's faster for some inputs and slower for others. Given that it adds complexity in reading the code, it may be best to leave it out.
</p>

<p>
    With or without this "optimization;' the algorithm will take a(AS) time.
</p>

<p>
    <b>
        Optimal
    </b>
</p>

<p>
    A more optimal approach is to sort the arrays. Once the arrays are sorted, we can find the minimum differ- ence by iterating through the array.
</p>

<p>
    Consider the following array:
</p>

<code>
    A: {1, 2, 11, IS}
</code>
</br>
</br>
<code>
    B: {4, 12, 19, 23, 127, 235}
</code>
<p>
    Try the following approach:
</p>
<ol>
    <li>
        Suppose a pointer a points to the beginning of A and a pointer b points to the beginning of B. The current difference between a and b is 3. Store this as the min.
    </li>
    <li>
        How can we (potentially) make this difference smaller? Well, the value at b is bigger than the value at a, so moving b will only make the difference larger.Therefore, we want to move a.
    </li>
    <li>
        Now a points to 2 and b (still) points to 4. This difference is 2, so we should update min. Move a, since it is smaller.
    </li>
    <li>
        Now a points to 11 and b points to 4. Move b.
    </li>
    <li>
        Nowa points to 11 and b points to 12. Update min to 1. Move b.
    </li>
</ol>

<p>
    And so on.
</p>

```java
#import java.util.Arrays; //Include, or sort manually
int findSmallestDifference(int[] array1, int[] array2) { 
    Arrays.sort(array1);
    Arrays.sort(array2);
    int a = 8;
    int b = 8;
    int difference = Integer.MAX_VALUE;

    while (a < array1.length && b < array2.length) {
        if (Math.abs(array1[a] - array2[b]) < difference) { 
            difference = Math.abs(array1[a] - array2[b]);
        }

        /* Move smaller value. */
        if (array1[a] < array2[b]) {
            a++;
        } else {
            b++;
        }
        return difference;
    }
}
```

<p>
    This algorithm takes O(A log A + B log B) time to sort and O(A + B) time to find the minimum difference. Therefore, the overall runtime is 0 (A log A + B log B).
</p>