# 513. Find Bottom Left Tree Value
### way 1: bfs
```PYTHON
from collections import deque
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        queue = deque([root])
        while queue:
            size = len(queue)
            while size > 0 :
                node = queue.popleft()

                if node.right:
                    queue.append(node.right)
                if node.left:
                    queue.append(node.left)
                
                if len(queue) == 1:
                    node = queue.popleft()
                    if not node.left and not node.right:
                        return node.val
                    else:
                        queue.append(node)
                elif len(queue) == 0:
                    return node.val

                size -= 1
```
### way 2: bfs with extra variable for storing the leftmost value 
```PYTHON
from collections import deque
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        queue = deque([root])
        while queue:
            size = len(queue)
            result = root.val
            while size > 0 :
                node = queue.popleft()

                result = node.val
                if node.right:
                    queue.append(node.right)
                if node.left:
                    queue.append(node.left)

                size -= 1

        return result
```
### way 3: recursive approach 
Use `result` for storing the leftmost value and `max_depth` for storing the depth of the value, update these two variables if encounter a leaf node which has larger depth.
```PYTHON
def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
    def recur(curr, depth):
        nonlocal max_depth
        nonlocal result 
        if not (curr.left or curr.right) and depth > max_depth:
            max_depth = depth
            result = curr.val
            return 
        
        if curr.left:
            depth += 1
            recur(curr.left, depth)
            depth -= 1
        
        if curr.right:
            depth += 1
            recur(curr.right, depth)
            depth -= 1

    max_depth = -float("inf")
    result = root.val
    recur(root, 0)
    return result
```

# 112. Path Sum
For this question, we only need one path where sum equal to `targetSum` so we can terminate the backtracking early by return `True`.
![](./images/20230219184640.png)  
```PYTHON
def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
    def recur(curr, target):
        if not curr.left and not curr.right and target == 0:
            return True
        
        if curr.left:
            target -= curr.left.val
            if recur(curr.left, target):
                # must return True for early termination, 
                # otherwise it will continue backtracking
                return True
            target += curr.left.val
        
        if curr.right:
            target -= curr.right.val
            if recur(curr.right, target):
                # must return True for early termination, 
                # otherwise it will continue backtracking
                return True
            target += curr.right.val

        return False
    
    if not root:
        return False
    
    return recur(root, targetSum - root.val)
```
# 113. Path Sum II
Unlike [Leetcode 112. Path Sum](./Day16_binary_tree.md/#112-path-sum), we need all the paths where sum equal to `targetSum`. Hence, we don't need any return value for the `recur` function as we have to traverse the whole binary tree 
![](./images/20230219184748.png)   
### way 1: backtracking with 'immutable' path
```PYTHON
def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
    def recur(curr, accum, path):
        nonlocal res_list
        nonlocal targetSum

        if not (curr.left or curr.right) and accum == targetSum:
            res_list += [path]
            return

        if curr.left:
            recur(curr.left, accum + curr.left.val, path + [curr.left.val])
        
        if curr.right:
            recur(curr.right, accum + curr.right.val, path + [curr.right.val])

    if not root:
        return []

    res_list = []
    recur(root, root.val, [root.val])
    return res_list
```
### way 2: backtracking with 'mutable' path
```PYTHON
def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
    def recur(curr, accum, path):
        nonlocal res_list
        nonlocal targetSum

        if not (curr.left or curr.right) and accum == targetSum:
            res_list.append([i for i in path])
            return

        if curr.left:
            path.append(curr.left.val)
            accum += curr.left.val
            recur(curr.left, accum, path)
            accum -= curr.left.val
            path.pop()
        
        if curr.right:
            path.append(curr.right.val)
            accum += curr.right.val
            recur(curr.right, accum, path)
            accum -= curr.right.val
            path.pop()
    
    if not root:
        return []

    res_list = []
    recur(root, root.val, [root.val])
    return res_list
```
# 106. Construct Binary Tree from Inorder and Postorder Traversal
![](./images/20230219220759.png)  
```PYTHON
def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
    if not postorder:
        return 

    mid = postorder.pop()

    mid_pointer = 0
    while(mid_pointer < len(inorder) and inorder[mid_pointer] != mid):
        mid_pointer += 1
    
    left_inorder = inorder[0:mid_pointer]
    right_inorder = inorder[mid_pointer + 1: len(inorder)]
    left_postorder = postorder[0:mid_pointer]
    right_postorder = postorder[mid_pointer: len(inorder)]

    leftNode = self.buildTree(left_inorder, left_postorder)
    rightNode = self.buildTree(right_inorder, right_postorder)

    return TreeNode(val = mid, left = leftNode, right = rightNode)
```
# 105. Construct Binary Tree from Preorder and Inorder Traversal
![](./images/20230219220920.png)  
```PYTHON
def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
    if not preorder:
        return

    mid = preorder[0]
    pointer = 0
    while(pointer < len(inorder) and inorder[pointer] != mid):
        pointer += 1

    left_in = inorder[0:pointer]
    right_in = inorder[pointer+1:]
    left_pre = preorder[1:pointer+1]
    right_pre = preorder[pointer+1:]

    return TreeNode(val = mid, left = self.buildTree(left_pre,left_in), right = self.buildTree(right_pre, right_in))
    

```
#### TODO: q112 way3 - iteractive approach with stack
#### TODO: the performance of q106 is too worse, try improve it
#### TODO: the performance of q105 is too worse, try improve it