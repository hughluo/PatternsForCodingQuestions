# 0/1 Knapsack

* Always start crack the question by explaining Brute-force and complexity (i.e For Knapsack, the brute-force approach has O(2^n) time complexity, since we have to choose yes/no for each element)
* Top down with memoization
* Bottom up using an n-dimension array

## Bruteforce
## Top Down with Memoization
## Bottom Up
* can often furthermore improve the space complexity.



## Examples
### 0/1 Knapsack
Given two integer arrays to represent weights and profits of ‘N’ items, find a subset of these items which will give us maximum profit such that their cumulative weight is not more than a given number ‘C’. Each item can only be selected once, which means either we put an item in the knapsack or we skip it.

Return the maximum profits.

* Solution 1: Top Down with Memoization, Time complexity O(NC), Space complexity O(NC)
```
def solve_knapsack(profits, weights, capacity):
    dp = [ [-1 for i in range(capacity + 1)] for j in range(len(profits)) ] 
    
    def helper(index, rest_capacity):
        if rest_capacity < 0 or index >= len(profits):
            return 0     
        if dp[index][rest_capacity] != -1:
            return dp[index][rest_capacity]
        
        new_rest_capacity = rest_capacity - weights[index]
        profit_include = 0
        if new_rest_capacity >= 0:
            profit_include = profits[index] + helper(index + 1, new_rest_capacity)
            
        profit_exclude = helper(index + 1, rest_capacity)
        dp[index][rest_capacity] = max(profit_include, profit_exclude)
        return dp[index][rest_capacity]

    return helper(0, capacity)
```

* Solution 2: Bottom Up, Time complexity O(NC), Space complexity O(NC)
```
def solve_knapsack(profits, weights, capacity):
    dp = [ [0 for i in range(capacity + 1)] for j in range(len(profits))]
    for index in range(len(profits)):
        for used_capacity in range(capacity + 1):
            if used_capacity == 0:
                continue
            if index == 0:
                dp[index][used_capacity] = profits[index]
                continue
            
            profit_exclude = dp[index-1][used_capacity]
            
            profit_include = 0            
            capacity_minus_curr = used_capacity - weights[index]
            if capacity_minus_curr >= 0:
                profit_include = profits[index] + dp[index-1][capacity_minus_curr]

            dp[index][used_capacity] = max(profit_exclude, profit_include)
    return dp[-1][-1]
```

### Equal Subset Sum
Given a set of positive numbers, find if we can partition it into two subsets such that the sum of elements in both subsets is equal.

Return `True` of `False`.

```
def can_partition(num):
    sum_all = sum(num)
    if sum(num) % 2 != 0:
        return False
    sum_half = sum_all // 2
    dp = [False for i in range(sum_half + 1)]
    
    for n in num:
        for sum_a, reachable in enumerate(dp):
            if sum_a == n:
                reachable = True
            if reachable and sum_a + n <= sum_half:
                dp[sum_a + n] = True
            if dp[-1]:
                return True

    return False
```
