# 416. Partition Equal Subset Sum
### way 1: dynamic programming
Core idea: \
Transform the question into knapsack problem which use dynamic programming algorithm.

For example,
```
nums = [1, 5, 11, 5]
Bag limit = sum(nums) // 2 = 11
weight[i] = value[i] = nums[i]
```

We try to fill the bag by using numbers from `nums` array, and the final weight of the bag will never exceed 11 as we set the bag limit as 11. If the `final weight = 11`, which means we can find a combination of numbers that summed to 11 (also means we can partition the `nums` array into two subsets such that the sum of the elements in both subsets is equal). If the `final weight < 11`, which means we can partition the `nums` array into two subsets such that the sum of the elements in both subsets are as close as possible but the sums are no equal.

Steps:\
(1) Determine the `dp` array and the meaning of its subscripts
- `dp[i]` = the maximum weight of items in the bag when the bag limit is `i`

(2) Determine the recurrence formula
- `dp[i] = max(dp[i], dp[i - weight[j]] + value[j])`

(3) The initialisation of the `dp` array
- `dp[0] = 0, ... , dp[half_total] = 0`

(4) Determine the traversal order
- We must traverse from the last bag weight so that the top left corner will not be overwrote

(5) Derive the resulted `dp` array and check it by printing
- For example, when `nums = [1, 5, 11, 5]`, the `dp` should be `[0, 1, 1, 1, 1, 5, 6, 6, 6, 6, 10, 11]`.

Time complexity: O(n<sup>2</sup>)
Space complexity: O(n)
```PYTHON
def canPartition(self, nums: List[int]) -> bool:

    total = sum(nums)
    if not (total // 2) == (total / 2):
        return False

    half_total = total // 2

    dp = [0] * (half_total + 1)
    for num in nums:
        for i in range(half_total, num - 1, -1):
            dp[i] = max(dp[i], dp[i - num] + num)

    return dp[half_total] == half_total
```




