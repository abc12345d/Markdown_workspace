# 344. Reverse String
```PYTHON
def reverseString(self, s: List[str]) -> None:
    """
    Do not return anything, modify s in-place instead.
    """
    left = 0
    right = len(s) - 1

    while(left <= right):
        s[left], s[right] = s[right], s[left]
        left += 1
        right -=
```

# 541. Reverse String II
```PYTHON
def reverseStr(self, s: str, k: int) -> str:
    
    for i in range(0,len(s),2*k):
        s = s[:i] + s[i:i+k][::-1] + s[i+k:]
    
    return s
```

# 剑指 Offer 05. 替换空格
```PYTHON
def replaceSpace(self, s: str) -> str:

    res = ""
    for i in range(len(s)):
        if s[i] == " ":
            res += "%20"
        else:
            res += s[i]
        
    return res
```


