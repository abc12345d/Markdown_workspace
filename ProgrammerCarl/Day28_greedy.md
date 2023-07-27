# 122. Best Time to Buy and Sell Stock II
### way 1: greedy
![](./images/20230306231408.png)
```PYTHON
def maxProfit(self, prices: List[int]) -> int:

    profit = 0
    for i in range(1, len(prices)):
        curr_diff = prices[i] - prices[i-1]
        if curr_diff > 0:
            profit += curr_diff

    return profit
```
### way 2: dp
```PYTHON
def maxProfit(self, prices: List[int]) -> int:
    if len(prices) <= 1: return 0

    # maxProfit earned when hold/unhold the stock on i-th day
    hold = [0] * len(prices)
    unhold = [0] * len(prices)

    hold[0] = -prices[0]
    for day in range(1,len(prices)):
        # two situations may lead to hold stock on day:
        # hold since yesterday
        # unhold but buy on today
        maxProfitAfterBuying = unhold[day-1] - prices[day]
        hold[day] = max(hold[day-1], maxProfitAfterBuying)

        # two situations may lead to unhold stock on day:
        # unhold since yesterday
        # hold but sell on today
        maxProfitAfterSelling = hold[day-1] + prices[day] 
        unhold[day] = max(unhold[day-1], maxProfitAfterSelling)

    return unhold[-1]
```

# 55. Jump Game
### way 1: greedy
```PYTHON
def canJump(self, nums: List[int]) -> bool:
    if len(nums) == 1: return True
    
    availableStep = nums[0]
    for i in range(1, len(nums)):
        if availableStep == 0:
            return False

        availableStep -= 1

        if nums[i] > availableStep:
            availableStep = nums[i]

        # early termination
        if i + availableStep >= len(nums)-1:
            return True
```
### way 2: greedy
stand at the jumpFromIndex and always jump to jumpToIndex which can achieve biggest move until reaching the last element or reaching somewhere there is no available steps
```PYTHON
def canJump(self, nums: List[int]) -> bool:

    jumpFromIndex = 0 
    while jumpFromIndex < len(nums) - 1:
        jumpToIndex = -1
        maxReachable = 0

        for idx in range(jumpFromIndex + 1, nums[jumpFromIndex] + jumpFromIndex + 1 ):
            if idx >= len(nums) - 1:  return True
            
            if nums[idx] + idx >= maxReachable:
                jumpToIndex = idx
                maxReachable = nums[idx] + idx

        if -1 == jumpToIndex :
            return False
        jumpFromIndex = jumpToIndex

    return True
```

# 45. Jump Game II
### way 1: 
time complexity: O(n<sup>k</sup>), where k = the minimum number of jumps to reach nums[n - 1] from nums[0]\
space complexity: O(1)
```PYTHON
def jump(self, nums: List[int]) -> int:
    if len(nums) == 1: return 0

    result = 0
    last_index = len(nums) - 1
    while last_index > nums[0]:
        for i in range(last_index + 1):
            if i + nums[i] >= last_index:
                last_index = i
                result += 1
                break

    return result + 1
```
### way 2: leetcode's greedy version
![](./images/20230307114944.png)(image from leetcode)

time complexity: O(n)\
space complexity: O(1)
```PYTHON
def jump(self, nums: List[int]) -> int:
    
    result = 0
    curr_end = 0
    max_reachable_index = nums[0]
    for i in range(len(nums)-1):
        max_reachable_index = max(max_reachable_index, nums[i] + i)
        if i == curr_end:
            curr_end = max_reachable_index
            result += 1
    
    return result
```

# reference
[bilibili - 林先先森](https://www.bilibili.com/video/BV1HY4y1S7WB/?spm_id_from=333.337.search-card.all.click&vd_source=acc545154bc52bac86d7eca5cf3da83e)
