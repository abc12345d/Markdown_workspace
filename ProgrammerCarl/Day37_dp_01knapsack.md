# 494. Target Sum
### way 1: backtracking without prunning
Running all the test cases with this way will lead to `time limit exceeded`.

Time complexity: O(2<sup>n</sup>) where n = len(nums)\
Space complexity: O(n)
```PYTHON
def findTargetSumWays(self, nums: List[int], target: int) -> int:
    def backtrack(total,index):
        nonlocal result
        if index ==(len(nums)-1):
            if total == target:
                result += 1
            return

        index += 1
        for symbol in [1, -1]:
            number = nums[index]
            number *= symbol
            total += number
            backtrack(total,index)
            total -= number

    

    result = 0
    backtrack(0,-1)
    return result 
```
### way 2: dynamic programming(recursion with memo)
This question is asking the number of different expressions that we can build, which evaluates to target.

According to the setting of questions, 
```PYTHON
f(target,index) = n number of different expr
f(target, index) = f(target - nums[index], index - 1) + f(target + nums[index], index - 1)

# when index = 0, 
result = 0
if accum == +nums[0]:
    result += 1
if accum == -nums[0]:
    result += 1
return result 

# nums = [1 2]
# f(2, 1)
# = f(0,0) + f(4,0)
# = 0

# f(3,1)
# = f(1,0) + f(5,0)

# nums = [1,1,1,1,1]
# f(3, 4)
# = f(2,3) + f(4,3) 
# = f(1,2) + f(3,2) + f(3,2) + f(5,2)
# = f(0,1) + 3*f(2,1) + 3* f(4,1) + f(6,1)
# = f(-1,0) +f(1,0) + 3*f(1,0) + 3*f(3,0) + 3*f(3,0) + 4*f(5,0)           + f(7,0)
# = 5
```
Time complexity: O(2n) where n = len(nums) \
Space complexity: O(n)
```PYTHON
def findTargetSumWays(self, nums: List[int], target: int) -> int:
    def dp(accum, index):

        if (accum,index) in memo.keys():
            return memo[(accum,index)]

        if index == 0:
            result = 0
            if accum == +nums[0]:
                result += 1
            if accum == -nums[0]:
                result += 1

            return result

        pos = dp(accum + nums[index], index - 1)
        neg = dp(accum - nums[index], index - 1)
        memo[(accum,index)] = pos + neg
        return pos + neg

    memo = {}
    return dp(target, len(nums)-1)
```

TODO:  474.Ones and zeros
