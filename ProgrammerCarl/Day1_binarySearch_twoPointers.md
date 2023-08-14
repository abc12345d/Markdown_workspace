# 704. Binary Search
Core idea: the boundary of search space

The definition of boundaries will affect the loop invariant of our binary search algorithm, which in turn decides how we update our left & right variables and when we exit the while loop. The most common type of boundaries are [left, right] and [left, right). 

### [left,right] 
```PYTHON
def search(self, nums: List[int], target: int) -> int:
    left = 0
    right = len(nums) - 1

    # loop invariant: left <= tight
    while left <= right:
        mid = (left + right) // 2 

        if (nums[mid] == target):
            # target found, return its index
            return mid 

        elif (nums[mid] < target):
            # cut lower part of current search space
            left = mid + 1  

        else:
            # cut upper part of current search space
            right = mid - 1
    
    return -1 # target not found
```

### [left,right)

```PYTHON
def search(self, nums: List[int], target: int) -> int:
    left = 0
    right = len(nums)

    # loop invariant: left < right
    while left < right:
        mid = (left + right) // 2

        if (nums[mid] == target):
            # target found, return its index
            return mid 

        elif (nums[mid] < target):
            # discard lower part of current search space
            left = mid + 1  

        else:
            # discard upper part of current search space
            right = mid 
    
    return -1 # target not found
```

For the condition of while loop and loop invariant, we can always think of the case where there is only one element in the `nums` array and we want to exit the loop after one execution

<img width="484" alt="20230201160735" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/6d7abf78-ef0b-474e-841a-9c764b715278">

Time complexity: \
Binary search reduces its search space by half in every iteration. Hence, the worst-case time complexity is O(log n), and the best-case time complexity is O(1).

Extra:\
To avoid integer overflow while calculating mid, we can change our mid calculation into `mid = left + (right - left) // 2`. However, in Python, we do not need to worry about integer overflow because Python3 integers can be arbitrarily large.

# 27. Remove Element

### Two pointers solution: O(n)

```PYTHON
def removeElement(self, nums: List[int], val: int) -> int:
    # loop invariant 1: 
    # elements in nums array where index < left are not equal to val
    # elements in nums array where index > right are equal to val
    # loop invariant 2:
    # search space = [left,right]

    left = 0
    # right is the index of last unchecked position
    right = len(nums)-1

    while(left <= right):
        if nums[left] == val:
            # found element should be removed
            # move it to the last unchecked position
            # update the index of last unchecked position aka right
            nums[left], nums[right] = nums[right], nums[left]
            right -= 1 

        else:
            left += 1
    
    # as the array is zero-based indexing and the exit condition of while loop, 
    # elements in this range [0:k-1] are not equal to val
    # elements in this range [k:len(nums)-1] are equal to val
    # left = the index of first occurance of val in nums array = k
    return left 
```

### Brute-force solution: O(n<sup>2</sup>)

```PYTHON
def removeElement(self, nums: List[int], val: int) -> int:

    nums_len = len(nums)
    # k is the number of elements which are not equal to val
    # initially, it is set to the length of nums array
    k = len(nums)

    for i in range(nums_len):
        while nums[i] == val and i < k:
            # found element should be removed, so update k
            k -= 1

            # move the rest elements of the nums array one position forward
            for j in range(i+1,nums_len):
                nums[j-1] = nums[j]

    return k
```

# 26. Remove Duplicates from Sorted Array
Core idea: slow & fast pointers
### Two pointers (slow & fast) solution: O(n)

```PYTHON
def removeDuplicates(self, nums: List[int]) -> int:
    if len(nums) == 1:
        return 1

    slow, fast = 1, 1
    while fast < len(nums):
        if not nums[fast] == nums[fast - 1]:
            nums[slow] = nums[fast]
            slow += 1
        fast += 1

    return slow
```

# 35. Search Insert Position
There are two possible endings for this question:\
(1) found target and return the index of target\
(2) found nothing and return the index where it would be if it were inserted in order

