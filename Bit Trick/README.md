# Bit Trick
## Tricks
* x & (x-1) equals x with its lowest set bit erased. 101100 -> 101000
* x & ~(x-1) equals x with only its lowest set bit. 101100 -> 000100

## Example
### Count how many set bits
Count how many '1' bits in a word.
Time complexity: O(k) where k is the '1' bits in the word
```
def count_set_bit(x: int) -> int:
    count = 0
    while x != 0:
        count += 1
        x = x & (x-1)
    return count
```
