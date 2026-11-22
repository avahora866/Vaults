---
tags:
  - Algorithm
  - ALGO_STUDY
---
The subsets algorithm generates all possible combinations of elements in a list using either
- Backtracking
- Bit Manipulation
- Iteration

# Explanation
The subset algorithm is used for:
- Combination/permutation problems
- Power set generation

## Types of Subset algorithm implementations
### Backtracking
We start with an empty subset. When we loop through the elements we consider the existing collection of subsets. For each subset we currently have copy it and add the current element.

```
Current Subsets: [[], [1], [2], [1,2]]
Current Element: 3
New Subsets: [[], [1], [2], [1,2], [3], [1, 3], [2, 3], [1,2, 3]]
```

### Bit Manipulation
#TODO

## Time Complexity
Time complexity is O(2^n) as there are 2^n possible subsets.

## State Walkthrough
### Backtracking
Given a set `{1, 2, 3}`, we need to generate all possible subsets.

#### Step 1: Start with an Empty Subset
Begin with an empty subset `[]`.

The algorithm explores each element, deciding whether to include it or not.

Current subset: `[]`

#### Step 2: Consider Including 1
We now have a choice:

1. Include 1 → `[1]`
2. Skip 1 → [] (unchanged)

Current subsets:
```
[[], [1]]
```

#### Step 3: Consider 2 for Each Existing Subset
For each subset from the previous step:

Add 2 to each existing subset

Keep existing subsets unchanged

Current Subsets:
```
[[], [1], [2], [1,2]]
```

#### Step 4: Consider 3 for Each Existing Subset
For each subset from the previous step:

Add 3 to each existing subset

Keep existing subsets unchanged

Current subsets: 
```
[[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]
```

#### Final Output
The algorithm has generated all possible subsets:

```
[[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]
```

### Bit Manipulation
#TODO


# Example code
## Backtracking
```java
import java.util.*;

public class SubsetsBacktracking {
    public static List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        generateSubsets(0, nums, new ArrayList<>(), result);
        return result;
    }

    private static void generateSubsets(int index, int[] nums, List<Integer> current, List<List<Integer>> result) {
        if (index == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }
        // Exclude the current element
        generateSubsets(index + 1, nums, current, result);
        
        // Include the current element
        current.add(nums[index]);
        generateSubsets(index + 1, nums, current, result);
        current.remove(current.size() - 1); // Backtrack
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3};
        System.out.println(subsets(nums));
    }
}
```

## Bit Manipulation
```java
import java.util.*;

public class SubsetsBitManipulation {
    public static List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        int totalSubsets = 1 << n; // 2^n subsets
        List<List<Integer>> result = new ArrayList<>();

        for (int mask = 0; mask < totalSubsets; mask++) {
            List<Integer> subset = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                if ((mask & (1 << i)) != 0) { // Check if bit i is set
                    subset.add(nums[i]);
                }
            }
            result.add(subset);
        }
        return result;
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3};
        System.out.println(subsets(nums));
    }
}
```


## Iteration
```java
import java.util.*;

public class SubsetsIterative {
    public static List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(new ArrayList<>()); // Start with empty subset

        for (int num : nums) {
            int size = result.size();
            for (int i = 0; i < size; i++) {
                List<Integer> newSubset = new ArrayList<>(result.get(i));
                newSubset.add(num);
                result.add(newSubset);
            }
        }
        return result;
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3};
        System.out.println(subsets(nums));
    }
}
```