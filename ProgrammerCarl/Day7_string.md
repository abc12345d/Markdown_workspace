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

# 151. Reverse Words in a String

### way 1: 
Time complexity: O(n)
Space complexity: O(n)
```PYTHON
def reverseWords(self, s: str) -> str:
        
    pointer = 0
    word = ""
    res_list = []
    for i in range(len(s)):

        if s[i] == " " and word != "":
            res_list.append(word)
            word = ""
            continue
            
        if s[i] != " ": 
            word += s[i]

            if (i == len(s) - 1):
                res_list.append(word)

    res_list = res_list[::-1]
    res_str = ""
    for i in range(len(res_list) - 1):
        res_str += res_list[i] + " "
    res_str += res_list[-1]
    
    return res_str
```

### way 2: use fast & slow pointer to remove excessive space, then reverse 
Space complexity: O(1)
```PYTHON
# remove excessive space character
s_list = list(s)
slow = 0
fast = 0
last_char = -1
while slow <= fast and fast < len(s):
    
    while (fast < len(s) and s_list[fast] == " "):
        fast += 1
        
    while (slow <= fast and fast < len(s) and s_list[fast] != " " ):
        s_list[fast], s_list[slow] = s_list[slow], s_list[fast]
        last_char = slow
        slow += 1
        fast += 1
    
    slow += 1
    fast += 1
```

#  剑指Offer58-II.左旋转字符串 


##### TODO: 