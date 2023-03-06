# 455. Assign Cookies
```PYTHON
def findContentChildren(self, g: List[int], s: List[int]) -> int:
    g.sort()
    s.sort()
    g_pointer , s_pointer = 0, 0
    while(g_pointer < len(g) and s_pointer < len(s)):
        if s[s_pointer] >= g[g_pointer]:
            g_pointer += 1
            s_pointer += 1
        else:
            s_pointer += 1

    return g_pointer
```

# 376. Wiggle Subsequence
![](20230306210619.png)
### way 1: my version
```PYTHON
def wiggleMaxLength(self, nums: List[int]) -> int:
    if len(nums) == 1:
        return 1
    
    if len(nums) == 2 and nums[0] != nums[1]:
        return 2

    neg_counter, pos_counter = 1, 1
    last_num = nums[0]
    for i in range(1, len(nums)):

        # first difference is positive
        if pos_counter % 2 == 0:
            if nums[i] < last_num:
                pos_counter += 1
        else:
            if nums[i] > last_num:
                pos_counter += 1
    
        # first difference is negative
        if neg_counter % 2 == 0:
            if nums[i] > last_num:
                neg_counter += 1
        else:
            if nums[i] < last_num:
                neg_counter += 1

        last_num = nums[i]

    return max(neg_counter, pos_counter)
```
### way 2: programmer Carl's version
```PYTHON
def wiggleMaxLength(self, nums: List[int]) -> int:
    if len(nums) == 1:
        return 1
    
    curr_diff , pre_diff = 0, 0
    result = 1 
    for i in range(1, len(nums)):
        curr_diff = nums[i] - nums[i-1]
        if (pre_diff >= 0 and curr_diff < 0) or (pre_diff <= 0 and curr_diff > 0):
            result += 1
            pre_diff = curr_diff

    return result
```
TODO: 53. Maximum Subarray
