<h2>16.9 Operations</h2>
<p>
    Write methods to implement the multiply, subtract, and divide operations for integers. The results of all of these are integers. Use only the add operator.
</p>

<h3>Solution</h3>

<p>
    The only operation we have to work with is the add operator. In each of these problems, it's useful to think in depth about what these operations really do or how to phrase them in terms of other operations (either add or operations we've already completed).
</p>

<h4>Subtraction</h4>
<p>
    How can we phrase subtraction in terms of addition? This one is pretty straightforward. The operation a - b is the same thing as a + (-1) * b. However, because we are not allowed to use the * (multiply) operator, we must implement a negate function.
</p>

```java
/* Flip a positive sign to negative or negative sign to pos. */
int negate(int a) { 
    int neg = 0; 
    int newSign = a < 0 ? 1 : -1; 
    while (a != 0) { 
        neg += newSign; 
        a += newSign; 
    } 
    return neg;
} 

/* Subtract two numbers by negating b and adding them */ 
int minus(int a, int b) { 
    return a + negate(b); 
} 
```
<p>
    The negation of the value k is implemented by adding -1 k times. Observe that this will take 0 (k) time. 
</p>

<p>
    If optimizing is something we value here, we can try to get a to zero faster. (For this explanation, we'll assume that a is positive.) To do this, we can first reduce a by 1, then 2, then 4, then 8, and so on. We'll call this value de 1 tao We want a to reach exactly zero. When reducing a by the next de 1 ta would change the sign of a, we reset del ta back to 1 and repeat the process. 
</p>

<p>For example: </p>
<code>
    a: 29 28 26 22 14 13 11 7 6 4 0 
</code>
</br>
</br>
<code>
    delta: -1 -2 -4 -8 -1 -2 -4 -1 -2 -4 
</code>

<p>The code below implements this algorithm</p>

```java
int negate(int a) { 
    int neg = 0;
    int newSign = a < 0 ? 1 : -1; 
    int delta = newSign; 
    while (a != 0) {
        boolean differentSigns = (a + delta > 0) != (a > 0); 
        if (a + delta != 0 && differentSigns) { 
            //If delta is too big, reset it. 
            delta = newSign;
        } 
        neg += delta; 
        a += delta; 
        delta += delta; //Double the delta 
    } 
    return neg; 
}
```

<p>
    Figuring out the runtime here takes a bit of calculation. 
</p>
<p>
    Observe that reducing a by half takes 0 (log a) work. Why? For each round of "reduce a by half'; the absolute values of a and delta always add up to the same number. The values of del ta and a will converge at a/2. Since de 1 ta is being doubled each time, it will take 0 (log a) steps to reach half of a.
</p>

<p>
    We do O(log a)
</p>

<ol>
    <li>
        Reducing a to a/2 takes 0(log a) time. 
    </li>
    <li>
        Reducing a/2 to a/4 takes 0(log a/2) time.
    </li>
    <li>
        Reducing a/4 to a/8 takes 0(log a/4) time. 
    </li>
</ol>
<p>... As so on, for O(log a) rounds.</p>
<p>
    The runtime therefore is 0 (log a + log ( a/2 ) + log ( a/4 ) + . . . ), with 0 (log a) terms in the expression. 
</p>
<p>Recall two rules of logs:</p>
<ul>
    <li>log(xy) = log x + log Y</li>
    <li>log(x/y) = log x - log Y</li>
</ul>
<p>If we apply this to the above expression, we get: </p>
<ol>
    <li>O(log a + log( a/2 ) + log( a/4 ) + ... )</li>
    <li>O(log a + (log a - log 2) + (log a - log 4) + (log a - log 8) + ...</li>
    <li>O((log a)*(log a) - (log 2 + log 4 + log 8 + ... + log )) // O(log a) terms</li>
    <li>O((log a)*(log a) - (1 + 2 + 3 + ... + log a» //computing the values of logs</li>
    <li>0( (log a) * (log a) - (log(a)(1+log(a)))/2 ) // apply equation for sum of 1 through k</li>
    <li>0( (log a) 2) // drop second term from step 5</li>
</ol>
<p>
    Therefore, the runtime is 0 ( (log a)2). 
</p>
<p>
    This math is considerably more complicated than most people would be able to do (or expected to do) in an interview. You could make a simplification: You do O( log a) rounds and the longest round takes O( log a) work. Therefore, as an upper bound, negate takes O( (log a)2) time. In this case, the upper bound happens to be the true time. 
</p>
<p>
    There are some faster solutions too. For example, rather than resetting del ta to 1 at each round, we could change del ta to its previous value. This would have the effect of del ta "counting up" by multiples of two, and then "counting down"by multiples oftwo. The runtime of this approach would be 0 (log a). However, this implementation would require a stack, division, or bit shifting-any of which might violate the spirit of the problem. You could certainly discuss those implementations with your interviewer though.
</p>

<h4>Multiplication </h4>
<p>
    The connection between addition and multiplication is equally straightforward. To multiply a by b, we just add a to itself b times. 
</p>

```java
/* Multiply a by b by adding a to itself b times */ 
int multiply(int a, int b) { 
    if (a < b) { 
        return multiply(b, a); // algorithm is faster if b < a  
    } 
    int sum = 0; 
    for (int i = abs(b); i > 0; i = minus(i, 1)) { 
        sum += a;
    } 
    if (b < e) { 
        sum = negate(sum); 
    } return sum; 
} 
/* Return absolute value */ 
int abs(int a) { 
    if (a < 0) { 
        return negate(a); 
    } else { 
        return a; 
    }
```
<p>
    The one thing we need to be careful of in the above code is to properly handle multiplication of negative numbers. If b is negative, we need to flip the value of s urn. So, what this code really does is: 
</p>
<code>
    multiply(a, b) <-- abs(b) * a * (-1 if b < 0). 
</code>

<p>We also implemented a simple abs function to help.</p>

<h4>Division</h4>
<p>
    Of the three operations, division is certainly the hardest. The good thing is that we can use the multiply, subtract, and negate methods now to implement divide. 
</p>

<p>
    We are trying to compute x where X = a/b . Or, to put this another way, find x where a = bx. We've now changed the problem into one that can be stated with something we know how to do: multiplication. 
</p>

<p>
    We could implement this by multiplying b by progressively higher values, until we reach a. That would be fairly inefficient, particularly given that our implementation of multiply involves a lot of adding. 
</p>

<p>
    Alternatively, we can look at the equation a = xb to see that we can compute x by adding b to itself repeatedly until we reach a. The number of times we need to do that will equal x.
</p>
<p>
    Of course, a might not be evenly divisible by b, and that's okay. Integer division, which is what we've been asked to implement. is supposed to truncate the result. 
</p>
<p>The code below implements this algorithm. </p>

```java
int divide(int a, int b) throws java.lang.ArithmeticException { 
    if (b == 0) { 
        throw new java.lang.ArithmeticException("ERROR")
    } 
    int absa = abs(a); 
    int absb = abs(b); 
    int product = a; 
    int x = a; 
    while (product + absb <= absa) { 
        /* don't go past a */ 
        product += absb; 
        x++; 
    } 
    if ((a < a && b < a) || (a > a && b > a)) { 
        return x; 
    } else { 
        return negate(x); 
    } 
}
```
<p>In tackling this problem, you should be aware of the following: </p>
<ul>
    <li>
        A logical approach of going back to what exactly multiplication and division do comes in handy. Remember that. All (good) interview problems can be approached in a logical, methodical way! 
    </li>
    <li>
        The interviewer is looking for this sort of logical work-your-way-through-it approach. 
    </li>
    <li>
        This is a great problem to demonstrate your ability to write clean code- specifically, to show your ability to reuse code. For example, if you were writing this solution and didn't put negate in its own method, you should move it into its own method once you see that you'll use it multiple times. 
    </li>
    <li>
        Be careful about making assumptions while coding. Don't assume that the numbers are all positive or that a is bigger than b. 
    </li>
</ul>