To tackle scenario 1, code in [704. Binary Search](#704-binary-search) is enough.
For scenario 2, we have to find the first index of the " > target " range. The exit condition of while loop is `left > right` and the shrinking logic of binary search will lead to `right` points to the last element of " < target " range and `left` points to the first element of " > target " range. Therefore, we can either return `right + 1` or `left`.

<img width="832" alt="20230201222957" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/e5cd92cf-6586-4b6d-a9c1-ad1372243371">

Other than the return value when the target is not found, the algorithm of this question is similar to [704. Binary Search](#704-binary-search). Hence they have the same time complexity, i.e. the worst-case time complexity is O(log n).

```PYTHON
def searchInsert(self, nums: List[int], target: int) -> int:
    # loop invariant 1:
    # search space = [left, right]

    left = 0
    right = len(nums) - 1

    while (left <= right):
        mid = (left + right) // 2

        if (nums[mid] == target):
            return mid
        elif (nums[mid] < target):
            left = mid + 1
        else:
            right = mid - 1
    
    # return right + 1 
    return left
```

# 34. Find First and Last Position of Element in Sorted Array
There are two ways to solve this problem:\
(1) use binary search to find the starting position(or ending position) of target, then loop from the position until no target value is found\
(2) use two binary search to find the starting position and ending position of target.\
The worst-case time complexities of both ways are O(log n), but way (2) is more bug-free since we use the same algorithm twice instead of one algorithm to find the first position and another algorithm to find the last position of target. Besides, way (1) have to consider more edge case.

### way (1a): find starting position 
To find the starting position of target, we have to separate the search space into two parts: " < target " and " >= target " by modifying the if statement inside the binary search. 

<img width="408" alt="20230202115427" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/26c88e0b-518d-456d-a6ed-ae4119633a07">

```PYTHON
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        
        def searchFirstTarget(nums, target):
            # loop invariant: [left, right]
            left = 0
            right = len(nums) - 1

            while(left <= right):
                mid = (left + right) // 2

                if (nums[mid] < target):
                    left = mid + 1
                else:
                    right = mid - 1
            
            # due to the if condition,
            # left is the possible starting position of target (see image)
            return left

        start = -1
        end = -1
        possible_start = searchFirstTarget(nums,target)

        # edge case
        if possible_start >= len(nums) or len(nums) == 0:
            return [start,end]

        
        if nums[possible_start] == target:
            start = possible_start
            counter = possible_start + 1

            # loop from the start position until no target value is found
            while(counter < len(nums) and (nums[counter] == target)):
                counter += 1
            end = counter - 1
        
        return [start,end]
```

### way (1b): find ending position 
To find the ending position of target, we have to separate the search space into two parts: " <= target " and " > target " by modifying the if statement inside the binary search. 

<img width="402" alt="20230202120856" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/582ed772-b2ba-412e-9a53-8c433768e194">

```PYTHON
def searchRange(self, nums: List[int], target: int) -> List[int]:
        
        def searchEndTarget(nums, target):
            # loop invariant: [left, right]
            left = 0
            right = len(nums) - 1

            while(left <= right):
                mid = (left + right) // 2

                if (nums[mid] <= target):
                    left = mid + 1
                else:
                    right = mid - 1
            
            # due to the if condition,
            # right is the possible ending position of target (see image)
            return right

        start = -1
        end = -1
        possible_end = searchEndTarget(nums,target)

        # edge case
        if possible_end < 0:
            return [start,end]

        if nums[possible_end] == target:
            end = possible_end
            counter = possible_end - 1

            # loop from the ending position until no target value is found
            while(counter >= 0 and (nums[counter] == target)):
                counter -= 1
            start = counter + 1
        
        return [start,end]
```

### way (2): two binary search
What we do after we have found the target will decide the positions of the `left` and `right` pointers (after exiting from the while loop).

<img width="851" alt="20230202123745" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/d755f7a1-0320-496e-8987-d5cbebb82053">

```PYTHON
def searchRange(self, nums: List[int], target: int) -> List[int]:
        
        def searchFirstTarget(nums, target):
            # loop invariant: [left, right]
            left = 0
            right = len(nums) - 1
            isFound = False

            while(left <= right):
                mid = (left + right) // 2

                if (nums[mid] == target):
                    isFound = True
                    right = mid - 1
                elif (nums[mid] < target):
                    left = mid + 1
                else:
                    right = mid -1
                    
            return left if isFound else -1

        def searchEndTarget(nums, target):
            # loop invariant: [left, right]
            left = 0
            right = len(nums) - 1
            isFound = False

            while(left <= right):
                mid = (left + right) // 2

                if (nums[mid] == target):
                    isFound = True
                    left = mid + 1
                elif (nums[mid] < target):
                    left = mid + 1
                else:
                    right = mid -1
                    
            return right if isFound else -1

        return [searchFirstTarget(nums, target),searchEndTarget(nums, target)]
```

