---
tags:
  - Algorithm
  - ALGO_STUDY
---
The two pointers or iterator algorithm maintains two pointers that traverse an array or linked list, often moving at **different paces** or **directions**. Used for
- array partitioning
- palindrome detection
- finding pairs with target sum.

# Explanation
Problem Example: Given a sorted list of integers, find two numbers that sum up to a target value.

For instance, consider the sorted list:

```
[1, 2, 3, 4, 5, 6, 7]
```

And the target sum is 10.

Algorithm Explanation (Two Pointers Approach):

1. Initialize two pointers:
	- One pointer starts at the beginning of the list (left).
	- The other pointer starts at the end of the list (right).
2. Loop through the list:
	- Calculate the sum of the elements at the left and right pointers.
	- If the sum equals the target, return the pair.
	- If the sum is less than the target, move the left pointer to the right to increase the sum.
	- If the sum is greater than the target, move the right pointer to the left to decrease the sum.
3. Repeat the process until the pointers cross each other (left >= right).

## Time Complexity
Time complexity is typically O(n) as each element is processed once.

## State Walkthrough
Let’s go through the algorithm step-by-step with the list `[1, 2, 3, 4, 5, 6, 7]` and the target sum 10.

Initial State:

```
List: [1, 2, 3, 4, 5, 6, 7]

left = 0 (value 1), right = 6 (value 7)

```

### After First Iteration 
Sum 1 + 7 = 8 is less than 10

```
Move left pointer to 1.

List: [1, 2, 3, 4, 5, 6, 7]

left = 1 (value 2), right = 6 (value 7)
```

### After Second Iteration 
Sum 2 + 7 = 9 is still less than 10

```
Move left pointer to 2.

List: [1, 2, 3, 4, 5, 6, 7]

left = 2 (value 3), right = 6 (value 7)
```

### After Third Iteration 
Sum 3 + 7 = 10 matches the target

We found the pair: 3 and 7.

Algorithm terminates.

### Summary:
In the two pointers approach:

- Start with two pointers at the extremes of the list.
- Move the pointers based on the sum relative to the target.

The algorithm efficiently finds the desired pair with a time complexity of O(n), where n is the number of elements in the list.

# Example code
```java
class TwoPointers {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            
            if (sum == target) {
                return new int[]{left + 1, right + 1};
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        return new int[]{};
    }
}
```