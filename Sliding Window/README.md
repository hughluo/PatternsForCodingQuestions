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


