# 977. Squares of a Sorted Array
Core idea: two pointers \
Use two pointers, i.e. `pos` points to the last element ( largest number) and `neg` points to the first element (smallest number), to loop through the `nums` array.

For each iteration, we fill the `res` array from the end with the bigger number between the square of both nums[neg] and num[pos].

```PYTHON
def sortedSquares(self, nums: List[int]) -> List[int]:
        # loop invariant: [neg,pos]
        neg = 0
        pos = len(nums) - 1
        res = [-1000000] * len(nums) 
        counter = len(nums) -1

        while(neg <= pos):
            neg_sqr = nums[neg] ** 2
            pos_sqr = nums[pos] ** 2

            if neg_sqr <= pos_sqr:
                res[counter] = pos_sqr
                pos -= 1
            else:
                res[counter] = neg_sqr
                neg += 1

            counter -= 1
        
        return res
```

# 209. Minimum Size Subarray Sum
Core idea: sliding window \
Use two pointers `left` and `right` to indicate the left and right ends of the current range, i.e. sliding window. In each step, we either move `left` or `right` to the next range. 

Instead of counting the sum of the current range for every iteration, we maintained `accum` to store the sum of the range. By only adding or subtracting one value for each iteration, we can save lots of computational power, especially when the size of `nums` is enormous.

However, there are few points to note (marked in the code): \
(p 1) we must first update `accum`, then move our `left` pointer\
(p 2) we must first move our `right` pointer, then update `accum`\
(p 3) we have to add this if-statement, otherwise we'll get an `IndexError: list index out of range` Error for this line `accum += nums[right]`.

Both pointers `left` and `right` will iterate the `nums` array only once; therefore, the time complexity of this algorithm is O(2n).

```PYTHON
def minSubArrayLen(self, target: int, nums: List[int]) -> int:
    # loop invariant: left <= right and right < len(nums)
    left = 0
    right = 0
    min_len = float('inf')
    accum = nums[0]
    
    while(left <= right and right < len(nums)):
        if (accum >= target):
            min_len = min((right + 1 - left), min_len)
            # p 1
            accum -= nums[left]
            left += 1
        else: 
            # p 2
            right += 1
            if (right >= len(nums)):    # p 3
                break
            accum += nums[right]
        
    return min_len if min_len != float('inf') else 0
```

# 59. Spiral Matrix II
Core idea: loop invariant\
Through observing the example, we can see that the matrix can be filled in 4 types of directions:\
(1) from left to right to fill the top row\
(2) from up to down to fill the right column\
(3) from right to left to fill the bottom row\
(4) from down to up to fill the left column

We iteratively shrink the available space to fill by specifying `row_start`,`row_end`,`col_start` and `col_end`, and maintaining the loop invariant that `start` always <= `end`.

![](20230203002603.png)  
Both the time complexity and space complexity are O(n by n).
```PYTHON
def generateMatrix(self, n: int) -> List[List[int]]:
        # loop invariant:
        # row_start <= row_end
        # col_start <= col_end

        row_start = 0                           # top row index
        row_end = n - 1                         # bottom row index
        col_start = 0                           # left column index 
        col_end = n - 1                         # right column index
        res = [[None] * n for _ in range(n)]
        counter = 1

        while(row_start <= row_end and col_start <= col_end):

            # fill top row
            for j in range(col_start, col_end + 1):
                res[row_start][j] = counter
                counter += 1
            row_start += 1

            # fill right column
            for i in range(row_start, row_end + 1):
                res[i][col_end] = counter
                counter += 1
            col_end -= 1

            # fill bottom row
            for j in range(col_end, col_start - 1, -1):
                res[row_end][j] = counter
                counter += 1
            row_end -= 1

            # fill left column
            for i in range(row_end, row_start - 1, -1):
                res[i][col_start] = counter 
                counter += 1
            col_start += 1

        return res
```
