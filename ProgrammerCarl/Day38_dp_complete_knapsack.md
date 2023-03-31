# 518. Coin Change II
Core idea: \ 
complete knapsack problem which use dynamic programming algorithm

Steps:\
(1) Determine the `dp` array and the meaning of its subscripts
- `dp[i]` = the number of ways to fill bag when bag_limit = i

(2) Determine the recurrence formula
- `dp[i] = dp[i] + dp[i-coin]`
```
current no_of_way 
= no_of_way when we don't consider this coin_amount + 
  no_of_way when we do consider this coin_amount 
```

(3) The initialisation of the `dp` array
- `dp[0] = 1`

(4) Determine the traversal order
- We must traverse from the first bag weight as each kind of coin can be included infinite number of times.
- Besides:
    - 如果求组合数就是外层for循环遍历物品，内层for遍历背包。
    - 如果求排列数就是外层for遍历背包，内层for循环遍历物品。

(5) Derive the resulted `dp` array and check it by printing
- For example, when `amount = 5, coins = [1,2,5]`, the `dp` should be `[1, 1, 2, 2, 3, 4]`.

```PYTHON
def change(self, amount: int, coins: List[int]) -> int:
    dp = [0] * (amount + 1)
    dp[0] = 1

    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] += dp[i-coin]

    return dp[amount]
```

# Reference
[代码随想录 - 518.零钱兑换II](https://programmercarl.com/0518.零钱兑换II.html#思路)

TODO: leetcode 377