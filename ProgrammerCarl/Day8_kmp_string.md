# 28.Find the Index of the First Occurrence in a String
### way 1: brute-force
Time complexity: O(m * n) where m = len(haystack), n = len(needle)
Space complexity: O(1)
```PYTHON
def strStr(self, haystack: str, needle: str) -> int:
    for counter in range(len(haystack) - len(needle) + 1):
        if haystack[counter: counter + len(needle)] == needle:
            return counter

    return -1
```

### way 2: kmp algorithm
Time complexity:
Space complexity: 
