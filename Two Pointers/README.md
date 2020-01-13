# Two Pointers

## Examples 
### Quadruple Sum to Target
Given an array of unsorted numbers and a target number, find all unique quadruplets in it, whose sum is equal to the target number.
```
def search_quadruplets(arr, target):
    arr.sort()
    quadruplets = []
    for i in range(0, len(arr)-3):
        # skip same element to avoid duplicate quadruplets
        if i > 0 and arr[i] == arr[i - 1]:
            continue
        for j in range(i + 1, len(arr)-2):
            # skip same element to avoid duplicate quadruplets
            if j > i + 1 and arr[j] == arr[j - 1]:
                continue
            search_pairs(arr, target, i, j, quadruplets)
    return quadruplets


def search_pairs(arr, target_sum, first, second, quadruplets):
    left = second + 1
    right = len(arr) - 1
    while (left < right):
        sum = arr[first] + arr[second] + arr[left] + arr[right]
        if sum == target_sum:  # found the quadruplet
            quadruplets.append(
                [arr[first], arr[second], arr[left], arr[right]])
            left += 1
            right -= 1
            while (left < right and arr[left] == arr[left - 1]):
                left += 1  # skip same element to avoid duplicate quadruplets
            while (left < right and arr[right] == arr[right + 1]):
                right -= 1  # skip same element to avoid duplicate quadruplets
        elif sum < target_sum:
            left += 1  # we need a pair with a bigger sum
        else:
            right -= 1  # we need a pair with a smaller sum
```

### Minimun Sort Window
Given an array, find the length of the smallest subarray in it which when sorted will sort the whole array.


```
# 1 From the beginning and end of the array, find the first elements that are out of the sorting order. The two elements will be our candidate subarray.
# 2 Find the maximum and minimum of this subarray.
# 3 Extend the subarray from beginning to include any number which is bigger than the minimum of the subarray.
# 4 Similarly, extend the subarray from the end to include any number which is smaller than the maximum of the subarray.

import math

def shortest_window_sort(arr):
    low, high = 0, len(arr) - 1
    # find the first number out of sorting order from the beginning
    while (low < len(arr) - 1 and arr[low] <= arr[low + 1]):
        low += 1

    if low == len(arr) - 1:  # if the array is sorted
        return 0

    # find the first number out of sorting order from the end
    while (high > 0 and arr[high] >= arr[high - 1]):
        high -= 1

    # find the maximum and minimum of the subarray
    subarray_max = -math.inf
    subarray_min = math.inf
    for k in range(low, high+1):
        subarray_max = max(subarray_max, arr[k])
        subarray_min = min(subarray_min, arr[k])

    # extend the subarray to include any number which is bigger than the minimum of the subarray
    while (low > 0 and arr[low-1] > subarray_min):
        low -= 1
    # extend the subarray to include any number which is smaller than the maximum of the subarray
    while (high < len(arr)-1 and arr[high+1] < subarray_max):
        high += 1

    return high - low + 1


def main():
    print(shortest_window_sort([1, 2, 5, 3, 7, 10, 9, 12]))
    print(shortest_window_sort([1, 3, 2, 0, -1, 7, 10]))
    print(shortest_window_sort([1, 2, 3]))
    print(shortest_window_sort([3, 2, 1]))


main()
```
