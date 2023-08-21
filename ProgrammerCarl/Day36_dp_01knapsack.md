# 416. Partition Equal Subset Sum

<img width="649" alt="image" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/b9a9bf8a-07e5-4076-9efd-1605a7665723">


## way 3: dynamic programming
Core idea: 

Transform the question into 1/0 knapsack problem which use dynamic programming algorithm.

E.g.
```
nums = [1, 5, 11, 5]
Bag limit = sum(nums) // 2 = 11
weight[i] = value[i] = nums[i]
```

We try to fill the bag by using numbers from `nums` array, and the final weight of the bag will never exceed 11 as we set the bag limit as 11. If the `final weight = 11`, which means we can find a combination of numbers that summed to 11 (also means we can partition the `nums` array into two subsets such that the sum of the elements in both subsets is equal). If the `final weight < 11`, which means we can partition the `nums` array into two subsets such that the sum of the elements in both subsets are as close as possible but the sums are no equal.

Steps:

1. Determine the `dp` array and the meaning of its subscripts
- `dp[i]` = the maximum weight of items in the bag when the bag limit is `i`

2. Determine the recurrence formula
- `dp[i] = max(dp[i], dp[i - weight[j]] + value[j])`

3. The initialisation of the `dp` array
- `dp[0] = 0, ... , dp[half_total] = 0`

4. Determine the traversal order
- We must traverse from the last bag weight so that the top left corner will not be overwrote

5. Derive the resulted `dp` array and check it by printing
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

# 1049. Last Stone Weight II
core idea:

Transform the question into 1/0 knapsack problem which use dynamic programming algorithm.

From the question, we knew that there is at most one stone left at the end of the game and we must return the smallest possible weight of the left stone (if there is one). We can get the smallest left stone by partitioning the stones into two piles of stones and smash any two stones from the two piles each time.
```

stones = [a,b,b,c,c,d,e,f]

abs((a pile of stones) - (a pile of stones))
= abs((a,b,c,d) - (b,c,e,f))

Let's say, we smash stone d and e, 
if d > e:
= abs((a,b,c, (d-e)) - (b,c,f))
if d < e:
= abs((a,b,c) - (b,c,(e-d),f))


We continue the smashing until only one stone left in each pile. 
Throughtout the smashing process, the difference between the two piles of stone 
will remain the same which is equal to the left_stone

= abs(stone_X - stone_Y )
= left_stone


```

To get two piles of stones which have smallest difference in weight, we must form two piles of stones where their weight summed to `sum(stones) // 2` or as close as possible to the number.


E.g. 
```
stones = [2,7,4,1,8,1]
Bag limit = sum(stones) // 2 = 11
weight[i] = value[i] = stones[i]
dp[i] = the maximum weight of stones in the bag when the bag limit is `i`
```

Time complexity: O(m * n) where m = len(stones), n = sum(stones)

Space complexity: O(n)

```PYTHON
def lastStoneWeightII(self, stones: List[int]) -> int:
    halfSum = sum(stones) // 2 

    # initialisation of dp array
    dp = [0] * (halfSum + 1)
    for stone in stones:
        # traverse from the last bag weight 
        # so that the top left corner will not be overwrote
        for bag in range(halfSum, stone-1, -1):
            dp[bag] = max(dp[bag], dp[bag-stone] + stone)

    return (sum(stones)- dp[halfSum]) - dp[halfSum]
```


# reference
- [Neetcode - Last Stone Weight II Leetcode 1049](https://www.youtube.com/watch?v=gdXkkmzvR3c)
