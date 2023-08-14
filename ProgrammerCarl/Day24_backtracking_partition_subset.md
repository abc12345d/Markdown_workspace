# 93. Restore IP Addresses

<img width="805" alt="20230227170728" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/c083e5ff-0fa6-4276-b5a1-0a41edf38170">

### way 1: backtracking with startIndex and lastIndex

```PYTHON
def restoreIpAddresses(self, s: str) -> List[str]:
    def isValidSubIP(s):
        if len(s) > 1 and s[0] == '0':
            # detect leading zeros
            return False
        if s and int(s) > 255:
            # more than 255
            return False
        return True

    def backtrack(path, s, startIndex):
        if len(s) == 0:
            if len(path) == 4:
                res_list.append(".".join(path))
            return

        lastIndex = min(startIndex + 3, len(s))
        for i in range(startIndex,lastIndex):
            subIP = s[startIndex:i+1]

            if not isValidSubIP(subIP):
                continue

            if len(path) == 4:
                return
            
            path.append(subIP)
            backtrack(path, s[i+1:], startIndex)
            path.pop()

    res_list = []
    backtrack([], s, 0)
    return res_list
```

### way 2: backtracking with smaller string

```PYTHON
def restoreIpAddresses(self, s: str) -> List[str]:
    def isValidSubIP(s):
        if len(s) > 1 and s[0] == '0':
            # detect leading zeros
            return False
        if s and int(s) > 255:
            # more than 255
            return False
        return True

    def backtrack(path, s):
        if len(s) == 0:
            if len(path) == 4:
                res_list.append(".".join(path))
            return

        lastIndex = min(4, len(s)+1)
        for i in range(1,lastIndex):
            subIP = s[:i]

            if not isValidSubIP(subIP):
                continue

            if len(path) == 4:
                continue
            
            path.append(subIP)
            backtrack(path, s[i:])
            path.pop()

    res_list = []
    backtrack([], s)
    return res_list
```

# 78. Subsets

<img width="776" alt="20230228113427" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/5d57d1f9-6cf0-4ead-9f4b-135a41fd6a49">

For backtracking in problem involved subsets, we remove duplicates at the branch level by using `startIndex` like in [backtracking in problem involved combination](./Day23_backtracking_combination_partition.md/#40-combination-sum-ii). However, instead of only recording the `path` when we reach the leaf node of the state-space tree, we record the `path` at every node while we traverse the state-space tree using backtracking.

```PYTHON
def subsets(self, nums: List[int]) -> List[List[int]]:
    def backtrack(startIndex, nums, path):
        res_list.append(path[:])

        if startIndex > len(nums):
            return

        for i in range(startIndex, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, nums, path)
            path.pop()

    res_list = []
    backtrack(0, nums, [])
    return res_list
```

# 90. Subsets II 
### way 1: with an extra array `used`

```PYTHON
def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
    def backtrack(startIndex, nums, path, used):
        res_list.append(path[:])

        if startIndex > len(nums):
            return

        for i in range(startIndex, len(nums)):
            
            if i > 0 and nums[i] == nums[i-1] and used[i-1] == False:
                continue

            path.append(nums[i])
            used[i] = True
            backtrack(i + 1, nums, path, used)
            used[i] = False
            path.pop()


    res_list = []
    used = [False] * len(nums)
    backtrack(0, sorted(nums), [], used)
    return res_list
```

### way 2: without any extra array

```PYTHON
def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
    def backtrack(startIndex, nums, path):
        res_list.append(path[:])

        if startIndex > len(nums):
            return

        for i in range(startIndex, len(nums)):
            
            if i > startIndex and nums[i] == nums[i-1]:
                continue

            path.append(nums[i])
            backtrack(i + 1, nums, path)
            path.pop()


    res_list = []
    backtrack(0, sorted(nums), [])
    return res_list
```
