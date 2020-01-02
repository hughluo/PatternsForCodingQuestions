# Sliding Window
## Procedure

* first, figure out if fixed window_size?
* current window_size is window_end - window_start + 1
* For fixed window_size, if window_end >= window_size , we need to shrink window by one.
* For not fixed window_size, the shrink will be triggered when the constraints is not satisfied.
```
def sliding_window_for_fixed_window_size():
    init_variables()
    
    for window_end in range(len(arr)):
        grow_window()
        
        if window_end >= window_size:
            assert window_end - window_start + 1 == window_size + 1
            shrink_window()
        
        update_result()
    return res
```

```
def sliding_window_for_not_fixed_window_size():
    init_variables()
    
    for window_end in range(len(arr)):
        grow_window()
        
        if not constraints_satisfied:
            shrink_window()
        
        update_result()
    return res
```

## Tipps
* collections.Counter will be useful.

## Examples
### Maximum Sum Subarray of Size K
Given an array of positive numbers and a positive number ‘k’, find the maximum sum of any contiguous subarray of size ‘k’.
```
def max_sub_array_of_size_k(k, arr):
    window_start = 0
    res = 0
    curr_sum = 0
    
    for window_end in range(len(arr)):
        num_end = arr[window_end]
        curr_sum += num_end
        
        if window_end >= k:
            num_start = arr[window_start]
            curr_sum -= num_start
            window_start += 1
        
        if window_end >= k - 1:
            res = max(res, curr_sum) 
    return res
```

### String Anagrams
Given a string and a pattern, find all anagrams of the pattern in the given string.
Return a list of starting indices of the anagrams of the pattern in the given string.
```
from collections import Counter
def find_string_anagrams(str, pattern):
    result_indexes = []
    window_start = 0
    matched = 0
    pattern_counter = Counter(list(pattern))
    for window_end in range(len(str)):
        char_end = str[window_end]
        if char_end in pattern_counter:
            pattern_counter[char_end] -= 1
        if pattern_counter[char_end] == 0:
            matched += 1
    
        if window_end >= len(pattern):
            assert window_end - window_start + 1 == len(pattern) + 1
            char_start = str[window_start]
            if char_start in pattern_counter:
                if pattern_counter[char_start] == 0:
                    matched -= 1
                pattern_counter[char_start] += 1
      
            window_start += 1
    
        if matched == len(pattern):
            result_indexes.append(window_start)

    return result_indexes
```
