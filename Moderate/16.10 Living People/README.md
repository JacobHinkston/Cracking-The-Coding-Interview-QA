<h2>16.10 Living People</h2>

<p>
    Given a list of people with their birth and death years, implement a method to compute the year with the most number of people alive. You may assume that all people were born between 1900 and 2000 (inclusive). If a person was alive during any portion of that year, they should be included in that year's count. For example, Person (birth = 1908, death = 1909) is included in the counts for both 1908 and 1909. 
</p>

<h3>Solution</h3>

<p>
    The first thing we should do is outline what this solution will look like. The interview question hasn't specified the exact form of input. In a real interview, we could ask the interviewer how the input is structured. Alternatively, you can explicitly state your (reasonable) assumptions. 
</p>

<p>
    Here, we'll need to make our own assumptions. We will assume that we have an array of simple Person objects: 
</p>

```java
public class Person { 
    public int birth;
    public int death; 
    public Person(int birthYear, int deathYear) { 
        birth birthYear; 
        death = deathYear; 
    } 
} 
```

<p>
    We could have also given Person a getBirthYear() and getDeathYear() objects. Some would argue that's better style, but for compactness and clarity, we'll just keep the variables public. 
</p>
<p>
    The important thing here is to actually use a Person object. This shows better style than, say, having an integer array for birth years and an integer array for death years (with an impliCit association of births [i) and deat hs [i) being associated with the same person). You don't get a lot of chances to demonstrate great coding style, so it's valuable to take the ones you get. 
</p>

<p>
    With that in mind, let's start with a brute force algorithm. 
</p>

<h4>Brute Force</h4>

<p>
    The brute force algorithm falls directly out from the wording of the problem. We need to find the year with the most number of people alive. Therefore, we go through each year and check how many people are alive in that year.
</p>

```java
int maxAliveYear(Person[] people, int min, int max) { 
    int maxAlive = 9; 
    int maxAliveYear = min; 
    
    for (int year = min; year <= max; year++) { 
        int alive = 9; 
        for (Person person : people) { 
            if (person.birth <= year && year <= person. death) { 
                alive++; 
            } 
        } 
        if (alive > maxAlive) { 
            maxAlive = alive; 
            maxAliveYear = year; 
        } 
    } 
    return maxAliveYear;
}
```

<p>Note that we have passed in the values for the min year (1900) and max year (2000). We shouldn't hard code these values.</p>
<p>The runtime of this is O( RP), where R is the range of years (100 in this case) and P is the number of people. </p>

<h4>Slightly Better Brute Force </h4>
<p>A slightly better way of doing this is to create an array where we track the number of people born in each year. Then, we iterate through the list of people and increment the array for each year they are alive.</p>

```java
int maxAliveYear(Person[] people, int min, int max) { 
    int[] years = createYearMap(people, min, max);
    int best = getMaxIndex(years); 
    return best + min; 
}
/* Add each person's years to a year map. */ 
int[] createYearMap(Person[] people, int min, int max) { 
    int[] years = new int[max - min + 1]; 
    for (Person person : people) { 
        incrementRange(years, person. birth - min, person. death - min); 
    } 
    return years;
}
/* Increment array for each value between left and right. */ 
void incrementRange(int[] values, int left, int right) { 
    for (int i = left; i <= right; i++) { 
        values[i]++;
    }
}
/* Get index of largest element in array. */
int getMaxlndex(int[] values) { 
    int max = 0; 
    for (int i = 1; i < values.length; i++) { 
        if (values[i] > values[max]) { 
            max = i;
        } 
    }
    return max;
}
```

<p>Be careful on the size of the array in line 9. If the range of years is 1900 to 2000 inclusive, then that's 101 years, not 100. That is why the array has size max - min + 1. </p>
<p>Let's think about the runtime by breaking this into parts.</p>
<ul>
    <li>We create an R-sized array, where R is the min and max years.</li>
    <li>Then, for P people, we iterate through the years (Y) that the person is alive. </li>
    <li>Then, we iterate through the R-sized array again.</li>
</ul>
<p>The total runtime is 0 (PY + R) . In the worst case, Y is R and we have done no better than we did in the first algorithm.</p>
<h4>More Optimal</h4>
<p>Let's create an example. (In fact, an example is really helpful in almost all problems. Ideally, you've already done this.) Each column below is matched, so that the items correspond to the same person. For compactness, we'll just write the last two digits of the year. </p>
<code>
    birth: 12 20 10 01 10 23 13 90 83 75 
    </br>
    </br>
    death: 15 90 98 72 98 82 98 98 99 94 
</code>

<p>It's worth noting that it doesn't really matter whether these years are matched up. Every birth adds a person and every death removes a person. </p>
<p>Since we don't actually need to match up the births and deaths, let's sort both. A sorted version of the years might help us solve the problem.</p>
<code>
    birth: 01 10 10 12 13 20 23 75 83 90
    </br>
    </br>
    death: 15 72 82 90 94 98 98 98 98 99 
</code>

<p>We can try walking through the years. </p>
<ul>
  <li>At year 0, no one is alive.</li>  
  <li>At year 1, we see one birth.</li>
  <li>At years 2 through 9, nothing happens.</li>
  <li>Let's skip ahead until year 10, when we have two births. We now have three people alive. </li>
  <li>At year 15, one person dies. We are now down to two people alive.</li>
  <li>And so on. </li>
