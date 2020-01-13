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
