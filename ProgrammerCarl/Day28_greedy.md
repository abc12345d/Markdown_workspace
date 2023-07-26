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
```PYTHON
def canJump(self, nums: List[int]) -> bool:
    if len(nums) == 1: return True
    
    curr = nums[0]
    for i in range(1, len(nums)):
        if curr == 0:
            return False

        curr -= 1

        if nums[i] > curr:
            curr = nums[i]

        if i + curr >= len(nums)-1:
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
