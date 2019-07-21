<h2>16.7 Number Max</h2>

<p>
    Write a method that finds the maximum of two numbers. You should not use if-else or any other comparison operator.
</p>

<h3>Solution</h3>

<p>
    A common way of implementing a max function is to look at the sign of a - b . In this case, we can't use a comparison operator on this sign, but we can use multiplication.
</p>

<code>
    Let k equal the sign of a - b such that if a - b >= 0, then k is. Else, k 0.Let q be the inverse of k.
</code>

<p>
    We can then implement the code as follows:
</p>

```java
/* Flips a 1 to a 0 and a 0 to a 1 */
int flip(int bit) {
    return 1^bit;
}

/* Returns 1 if a is positive, and 0 if a is negative */

int getMaxNaive(int a, int b){
    int k = sign(a - b);
    int q = flip(k);
    return a * k + b * q;
}
```

<p>
    This code almost works. It fails, unfortunately, when a - INT_MAX - 2 and b is -15. In this case, a - b will be greater than INT_MAX and will overflow, resulting in a negative value.
</p>
<p>
    We can implement a solution to this problem by using the same approach. Our goal is to maintain the condition where k is 1 when a > b. We will need to use more complex logic to accomplish this.
</p>
<p>
    When does a - b overflow? It will overflow only when a is positive and b is negative, or the other way around. It may be difficult to specially detect the overflow condition, but we can detect when a and b have different signs. Note that if a and b have different signs, then we want k to equal sign (a) .
</p>

<p>
    The logic looks like:
</p>

```python
    if a and b have different signs:
        // if a > 0, then b < 0, and k = 1.
        // if a < 0, then b > 0, and k = 0.
        // so either way, k = sign(a)
        let k = sign(a)
    else
        let k -= sign(a - b) // overflow is impossible
```
<p>
    The code below implements this, using multiplication instead of if-statements.
</p>


```java
int getMax(int a, int b){
    int c = a - b;

    int sa = sign(a); // if a >= 0, then 1 else 0.
    int sb = sing(b); // if b >= 0, then 1 else 0.
    int sc = sign(c); // depends on whether or not a - b overflows
    /* 
        Goal: define a value k which is 1 if a > b nd 6 if a < b. * (if a = b, it doesn't matter what value k is)
    */

    // If a and b have different signs, then k = sign(a)
    int use_sign_of_a = sa ^ sb;

    //If a and b have the same sign, then k = sign(a - b)
    int use_sign_of_c = flip(sa ^ sb);

    int k use_sign_of_a * sa + use_sign_of_e * SC;
    int q = flip(k); //opposite of k

    return a * k + b * q;
}
```

<p>
    Note that for clarity, we split up the code into many different methods and variables. This is certainly not the most compact or efficient way to write it, but it does make what we're doing much cleaner.
</p>