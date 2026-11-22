---
tags:
  - Algorithm
---
Big O notation is the way to measure an algorithm's efficiency by measuring the time complexity of an algorithm as the input size increases. As you increase input size the time the algorithm takes increases. Big O allows use to see the pattern of growth as input size increases. *"As the input size increases, the time complexity increases linearly (O(n))"*
**Note: Big O typically describes the worst-case scenario**


# Note regarding logarithms in Computer Science
By standard when using logarithms we use a base of 10 however in computer science we use a base of 2. So instead of $log_{10}(8)$ it would be $log_{2}(8)$.  We mainly use base 2 as it is more intuitive to understand as well as the fact that computers think in base 2
**By standard logs always use the base 10 so when not explicitly stated assume that the log is using a base of 10.** 
*A $log_{2}(x)$ is usually called a binary logarithm*
The fact that in computer science we use a binary logarithm does not matter much in the case of big o notation as the notation represents the pattern of growth instead of the precise growth which still can be seen with a base 10 logarithm. However it is important to understand.

# Types of O Notation

| Notation    | Description       | Explanation                                                                                                             |
| ----------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------- |
| O (1)       | Constant Time     | The algorithm takes the same amount of time regardless of input size                                                    |
| O (log n)   | Logorithmic time  | Time increases logarithmically with input size. Common in divide-and-conquer algorithms                                 |
| O (n)       | Linear Time       | Time grows proportionally with input size                                                                               |
| O (n log n) | Linearithmic Time | We have n elements. For every element we do a O (log n) resulting in. Linear Time * Logarithmic time or O (n * log (n)) |
| O (n^2)     | Quadratic Time    | Time grows with the square of input size. Common with nested loops                                                      |
| O (n^3)     | Cubic time        | Time grows with the cube of input size. Common with three nested loops                                                  |
| O (2^n)     | Exponential Time  | Time doubles with each additional input element. Usually indicates a brute-force approach.                              |
| O(n!)       | Factorial Time    | The worst common complexity. Time grows factorially with input size.                                                    |

## O (1) - Constant Time
Input size has no effect on the time complexity of the algorithm.
```java
import java.util.HashSet;

public boolean findInHashSet(HashSet<Integer> hashSet, int target) {
    return hashSet.contains(target);  // Direct access
}
```

## O (log n) - Logarithmic time
Input sizes increase increases the time complexity of the algorithm logarithmically:
- input size of 2 = $log_{2}(2) = 1$
- input size of 4 = $log_{2}(4) = 2$
- input size of 8 = $log_{2}(8) = 3$
- input size of 16 = $log_{2}(16) = 4$
- input size of 1,000 = $log_{2}(1,000) = 10$
- input size of 1,000,000 = $log_{2}(1,000,000) = 20$

**Conceptually: As input gets larger, each additional step becomes harder and harder to reach**

```java
public boolean binarySearch(int[] sortedArray, int target) {
    int left = 0;
    int right = sortedArray.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (sortedArray[mid] == target) {
            return true;
        } else if (sortedArray[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return false;
}
```

## O (n) - Linear Time
As Input size increases the algorithm complexity increases linearly

```java
public boolean linearSearch(int[] array, int target) {
    for (int item : array) {
        if (item == target) {
            return true;
        }
    }
    return false;
}
```

## O (n log n) - Linearithmic time
For every element in input we do a Logarithmic operation. This results in a O (n * log n) algorithm.
For example: 

- Input size of 2 = linearithmic time = $2 * log(2) = 1$
- Input size of 4 = linearithmic time = $4 * log(4) = 4 * 2 = 8$
- Input size of 8 = linearithmic time = $8 * log(8) = 8 * 3 = 24$
- Input size of 16 = linearithmic time = $16 * log(16) = 16 * 4 = 64$

```java
import java.util.Arrays;

public boolean sortThenSearch(int[] array, int target) {
    // Sorting takes O(n log n)
    Arrays.sort(array);
    // Binary search takes O(log n)
    return binarySearch(array, target);
}
```

## O (n^2) - Quadratic
Time grows with the square of input size. Common with nested loops
Unlike in linearithmic time for every element given we do n operation. This results in $n * n$ or $n^2$

```java
public boolean findUsingNestedLoops(int[] array, int target) {
    int n = array.length;
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            // Contrived example: check if target appears at least twice
            if (i != j && array[i] == target && array[j] == target) {
                return true;
            }
        }
    }
    return false;
}
```

## O (n^3) - Cube
Time grows with the cube of input size. Common with three nested loops
Unlike in linearithmic time for every element given we do $n^2$ operation. This results in $n * n * n$ or $n^3$

```java
public boolean findUsingNestedLoops(int[] array, int target) {
    int n = array.length;
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            // Contrived example: check if target appears at least twice
            if (i != j && array[i] == target && array[j] == target) {
                return true;
            }
        }
    }
    return false;
}
```

## O (2^n) - Exponential Time
Each additional element doubles the time complexity
- Input Size 1 = $2 ^ 1 = 2$
- Input Size 2 = $2 ^ 2 = 4$
- Input Size 3 = $2 ^ 3 = 8$
- Input Size 4 = $2 ^ 4 = 16$
- Input Size 5 = $2 ^ 5 = 32$
- Input Size 6 = $2 ^ 6 = 1,024$
- Input Size 7 = $2 ^ 7 = 1,048, 576$

```java
import java.util.ArrayList;
import java.util.List;

public boolean findInAllSubsets(int[] array, int target) {
    return generateSubsets(array, 0, new ArrayList<>(), target);
}

private boolean generateSubsets(int[] array, int index, List<Integer> current, int target) {
    if (index == array.length) {
        return current.contains(target);
    }
    
    // Don't include current element
    if (generateSubsets(array, index + 1, current, target)) {
        return true;
    }
    
    // Include current element
    current.add(array[index]);
    boolean found = generateSubsets(array, index + 1, current, target);
    current.remove(current.size() - 1); // backtrack
    
    return found;
}
```

## O (n!)#
Factorial time = multiply n by each positive integer until 1
Factorial time grows incredibly fast
- Input size 1 = 1 = 1
- Input size 2 = 2 x 1 = 2
- Input size 3 = 3 x 2 x 1 = 6
- Input size 4 = 4 x 3 x 2 x 1 = 24
- Input size 5 = 5 x 4 x 3 x 2 x 1 = 120
```java
import java.util.ArrayList;
import java.util.List;

public boolean findInAllPermutations(int[] array, int target) {
    List<Integer> list = new ArrayList<>();
    for (int num : array) {
        list.add(num);
    }
    return generatePermutations(list, 0, target);
}

private boolean generatePermutations(List<Integer> array, int start, int target) {
    if (start == array.size()) {
        return array.contains(target);
    }
    
    for (int i = start; i < array.size(); i++) {
        // Swap
        Integer temp = array.get(start);
        array.set(start, array.get(i));
        array.set(i, temp);
        
        // Recurse
        if (generatePermutations(array, start + 1, target)) {
            return true;
        }
        
        // Backtrack
        temp = array.get(start);
        array.set(start, array.get(i));
        array.set(i, temp);
    }
    return false;
}
```