---
{"dg-publish":true,"permalink":"/coding/data-structures/string-builder/","created":"2023-01-23T01:32:09.548+01:00","updated":"2023-01-23T01:32:09.548+01:00"}
---

# StringBuilder ⚒️
The `StringBuffer` / `StringBuilder` classes are primarily used to modify string values without having to initialize a new String object every time. They simply create a resizable array of all the strings, copying them back to a string only when necessary.

## Java and immutable strings 
#### Example in Java without string builder
This function concatenate a list of strings. 
The strings are all the same length (call this _x_) and that there are _n_ strings.
```java
String joinWords(String[] words) {
	String sentence = "";
	
	for (String w : words) 
		sentence = sentence + w;
	
	return sentence;
}
```
Since in Java strings are immutable, in this piece of code a new copy of the string is created on each concatenation, and the two strings are copied over, character by character. The first iteration requires us to copy _x_ characters, the second _2x_ characters, and so on.
- **Time complexity:** $O(x + 2x + \ ... \ + nx) = O(x \cdot n^2)$ (sum of _1_ through _n_ equals $\frac {n(n+1)} {2}$, or $O(n^2)$)

#### Example in Java with string builder
As said before, StringBuilder simply creates a resizable array of all the strings, copying them back only when necessary.
```java
String joinWords(String[] words) {
	StringBuilder sentence = new StringBuilder();
	
	for (String w: words)
		sentence.append(w);
		
	return sentence.toString();
}
```
- **Time complexity:** $O(x \cdot n)$

## C++ and mutable strings
C++ strings are mutable so the performance considerations of concatenation are less of a concern. However, you can decide to use `std::stringstream`.