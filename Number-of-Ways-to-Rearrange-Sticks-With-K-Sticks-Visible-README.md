# leet-day84

# Number of Ways to Rearrange Sticks With K Sticks Visible

## Problem Description

You are given three integers `n`, `m`, and `k`. Consider the following algorithm to find the maximum element of an array of positive integers:

```cpp
maximum_value = -1
maximum_index = -1
search_cost = 0
n = arr.length

for (i = 0; i < n; i++) {
    if (maximum_value < arr[i]) {
        maximum_value = arr[i]
        maximum_index = i
        search_cost = search_cost + 1
    }
}
return maximum_index
```

Your task is to build the array `arr` with the following properties:

1. `arr` has exactly `n` integers.
2. `1 <= arr[i] <= m` for all `0 <= i < n`.

After applying the mentioned algorithm to `arr`, the value of `search_cost` should be equal to `k`.

Your goal is to count the number of ways to build the array `arr` that satisfies these conditions. As the answer may grow large, return it modulo 10^9 + 7.

## Example

Input:
```cpp
n = 2, m = 3, k = 1
```

Output:
```cpp
6
```

Explanation:
The possible arrays are `[1, 1]`, `[2, 1]`, `[2, 2]`, `[3, 1]`, `[3, 2]`, and `[3, 3]`.

## Constraints

- `1 <= n <= 50`
- `1 <= m <= 100`
- `0 <= k <= n`

## Approach

To solve this problem optimally, you can use a dynamic programming approach. You will build a `dp` table where `dp[a][b][c]` represents the number of ways to start building the array starting from index `a`, where the `search_cost` is `c`, and the maximum used integer was `b`.

You can recursively solve the smaller sub-problems first and optimize your answer by stopping the search if you exceed `k` changes.

## Pseudocode

Here's a pseudocode version of the optimized solution:

```cpp
Initialize constants: MOD = 10^9 + 7
Initialize dp array: dp[51][101][51]
Initialize sum array: sum[51][101][51]

for maxN from 1 to m:
    dp[1][maxN][1] = 1
    sum[1][maxN][1] = maxN

for sz from 1 to n:
    for maxN from 1 to m:
        for cost from 1 to k:
            ans = maxN * dp[sz - 1][maxN][cost]
            ans = ans + sum[sz - 1][maxN - 1][cost - 1]
            dp[sz][maxN][cost] = dp[sz][maxN][cost] + ans
            sum[sz][maxN][cost] = sum[sz][maxN - 1][cost] + dp[sz][maxN][cost]

Return sum[n][m][k] % MOD as the answer
```

