---
tags:
  - Java
---
The Comparable interface is used to compare an object of the same class with an instance of that class, it provides ordering of data for objects of the user-defined class. The class has to implement the **`java.lang.Comparable`** interface to compare its instance, it provides the `compareTo` method that takes a parameter of the object of that class. In this article, we will see how we can sort an array of pairs of different data types on the different parameters of comparison.

# Using Comparable Interface

- In this method, we are going to implement the Comparable interface from **`java.lang`** Package in the Pair class.
- The Comparable interface contains the method **`compareTo`** to decide the order of the elements.
- Override the **`compareTo`** method in the Pair class.
- Create an array of Pairs and populate the array.
- Use the **`Arrays.sort`**() function to sort the array.

# Example 1

Given an array of Pairs consisting of two fields of type string and integer. you have to sort the array in ascending Lexicographical order and if two strings are the same sort it based on their integer value.

**Sample I/O:**

```
Input:  { {"abc", 3}, {"a", 4}, {"bc", 5}, {"a", 2} }
Output:  { {"a", 2}, {"a", 4}, {"abc", 3}, {"bc", 5} }

Input:  { {"efg", 1}, {"gfg", 1}, {"cba", 1}, {"zaa", 1} }
Output:  { {"cba", 1}, {"efg", 1}, {"gfg", 1}, {"zaa", 1} }
```

```java
import java.io.*;
import java.util.*;

class Pair implements Comparable<Pair> {
	String x;
	int y;

	public Pair(String x, int y)
	{
		this.x = x;
		this.y = y;
	}

	public String toString()
	{
		return "(" + x + "," + y + ")";
	}

	@Override public int compareTo(Pair a)
	{
		// if the string are not equal
		if (this.x.compareTo(a.x) != 0) {
			return this.x.compareTo(a.x);
		}
		else {
			// we compare int values
			// if the strings are equal
			return this.y - a.y;
		}
	}
}

public class GFG {
	public static void main(String[] args)
	{

		int n = 4;
		Pair arr[] = new Pair[n];

		arr[0] = new Pair("abc", 3);
		arr[1] = new Pair("a", 4);
		arr[2] = new Pair("bc", 5);
		arr[3] = new Pair("a", 2);

		// Sorting the array
		Arrays.sort(arr);

		// printing the
		// Pair array
		print(arr);
	}

	public static void print(Pair[] arr)
	{
		for (int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}
	}
}


```

**Output:**

```
**Before Sorting:**
(abc, 3);
(a, 4);
(bc, 5);
(a, 2);

**After Sorting:**
(a,2)
(a,4)
(abc,3)
(bc,5)
```
> **Note:** if two strings are the same then the comparison is done based on the value.

# Example 2

Given an array of Pairs consisting of two strings with first and last names. you have to sort the array in ascending Lexicographical order of the first name and if two strings are the same sort it based on their last name.

**Sample I/O:**

```
Input:  { {"raj", "kashup"}, {"rahul", "singh"}, {"reshmi", "dubey"}, {"rahul", "jetli"} }
Output:  { {"rahul", "jetli"}, {"rahul", "singh"}, {"raj", "kashup"}, {"reshmi", "dubey"} }

Input:  { {"abc", "last"}, {"pklz", "yelp"}, {"rpng", "note"}, {"ppza", "xyz"} }
Output:  { {"abc", "last"}, {"pklz", "yelp"}, {"ppza", "xyz"}, {"rpng", "note"} }
```

```java
import java.io.*;
import java.util.*;

class Pair implements Comparable<Pair> {
	String firstName;
	String lastName;

	public Pair(String x, String y)
	{
		this.firstName = x;
		this.lastName = y;
	}

	public String toString()
	{
		return "( " + firstName + " , " + lastName + " )";
	}

	@Override public int compareTo(Pair a)
	{
		// if the string are not equal
		if (this.firstName.compareTo(a.firstName) != 0) {
			return this.firstName.compareTo(a.firstName);
		}
		else {
			// we compare lastName if firstNames are equal
			return this.lastName.compareTo(a.lastName);
		}
	}
}

public class GFG {
	public static void main(String[] args)
	{

		int n = 4;
		Pair arr[] = new Pair[n];
		arr[0] = new Pair("raj", "kashup");
		arr[1] = new Pair("rahul", "singh");
		arr[2] = new Pair("reshmi", "dubey");
		arr[3] = new Pair("rahul", "jetli");

		// Sorting the array
		Arrays.sort(arr);

		// printing the
		// Pair array
		print(arr);
	}

	public static void print(Pair[] arr)
	{
		for (int i = 0; i < arr.length; i++) {
			System.out.println(arr[i]);
		}
	}
}

```
**Output:**

```
**Before Sorting:**
( raj , kashup )
( rahul , singh )
( reshmi , dubey )
( rahul , jetli )

**After Sorting:**
( rahul , jetli )
( rahul , singh )
( raj , kashup )
( reshmi , dubey )
```