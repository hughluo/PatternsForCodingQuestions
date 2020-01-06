# 0/1 Knapsack

* Always start crack the question by explaining Brute-force and complexity (i.e For Knapsack, the brute-force approach has O(2^n) (power of two) exponential time complexity, since we have to choose yes/no for each element)
* Top down with memoization
* Bottom up using an n-dimension array


## Bruteforce
## Top Down with Memoization
## Bottom Up
* can often furthermore improve the space complexity, compress an 2D-array to 1D-array
* when doing this compression, we may need to update array **reversely** to avoid error. (i.e. if we update 2nd and then 3rd, when we are at 2nd and we already updated 3rd, therefore, when we are at 3rd, 3rd is modified and different than the last round)



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

### Path with Maximum Sum
Find the path with the maximum sum in a given binary tree. Write a function that returns the maximum sum. A path can be defined as a sequence of nodes between any two nodes and doesn’t necessarily pass through the root.
```
import math


class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def find_maximum_path_sum(root):
    res = float('-inf')
    def helper(curr):
        if curr is None:
            return 0
        val_left = helper(curr.left)
        val_right = helper(curr.right)
        nonlocal res
        res = max(res, curr.val, curr.val + val_left + val_right)
        val_chain = curr.val + max(val_left, val_right)
        return val_chain

    helper(root)
    return res

def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    print("Maximum Path Sum: " + str(find_maximum_path_sum(root)))
    assert find_maximum_path_sum(root) == 6
    
    root.left.left = TreeNode(1)
    root.left.right = TreeNode(3)
    root.right.left = TreeNode(5)
    root.right.right = TreeNode(6)
    root.right.left.left = TreeNode(7)
    root.right.left.right = TreeNode(8)
    root.right.right.left = TreeNode(9)
    print("Maximum Path Sum: " + str(find_maximum_path_sum(root)))
    assert find_maximum_path_sum(root) == 31

    root = TreeNode(-1)
    root.left = TreeNode(-3)
    print("Maximum Path Sum: " + str(find_maximum_path_sum(root)))
    assert find_maximum_path_sum(root) == -1

if __name__ == "__main__":
    main()
```
