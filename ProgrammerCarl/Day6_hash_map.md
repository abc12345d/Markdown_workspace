# 454. 4Sum II

### use hash map
```PYTHON
def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:

        sum_dict = {}
        for i in nums1:
            for j in nums2:
                if i + j in sum_dict:
                    sum_dict[i + j] += 1
                else:
                    sum_dict[i + j] = 1
        
        count = 0
        for k in nums3:
            for l in nums4:
                if(0 -  k - l) in sum_dict:
                    count += sum_dict[0 - k - l]
        
        return count
```

# 383. Ransom Note

Time complexity: O(n)
Space complexity: O(1)
### use hash table
```PYTHON
def canConstruct(self, ransomNote: str, magazine: str) -> bool:

    # hash table where index corresponds to alphabet
    # value corresponds to number of appeared in magazine
    alpha_records = [0] * 26

    for alpha in magazine:
        alpha_records[ord(alpha) - ord('a')] += 1

    for alpha in ransomNote:
        alpha_records[ord(alpha) - ord('a')] -= 1

        if alpha_records[ord(alpha) - ord('a')] < 0:
            return False
    
    return True
```

### brute-force with nested for loop
Time complexity: O(n<sup>2</sup>)
Space complexity: O(n), longer str need more time and memory to convert to list.
```PYTHON
def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        ransomNote = list(ransomNote)
        for i in magazine:
            for j in ransomNote:
                if i == j:
                    ransomNote.remove(i)

                if len(ransomNote) == 0:
                    return True

        return False
```

# 15. 3Sum

### way 1: sort, then use hash map
```PYTHON
def threeSum(self, nums: List[int]) -> List[List[int]]:
    nums.sort()
    res = set()
    
    for i in range(len(nums)):
        target = 0 - nums[i]
        
        # key: target(0 - nums[i]) - nums[j]
        # value: j
        map = {}
        j =  i + 1
        while (j < len(nums)):
            if nums[j] in map:
                res.add((nums[i],target-nums[j],nums[j]))
            else:
                map[target-nums[j]] = j
            
            j += 1

    return res
```

### way 2: sort, then use two pointer
```PYTHON
def threeSum(self, nums: List[int]) -> List[List[int]]:
    nums.sort()
    res_list = []

    for i in range(len(nums)):
        if i > 0 and nums[i] == nums[i - 1]:
            continue

        left = i + 1
        right = len(nums) - 1
        while(left < right):
            total = nums[i] + nums[left] + nums[right]
            if (total == 0):
                res_list.append([nums[i], nums[left], nums[right]])
                left += 1
                right -= 1
            elif (total < 0):
                left += 1
            else:
                right -= 1

            while(right > left > i + 1 and nums[left] == nums[left - 1]):
                    left += 1
            while(len(nums) - 1 > right > left and nums[right] == nums[right + 1]):
                right -= 1

    return res_list
```
# 18. 4Sum