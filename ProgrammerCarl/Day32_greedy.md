# 738. Monotone Increasing Digits

### way 1: loop through string version of `n` from left to right
Need while loop in case of integer like `332`.
```PYTHON
def monotoneIncreasingDigits(self, n: int) -> int:
    def is_monotone(n):
        last_digit = float('inf')
        while n > 0:
            if n % 10 > last_digit:
                return False
            last_digit = n % 10
            n = n // 10
        return True

    if n < 10 : return n

    while not is_monotone(n):
        str_n = str(n)
        for i in range(len(str_n) - 1):
            curr_digit = int(str_n[i])
            next_digit = int (str_n[i+1])

            if curr_digit > next_digit:
                # curr_digit -= 1
                # change digit at this interval [i+1:] into digit 9
                n = int(str_n[:i] +  str(curr_digit - 1) + ("9" * (len(str_n) - i - 1) ))
                break
        
    return n
```
### way 2: loop through string version of `n` from right to left
```PYTHON
def monotoneIncreasingDigits(self, n: int) -> int:
    if n < 10 : return n

    result = list(str(n))
    for i in range(len(result) - 1, 0, -1):
        curr = int(result[i])
        prev = int(result[i-1])

        if prev > curr:
            # prev -= 1
            # change digit at this interval [i:] into digit 9
            result[i-1] = str(prev - 1)
            result[i:] = "9" * (len(result) - i)
            
    return int("".join(result))
```

# 968. Binary Tree Cameras

### way 1: postorder dfs + greedy algorithm
Start installing camera from the parents of leaf nodes. For each node, there are three situations thus there are three types of returns:\
(1) return 0, if there is no camera on the node and the node is not monitored by any camera\
(2) return 1, if there is a camera installed on the node\
(3) return 2, if the node is monitored by some cameras\
Updates `result` which records  minimum number of cameras needed to monitor the trees and return of current layer's `recur()` according the return from lower layer.


```PYTHON
def minCameraCover(self, root: Optional[TreeNode]) -> int:
    def recur(node):
        nonlocal result
        if not node: return 2

        left = recur(node.left)
        right = recur(node.right)

        if left == 0 or right == 0:
            result += 1
            return 1
        
        if left == 1 or right == 1:
            return 2
        
        return 0

    result = 0
    if recur(root) == 0:
        result += 1
        
    return result
```
