# 0/1 Knapsack

## Examples
### 0/1 Knapsack
Given two integer arrays to represent weights and profits of ‘N’ items, find a subset of these items which will give us maximum profit such that their cumulative weight is not more than a given number ‘C’. Each item can only be selected once, which means either we put an item in the knapsack or we skip it.

Return the maximum profits.

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
    