</ul>

<p>If we walk through the two arrays like this, we can track the number of people alive at each point. </p>

```java
int maxAliveYear(Person[] people, int min, int max) { 
    int[] births getSortedYears(people, true);
    int[] deaths = getSortedYears(people, false);

    int birthIndex = 0; 
    int deathIndex = 0; 
    int currentlyAlive 0;
    int maxAlive = 0; 
    int maxAliveYear = min; 
    /* Walk through arrays. */ 
    while (birthIndex < births.length) {
        if (births[birthIndex] <= deaths[deathIndex]) {
            currentlyAlive++; //include birth 
            if (currentlyAlive > maxAlive) { 
                maxAlive = currentlyAlive; 
                maxAliveYear = births[birthIndex]; 
            }
            birthIndex++; //move birth index
        } else if (births[birthIndex] > deaths[deathIndex]) { 
            currentlyAlive--; //include death 
            deathIndex++; // move death index 
        } 
    }
    return maxAliveYear;
}

/* Copy birth years or death years (depending on the value of copyBirthYear into * integer array, then sort array. */ 
int[] getSortedYears(Person[] people, boolean copyBirthYear) { 
    int[] years = new int[people.length]; 
    for (int i = 0; i < people. length; i++) { 
        years[i] = copyBirthYear ? people[i].birth people[i].death; 
    } 
    Arrays.sort(years); 
    return years; 
}
```
<p>There are some very easy things to mess up here. </p>
<p>On line 13, we need to think carefully about whether this should be a less than «) or a less than or equals «=). The scenario we need to worry about is that you see a birth and death in the same year. (It doesn't matter whether the birth and death is from the same person.)</p>
<p>When we see a birth and death from the same year, we want to include the birth before we include the death, so that we count this person as alive for that year. That is why we use a < = on line 13. </p>
<p>We also need to be careful about where we put the updating of max Alive and maxAliveYear.lt needs to be after the currentAlive++, so that it takes into account the updated total. But it needs to be before birthlndex++, or we won't have the right year. </p>
<p>This algorithm will take 0 (P log P) time, where P is the number of people.</p>

<h4>More Optimal (Maybe) </h4>
<p>Can we optimize this further? To optimize this, we'd need to get rid of the sorting step. We're back to dealing with unsorted values: </p>
<code>
    birth: 12 20 10 01 10 23 13 90 83 75
    </br>
    </br>
    death: 15 90 98 72 98 82 98 98 99 94 
</code>

<p>Earlier, we had logic that said that a birth is just adding a person and a death is just subtracting a person. Therefore, let's represent the data using the logic:</p>

<code>01: +1 10: +1 10: +1 12: +1 13: +1</code>
<code>15: -1 20: +1 23: +1 72: -1 75: +1</code>
<code>82: -1 83: +1 90: +1 90: -1 94: -1</code>
<code>98: -1 98: -1 98: -1 98: -1 99: -1</code>

<p>We can create an array of the years, where the value at array [yea r] indicates how the population changed in that year. To create this array, we walk through the list of people and increment when they're born and decrement when they die. </p>
<p>Once we have this array, we can walk through each of the years, tracking the current population as we go (adding the value at array [yea r] each time). </p>
<p>This logic is reasonably good, but we should think about it more. Does it really work? </p>
<p>One edge case we should consider is when a person dies the same year that they're born. The increment and decrement operations will cancel out to give 0 population change. According to the wording of the problem, this person should be counted as living in that year. </p>
<p>In fact, the "bug" in our algorithm is broader than that. This same issue applies to all people. People who die in 1908 shouldn't be removed from the population count until 1909. </p>
<p>There's a simple fix: instead of decrementing array[deathYear], we should decrement array[deathYear + 1]. </p>

```java

int maxAliveYear(Person[] people, int min, int max) { 
    /* Build population delta array. */
    int[] populationDeltas = getPopulationDeltas(people, min, max);
    int maxAliveYear = getMaxAliveYear(populationDeltas); 
    return maxAliveYear + min;
} 
/* Add birth and death years to deltas array. */
int[] getPopulationDeltas(Person[] people, int min, int max) { 
    int[] populationDeltas = new int[max - min + 2]; 
    for (Person person : people) { 
        int birth = person.birth - min; 
        populationDeltas[birth]++;
        int death = person.death - min; 
        populationDeltas[death + 1]--;
    } 
    return populationDeltas;
} 
/* Compute running sums and return index with max. */ 
int getMaxAliveYear(int[] deltas) { 
    int maxAliveYear = 13;
    int maxAlive = 13;
    int currentlyAlive = 0;
    for (int year = 13; year < deltas. length; year++) { 
        currentlyAlive += deltas[year]; 
        if (currentlyAlive > maxAlive) {
            maxAliveYear = year;
            maxAlive = currentlyAlive; 
        } 
    }
    return maxAliveYear;
} 
```
<p>This algorithm takes 0 (R + P) time, where R is the range of years and P is the number of people. Although O( R + P) might be faster than O( P log P) for many expected inputs, you cannot directly compare the speeds to say that one is faster than the other.</p>
