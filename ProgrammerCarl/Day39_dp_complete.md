# 322. Coin Change
Core idea: \ 
complete knapsack problem which use dynamic programming algorithm

Steps:\
(1) Determine the `dp` array and the meaning of its subscripts
- `dp[i]` = the fewest number of coins that we need to fill the bag when bag_limit = i

(2) Determine the recurrence formula
- `dp[i] = min(dp[i], dp[i-coin])`
```
current no_of_coins 
= no_of_coins  when we don't consider this coin_amount + 
  no_of_coins  when we do consider this coin_amount 
```

(3) The initialisation of the `dp` array
- `dp[0] = 0` and the rest of the `dp` array are set to the positive infinity number since we use `min()` in out recurrence formula

(4) Determine the traversal order
- We must traverse from the first bag weight as each kind of coin can be included infinite number of times.
- as we are asking the number of coins, it doesn't matter either we loop through the `coins` first or loop through the `bag` first. 

(5) Derive the resulted `dp` array and check it by printing
- For example, when `coins = [1,2,5], amount = 11`, the `dp` should be `[0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 3]`.

Time complexity: O(m*n), where m = len(coins) and n = amount + 1
Space complexity: O(n)
```PYTHON
def coinChange(self, coins: List[int], amount: int) -> int:

    dp = [float('inf')] * (amount + 1)
    dp[0] = 0

    for coin in coins:
        for i in range(coin, amount + 1):
            if i == coin:
                dp[i] = 1
            else:
                dp[i] = min(dp[i-coin] + 1, dp[i])
    
    return dp[amount] if dp[amount] != float('inf') else-1
```

# 279. Perfect Squares
Core idea: \ 
complete knapsack problem which use dynamic programming algorithm

Steps:\
(1) Determine the `dp` array and the meaning of its subscripts
- `dp[i]` = the least number of perfect square numbers that sum to i 

(2) Determine the recurrence formula
- `dp[i] = min(dp[i], dp[i - square] + 1)`
```
current no_of_perfect_square 
= no_of_perfect_square  when we don't consider this perfect_square + 
  no_of_perfect_square  when we do consider this perfect_square 
```

(3) The initialisation of the `dp` array
- we set the `dp[index] = 1` where index is a perfect square <= n
- `dp[0] = 0` and the rest of the `dp` array are set to the positive infinity number since we use `min()` in out recurrence formula

(4) Determine the traversal order
- We must traverse from the `square+1` to `n` as each perfect square can be included infinite number of times.
- as we are asking the number of perfect_square, it doesn't matter either we loop through the `perfect_square` first or loop through the `bag` first. 

(5) Derive the resulted `dp` array and check it by printing
- For example, when `n = 12`, the `dp` should be `[0, 1, 2, 3, 1, 2, 3, 4, 2, 1, 2, 3, 3]`.

Time complexity: O(x * y), where x = no_of_perfect_square < n and y = n
Space complexity: O(x + y)
```PYTHON
def numSquares(self, n: int) -> int:
    dp = [float('inf')] * (n+1)
    dp[0] = 0
    perfect_square = []

    for i in range(1,n+1):
        square = i ** 2
        if square > n:
            break
        dp[square] = 1
        perfect_square.append(square)

    for square in perfect_square:
        for i in range(square+1, n+1):
            dp[i] = min(dp[i], dp[i - square] + 1)

    return dp[n] 
```
