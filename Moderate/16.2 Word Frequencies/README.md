<h2>16.2 Word Frequencies:</h2>
<p>
    Design a method to find the frequency of occurrences of any given word in a book. what if we were running this algorithm multiple times? 
</p>

<h3>Solution 1: Single Query</h3>

<p>
    Let's start with the simple case.
</p>
<p>
    In this case, we simply go through the book, word by word, and count the number of times that a word appears. This will take 0 (n) time. We know we can't do better than that since we must look at every word in the book.
</p>

```java
int getFrequency(String[] book, String word) { 
    word = word.trim().toLowerCase(); 
    int count = 0;
    for (String w : book) {
        if (w.trim().toLowerCase().equals(word)) { 
            count++;
        } 
    }
    return count;
} 
```

<p>
    We have also converted the string to lowercase and trimmed it. You can discuss with your interviewer if this is necessary (or even desired). 
</p>

<h3>Solution 2: Repetitive Queries</h3>

<p>
    If we're doing the operation repeatedly, then we can probably afford to take some time and extra memory to do pre-processing on the book. We can create a hash table which maps from a word to its frequency. The frequency of any word can be easily looked up in 0 (1) time. The code for this is below. 
</p>

```java
HashMap<String, Integer> setupDictionary(String[] book) {
    HashMap<String, Integer> table = new HashMap<String, Integer>()
    for (String word : book) { 
        word = word.toLowerCase();
        if (word.trim() != "") {
            if (!table.containsKey(word)) {  
                 table.put(word, 0);  
            }
            table.put(word, table.get(word) + l);  
        }
    } 
    return table;
} 

int getFrequency(HashMap<String, Integer> table, String word) {
    if (table == null || word == null) return -1; 
    word = word.toLowerCase() 
    if (table.containsKey(word)) { 
         return table.get(word) 
    }
    return 0
}
```
<p>
    Note that a problem like this is actually relatively easy. Thus, the interviewer is going to be looking heavily at how careful you are. Did you check for error conditions? 
</p>
