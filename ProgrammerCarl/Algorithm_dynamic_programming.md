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


# Reference
[Introduction to Algorithms by Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, and Clifford Stein](https://en.wikipedia.org/wiki/Introduction_to_Algorithms)