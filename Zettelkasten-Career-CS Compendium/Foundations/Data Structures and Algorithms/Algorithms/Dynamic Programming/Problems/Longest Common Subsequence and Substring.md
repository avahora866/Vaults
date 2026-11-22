---
tags:
  - Algorithm
---
# Longest Common Subsequence (LCS)

## What is it?

Given two strings, LCS is the **longest sequence of characters** that appear **in both strings in the same order**, but **not necessarily consecutively**.

## Example:

```
String A = "abcde"
String B = "ace"

LCS = "ace"  → Length = 3
```

---

## DP Approach:

We use a 2D DP table where:

* `dp[i][j]` \= length of LCS between `A[0..i-1]` and `B[0..j-1]`

## Java Code:

```java
public class LCS {

    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];

        // Fill the DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1))
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                else
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }

        return dp[m][n];
    }

    public static void main(String[] args) {
        LCS lcs = new LCS();
        String s1 = "abcde";
        String s2 = "ace";
        System.out.println("Length of LCS: " + lcs.longestCommonSubsequence(s1, s2));
    }
}
```

---

## Time Complexity:

* **O(m × n)** — where `m` and `n` are lengths of the two strings

---

# Longest Common Substring

## What is it?

Longest Common Substring is the longest string that appears **as a contiguous block in both strings**.

## Example:

```
String A = "abcdf"
String B = "abedf"

Longest Common Substring = "ab" → Length = 2
```

---

## DP Approach:

We still use a 2D DP table:

* `dp[i][j]` \= length of the longest common substring **ending at** `A[i-1]` and `B[j-1]`

We update a `maxLength` during the process.

---

## Java Code:

```java
public class LongestCommonSubstring {

    public int longestCommonSubstring(String A, String B) {
        int m = A.length(), n = B.length();
        int[][] dp = new int[m + 1][n + 1];
        int maxLength = 0;

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (A.charAt(i - 1) == B.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                    maxLength = Math.max(maxLength, dp[i][j]);
                } else {
                    dp[i][j] = 0; // reset if characters don't match
                }
            }
        }

        return maxLength;
    }

    public static void main(String[] args) {
        LongestCommonSubstring lcs = new LongestCommonSubstring();
        String A = "abcdf";
        String B = "abedf";
        System.out.println("Length of Longest Common Substring: " + lcs.longestCommonSubstring(A, B));
    }
}
```

---

## Time Complexity:

* **O(m × n)**

---

# Quick Comparison

| Feature | LCS | Longest Common Substring |
| ----- | ----- | ----- |
| Contiguous required? | ❌ No | ✅ Yes |
| Usage examples | DNA sequence matching | Plagiarism detection, pattern match |
| DP logic | Max of 3 options (match, skip A, skip B) | Reset to 0 on mismatch |


