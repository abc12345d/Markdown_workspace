# Dynamic programming
Dynamic programming applies when the subproblems overlap—that is, when subproblems share subsubproblems. 

A dynamic-programming algorithm solves each subsubproblem just once and then saves its answer in a table, thereby avoiding the work of recomputing the answer every time it solves each subsubproblem.

# Steps to develop a dynamic-programing algorithm
Here we use [509. fibonacci number](./Day33_dynamic_programming.md/#509-fibonacci-number) as an example for these steps.

(1) Determine the `dp` array and the meaning of its subscripts
- `dp[i]` = the `i`th fibonacci number

(2) Determine the recurrence formula
- `dp[i] = dp[i - 1] + dp[i - 2]`

(3) The initialisation of the `dp` array
- `dp[0] = 0` , `dp[1] = 1`

(4) Determine the traversal order
- As `dp[i]` depends on `dp[i - 1]` and `dp[i - 2]`, so we should traverse in ascending order from `0` to `n`

(5) Derive the resulted `dp` array and check it by printing
- For example, when `n = 10`, the `dp` should be `[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55].

# Common questions involved dynamic programming
### 0/1 Knapsack problem
Description: \
Given a set of items, each with a weight and a value, determine which items to include in the collection so that the total weight is less than or equal to a given limit and the total value is as large as possible. **Each item can only be added once.**

E.g. 
``` 
bag_size = 4
weight = [1, 3, 4]
value = [15, 20, 30]
```
![](20230327140542.png)
Steps to develop a dynamic-programing algorithm: 
<ol>
<li> <code>dp[i][j]</code> = maximum value we can achieved after considering the inclusion of items from <code>0 to i</code> when the weight limit of the knapsack = <code>j</code>
</li>

<li> There are two ways to get <code>dp[i][j]</code>: 
    <ol>
        <li> without including item <code>i</code> = <code>dp[i - 1][j]</code> 
        </li>
        <li> including item <code>i</code> = 
        <code> dp[i - 1][j - weight[i]] + value[i]</code> 
        </li>
    </ol>

Since  <code>dp[i][j]</code> indicates maximum value, so the recurrence formula is:
<code>dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i])</code>
</li>
<li>The <code>dp</code> array must equal to zero when <code>j = 0</code> (since the knapsack can't include anything). For the first row, the <code>dp</code> array must equal to zero when <code>j < weight[0]</code> (as the knapsack is too small) and equal to <code>value[0]</code> when <code>j >= weight[0]</code> (as the knapsack is now enough to include the item 0)
</li>
<li>The order of traversing the <code>dp</code> array does not matter as long as we traverse it only after the top-left corner is ready.
</li>
</ol>

Advanced:
use 1-D array to implement dp algorithm for Knapsack problem
```PYTHON
def dp_with_1d_array_knapsack():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bag_weight = 4
    
    # initialisation
    dp = [0] * (bag_weight + 1)

    for i in range(len(weight)):
        # must traverse the last bag weight first
        # so that the top left corner will not be overwrote
        for j in range(bag_weight, weight[i] - 1, -1):
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

    print(dp)
```
### complete knapsack
Description: \
Given a set of items, each with a weight and a value, determine which items to include in the collection so that the total weight is less than or equal to a given limit and the total value is as large as possible. **Each item can be added more than once.**

Algorithm for complete knapsack is highly similar to algorithm for 1/0 knapsack except the traverse order. Since each item can be added more than once for complete knapsack, we must traverse from `bag_weight = 0` instead of `bag_weight = bag limit`,so that we can take account of the situation where the item have been added.

```PYTHON
# update item_0 from bag_limit 
# to bag_weight = 3 (right to left) 
        0   1   2   3    4   
0       0   0   0   15   15  

# update item_0 from the first bag_weight which can include item_0 
# to bag_weight = 3 (left to right)
        0   1    2    3    4   
0       0   15   30   45   0  
```

```PYTHON
    # initialisation
    dp = [0] * (bag_weight + 1)

    for i in range(len(weight)):
        # must traverse from the first bag_weight which can include item_i
        for j in range(weight[i], bag_weight + 1):
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
```

# Reference
[Introduction to Algorithms by Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, and Clifford Stein](https://en.wikipedia.org/wiki/Introduction_to_Algorithms)\
[动态规划：01背包理论基础](https://programmercarl.com/背包理论基础01背包-1.html)\
[动态规划：01背包理论基础（滚动数组](https://programmercarl.com/背包理论基础01背包-2.html#一维dp数组-滚动数组)\