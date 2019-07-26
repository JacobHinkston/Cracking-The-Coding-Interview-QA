<h2>16.12 XML Encoding</h2>

<p> Since XML is very verbose, you are given a way of encoding it where each tag gets mapped to a pre-defined integer value. The language/grammar is as follows:</p>

<p>
    <i>
        Element --> Tag Attributes END Chil dren END 
    </i>
</p>
<p>
    <i>
        Attribute --> Tag Value 
    </i>
</p>
<p>
    <i>
        END --> 0
    </i>
</p>
<p>
    <i>
        Tag --> Some Predefined Mapping to Int
    </i>
</p>
<p>
    <i>
        Value --> String Value
    </i>
</p>

<p>
    For example, the following XML might be converted into the compressed string below (assuming a mapping of family - > 1, person - >2, firstName - > 3, lastName - > 4, state -> 5). 
</p>

```html
    <family lastName="McDowell" state="CA">
        <person firstName="Gayle">Some Message</person> 
    </family>
```
<p>Becomes: </p>
<p>1 4 McDowell 5 CA e 2 3 Gayle e Some Message e e </p>
<p>
    Write code to print the encoded version of an XML element (passed in Element and Attribute objects). 
</p>

<h3>SOLUTION</h3>
<p>
    Since we know the element will be passed in as an Element and Attribute, our code is reasonably simple. We can implement this by applying a tree-like approach.
</p>
<p>
    We repeatedly call encode () on parts of the XML structure, handling the code in slightly different ways depending on the type of the XML element.
</p>

```java
void encode(Element root, StringBuilder sb) { 
    encode(root.getNameCode(), sb); 
    for (Attribute a : root.attributes) {
        encode(a, sb); 
    } 
    encode("0", sb); 
    if (root.value != null && root. value != "") { 
        encode(root.value, sb); 
    } else { 
        for (Element e : root.children) { 
            encode(e, sb); 
        } 
    } 
    encode("0", sb); 
} 

void encode(String v, StringBuilder sb) { 
    sb.append(v); 
    sb.append(" ");
}

void encode(Attribute attr, StringBuilder sb) { 
    encode(attr.getTagCode(), sb); 
    encode(attr.value, sb); 
}

String encodeToString(Element root) { 
    stringBuilder sb = new StringBuilder(); 
    encode(root, sb); 
    return sb.toString(); 
} 
```

<p>
    Observe in line 17, the use of the very simple encode method for a string. This is somewhat unnecessary; all it does is insert the string and a space following it. However, using this method is a nice touch as it ensures that every element will be inserted with a space surrounding it. Otherwise, it might be easy to break the encoding by forgetting to append the empty string.
</p>