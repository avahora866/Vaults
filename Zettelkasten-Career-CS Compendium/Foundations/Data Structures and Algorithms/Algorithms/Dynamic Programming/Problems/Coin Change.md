---
tags:
  - Algorithm
---
There are actually **two main versions** of the problem

---

# Coin Change Problem (Minimum Number of Coins)

## Problem Statement:

Given a list of coin denominations and a target amount `amount`, return the **minimum number of coins** needed to make up that amount.  
 If it’s not possible, return `-1`.

---

## Example:

```
Coins = [1, 2, 5], Amount = 11  
Result = 3 (11 = 5 + 5 + 1)
```

---

## Dynamic Programming Approach

We define:

* `dp[i]` \= the **minimum number of coins needed to make amount `i`**

Initial state:

* `dp[0] = 0` (0 coins to make amount 0\)

* All others initialized to `amount + 1` (a sentinel value for "unreachable")

---

## Java Code (Minimum Coins)

```java
import java.util.Arrays;

public class CoinChange {

    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);  // set to max value
        dp[0] = 0;

        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }

    public static void main(String[] args) {
        CoinChange cc = new CoinChange();
        int[] coins = {1, 2, 5};
        int amount = 11;
        System.out.println("Minimum coins needed: " + cc.coinChange(coins, amount));
    }
}
```

---

## Time & Space Complexity:

* **Time**: O(amount × coins.length)

* **Space**: O(amount)

---

# Coin Change II (Count Total Ways to Make Change)

## Problem Statement:

Given coins and amount, return **how many different combinations** of coins can sum up to the amount.

Order doesn't matter (i.e., \[1,2\] and \[2,1\] count as the same).

---

## Example:

```
Coins = [1, 2, 5], Amount = 5  
Combinations:  
- 5  
- 2 + 2 + 1  
- 2 + 1 + 1 + 1  
- 1 + 1 + 1 + 1 + 1  
=> Total = 4
```

---

## Dynamic Programming Approach

Use a 1D array `dp[i]` \= number of ways to make amount `i`

We loop over **coins first**, then **amounts**, to avoid counting permutations multiple times.

---

## Java Code (Total Combinations)

```java
public class CoinChangeCombinations {

    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1; // 1 way to make amount 0

        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] += dp[i - coin];
            }
        }

        return dp[amount];
    }

    public static void main(String[] args) {
        CoinChangeCombinations ccc = new CoinChangeCombinations();
        int[] coins = {1, 2, 5};
        int amount = 5;
        System.out.println("Total combinations: " + ccc.change(amount, coins));
    }
}
```

---

## Time & Space Complexity:

* **Time**: O(amount × coins.length)

* **Space**: O(amount)

---

# Summary Table

| Version | Goal | Output | Loop Order |
| ----- | ----- | ----- | ----- |
| Coin Change (min coins) | Min number of coins | Integer or \-1 | Amount → Coin |
| Coin Change II (combinations) | Number of ways to make amount | Integer | Coin → Amount |

---

