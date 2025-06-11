---
title: "Subset Sum Problem"
date: 2025-06-11 00:00:00 +0800
categories: [C++]
tags: [Dynamic Programming, Algorithm, C++]
---

**Subset Sum Problem**  
The Subset Sum Problem is a classic problem in computer science, particularly in the field of algorithms. This problem asks whether there is a subset of a given set of numbers that sums up to a specific target value. It is an NP-complete problem in the general case, but it can be solved efficiently using a dynamic programming approach for certain cases, or with backtracking to find all possible subsets.

## Problem Statement
You are given:

- A set of non-negative numbers S = {s1, s2, ..., sn}.
- A target value T.

**Goal**: Determine whether there is a subset of S whose elements sum up to T.

**Example**: If S = {3, 34, 4, 12, 5, 2} and T = 9, the answer is **YES** because the subset {4, 5} or {3, 4, 2} sums to 9.

## Dynamic Programming Strategy
The dynamic programming approach for the Subset Sum Problem involves building a boolean table (or array) to keep track of whether a certain sum can be achieved using a subset of the elements processed so far.

**Steps:**

1. **Initialize Table**:  
   Create a `dp` table of size `(number_of_elements + 1) x (target_value + 1)`.  
   `dp[i][j]` will be true if the sum `j` can be achieved using a subset of the first `i` elements.

2. **Base Cases**:
   - `dp[0][0] = true` (Sum 0 can be achieved by taking no elements).
   - `dp[i][0] = true` for all `i` (Sum 0 can always be achieved).
   - `dp[0][j] = false` for all `j > 0` (No elements, cannot achieve a positive sum).

3. **Fill Table**:  
   For each element `s[i-1]`:
   - For each sum `j` from `1` to `target_value`:
     - If `s[i-1] > j`, then `dp[i][j] = dp[i-1][j]`
     - Else, `dp[i][j] = dp[i-1][j] || dp[i-1][j - s[i-1]]`

4. **Final Result**:  
   The final result is `dp[number_of_elements][target_value]`.

## C++ Code Example
```cpp
#include <iostream>
#include <vector>
#include <numeric> // For std::accumulate

// Function to determine if there is a subset with the target sum
bool isSubsetSum(std::vector<int> set, int n, int sum) {
    std::vector<std::vector<bool>> dp(n + 1, std::vector<bool>(sum + 1));

    for (int i = 0; i <= n; i++)
        dp[i][0] = true;

    for (int j = 1; j <= sum; j++)
        dp[0][j] = false;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= sum; j++) {
            if (set[i - 1] > j)
                dp[i][j] = dp[i - 1][j];
            else
                dp[i][j] = dp[i - 1][j] || dp[i - 1][j - set[i - 1]];
        }
    }

    return dp[n][sum];
}

int main() {
    std::vector<int> set = {3, 34, 4, 12, 5, 2};
    int sum = 9;
    int n = set.size();

    if (isSubsetSum(set, n, sum))
        std::cout << "Found a subset with sum " << sum << std::endl;
    else
        std::cout << "No subset found with sum " << sum << std::endl;

    set = {10, 7, 8, 15};
    sum = 20;
    n = set.size();

    if (isSubsetSum(set, n, sum))
        std::cout << "Found a subset with sum " << sum << std::endl;
    else
        std::cout << "No subset found with sum " << sum << std::endl;

    set = {1, 2, 3};
    sum = 7;
    n = set.size();

    if (isSubsetSum(set, n, sum))
        std::cout << "Found a subset with sum " << sum << std::endl;
    else
        std::cout << "No subset found with sum " << sum << std::endl;

    return 0;
}
```

## Example Input and Output

**Input 1:**  
Set: `{3, 34, 4, 12, 5, 2}`  
Target Sum: `9`  
**Output 1:**  
`Found a subset with sum 9`

**Input 2:**  
Set: `{10, 7, 8, 15}`  
Target Sum: `20`  
**Output 2:**  
`Found a subset with sum 20`

**Input 3:**  
Set: `{1, 2, 3}`  
Target Sum: `7`  
**Output 3:**  
`No subset found with sum 7`

## Why This Works
The dynamic programming approach works by breaking down the larger problem into smaller, overlapping subproblems.  
Each cell `dp[i][j]` stores the result for the subproblem "can sum `j` be achieved with the first `i` elements?".  
By systematically filling the table, we ensure subproblems are solved before they're needed, avoiding redundant calculations.  
This guarantees an optimal solution.

**Time Complexity**: `O(n × sum)`

## Summary
- The Subset Sum Problem asks if a subset of a given set of numbers can sum to a target value.
- Solved efficiently using dynamic programming with `O(n×sum)` complexity.
- Applications include resource allocation, cryptography, and data analysis.

**Posted by Naailah Mazaya**