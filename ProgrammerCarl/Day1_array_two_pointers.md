# Array
An array is a collection of similar data elements stored at contiguous regions of memory. Each data element can be accessed directly by using its index number (In most programming languages, array indexes start at zero, aka zero-based indexing).

![array](./images/array.png)

### Characteristic of Array
Since data elements are stored in the contiguous memory location, the worst case time complexity of insertion and deletion in an array is O(n), as all other data elements may have to be moved for the operations.

### Reference
[代码随想录 - 数组理论基础](https://programmercarl.com/数组理论基础.html#数组理论基础)

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

![](./images/20230201160735.png)  

Time complexity: \
Binary search reduces its search space by half in every iteration. Hence, the worst-case time complexity is O(log n), and the best-case time complexity is O(1).

Extra:\
To avoid integer overflow while calculating `mid`, we can change our `mid` calculation into 
```PYTHON
mid = left + (right - left) // 2
```
However, in Python, we do not need to worry about integer overflow because Python3 integers can be arbitrarily large.

# 27. Remove Element

### Two pointers solution: O(n)
```PYTHON
def removeElement(self, nums: List[int], val: int) -> int:
    # loop invariant 1: 
    # elements in nums array where index < left are not equal to val
    # elements in nums array where index > right are equal to val
    # loop invariant 2:
    # search space [left,right]

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
