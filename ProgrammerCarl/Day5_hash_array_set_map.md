# 242. Valid Anagram

### way 1: with hashmap
Time complexity: O(n)\
Space complexity: O(1), the max size of hashmap `s_dict` is 26 regardless of the size of input
```PYTHON
def isAnagram(self, s: str, t: str) -> bool:
    
    if (len(s) != len(t)):
        return False

    s_dict = {} 
    for i in list(s):
        if i in s_dict:
            s_dict[i] += 1
        else:
            s_dict[i] = 1
    
    for i in list(t):
        if i in s_dict:
            if s_dict[i] == 0:
                return False
            else:
                s_dict[i] -= 1
        else:
            return False

    return True 
```
### way 2: with hash table
Time complexity: O(n)\
Space complexity: O(1), the max size of hash table `hash_table` is 26 regardless of the size of input
```PYTHON
def isAnagram(self, s: str, t: str) -> bool:
        
    if (len(s) != len(t)):
        return False

    # where index 0 represents alphabet a ...
    # index 25 represents alphabet z
    # and the value represents the number of alphabet
    hash_table = [0] * 26

    for i in s:
        hash_table[ord(i) - ord("a")] += 1 

    for i in t:
        if hash_table[ord(i) - ord("a")] <= 0:
            return False
        else:
            hash_table[ord(i) - ord("a")] -= 1
        
    return True
```

# 349. Intersection of Two Arrays

### way(1): with hash set
```PYTHON
def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

    set1 = set(nums1)
    res = set()

    for i in nums2:
        if i in set1:
            res.add(i)

    return list(res)
```
### way(2): with hash table
```PYTHON
def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

    hash_table = [0] * 1001
    res_list = []

    for i in nums1:
        hash_table[i] += 1
    
    for i in nums2:
        if hash_table[i] > 0:
            res_list.append(i)
            hash_table[i] = 0
        
    return res_list
```

# 202. Happy Number
Use hash set() to memorise the appeared sum. Return False if we meet any of the appeared sum again, otherwise the while-loop will loop endlessly.
```PYTHON
def isHappy(self, n: int) -> bool:
    def calculate_sum(n):
        digits = []
        while (n > 0):
            digits.append(n % 10)
            n = n // 10 
        
        return sum(digits)

    appeared = set()
    while n not in appeared:
        appeared.add(n)
        n = calculate_sum(n)
        if (n == 1):
            return True

    return False
```

# 1. Two Sum
### way 1: with hash map
Time complexity: O(n)
Space complexity: O(n)
```PYTHON
def twoSum(self, nums: List[int], target: int) -> List[int]:
    hashMap = {}
    for i, v in enumerate(nums):
        if (target - v) in hashMap:
            return [hashMap[target - v], i]
        else:
            hashMap[v] = i
```
### way 2: brute-force
Time complexity: O(n<sup>2</sup>)\
Space complexity: O(1)
```PYTHON
def twoSum(self, nums: List[int], target: int) -> List[int]:
    for i in range(len(nums)-1):
        left = i
        right =len(nums)-1
        while (right>left):
            if (nums[left]+nums[right] == target):
                return [left,right]
            else:
                right -=1
```