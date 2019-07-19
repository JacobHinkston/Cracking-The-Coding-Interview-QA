<h2>16.5 Factorial Zeros</h2>

<p>
    Write an algorithm which computes the number of trailing zeros in n factorial. 
</p>

<h3>Solution</h3>

<p>
    A simple approach is to compute the factorial, and then count the number of trailing zeros by continuously dividing by ten. The problem with this though is that the bounds of an int would be exceeded very quickly. To avoid this issue, we can look at this problem mathematically. 
</p>

<p>
    Consider a factorial like 19!: 
</p>
<code>
    19! = 1*2*3*4*5*6*7*8*9*10*11*12*13*14*15*16*17*18*19 
</code>
<p>
    A trailing zero is created with multiples of 10, and multiples of 10 are created with pairs of 5-multiples and 2-multiples. 
</p>
<p>
    For example, in 19!, the following terms create the trailing zeros: 
</p>
<code>
    19! = 2 * ... * 5 * •.. * 10 * ... * 15 * 16 * ... 
</code>
<p>
    Therefore, to count the number of zeros, we only need to count the pairs of multiples of 5 and 2. There will always be more multiples of 2 than 5, though, so simply counting the number of multiples of 5 is sufficient.
</p>
<p>
    One "gotcha" here is 15 contributes a multiple of 5 (and therefore one trailing zero), while 25 contributes two (because 25 = 5 * 5). 
</p>
<p>
    There are two different ways to write this code.
</p>
<p>
    The first way is to iterate through all the numbers from 2 through n, counting the number of times that 5 goes into each number.
</p>

```java
/*If the number is a 5 of five, return which power of 5. For example: 5 -> 1, 2 * 25-> 2, etc. */
int factorsOf5(int i) { 
    int count = 0;
    while (i % 5 == 0) { 
        count++; 
        i /= 5; 
    } 
    return count; 
}
int countFactZeros(int num) { 
    int count = 0; 
    for (int i = 2; i <= num; i++) { 
        count += factorsOf5(i);
    }
    return count;
}
```

<p>
    This isn't bad, but we can make it a little more efficient by directly counting the factors of 5. Using this approach, we would first count the number of multiples of 5 between 1 and n (which is n/5 ), then the number of multiples of 25 ( n/25 ), then 125, and so on. 
</p>
<p>
    To count how many multiples of m are in n, we can just divide n by m. 
</p>

```java
int countFactZeros(int num) { 
    int count = 0;
    if (num < 0) { 
        return -1;
    }
    for (int i = 5; num / i > 0; i *= 5) {
        count += num / i; 
    } 
    return count;
}
```

<p>
    This problem is a bit of a brainteaser, but it can be approached logically (as shown above). By thinking through what exactly will contribute a zero, you can come up with a solution. You should be very clear in your rules upfront so that you can implement it correctly. 
</p>