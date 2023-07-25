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
### way 1: greedy
![](20230306210619.png)
<img width="810" alt="image" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/dbb609f6-ca56-4db0-9bf5-4b4c335fa353">

```PYTHON
def wiggleMaxLength(self, nums: List[int]) -> int:
    prev_diff = 0
    result = 1
    for index in range(1,len(nums)):
        curr_diff = nums[index] - nums[index-1]
        if (curr_diff > 0 and prev_diff <= 0) or (curr_diff < 0 and prev_diff >= 0):
            result += 1

            # below line must inside the if-statement
            # otherwise, fail case: nums = [1,7,4,5,5,7]
            prev_diff = curr_diff

    return result
```

### way 2: dp
(1) Determine the dp array and the meaning of its subscripts\
`up` = the length of the longest wiggle subsequence where the difference of last two numbers is positive\
`down` = the length of the longest wiggle subsequence where the difference of last two numbers is negative

(2) Determine the recurrence formula\
`up = down + 1`\
`down = up + 1`

(3) The initialisation of the dp array\
`up = 1`\
`down = 1`

(4) Determine the traversal order\
Doesn't matter which order we traverse.

(5) Derive the final result \
`max(up,down)` 

```PYTHON
def wiggleMaxLength(self, nums: List[int]) -> int:
    up = 1
    down = 1

    for index in range(1,len(nums)):
        if nums[index] > nums[index - 1]:
            up = down + 1
        elif nums[index] < nums[index - 1]:
            down = up + 1

    return max(up, down)
```
TODO: 53. Maximum Subarray

# reference
[bilibili- 跑马拉松的程序员](https://www.bilibili.com/video/BV1EY4y1H79j/?spm_id_from=333.337.search-card.all.click&vd_source=acc545154bc52bac86d7eca5cf3da83e)
