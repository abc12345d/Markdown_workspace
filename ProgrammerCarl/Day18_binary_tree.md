# 530. Minimum Absolute Difference in BST
### way 1 : recursive approach (dfs in postorder with min value and max value of the valid range as inputs)
```PYTHON
def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
    def recur(curr, lower, upper, min_diff):
        if not curr: return min_diff

        if (lower == -float("inf") and upper == float("inf")):
            return min(recur(curr.left, -float("inf"), curr.val, min_diff), recur(curr.right, curr.val, float("inf"),min_diff))

        if (lower != -float("inf") and upper != float("inf")):
            diff = min(abs(upper - curr.val), abs(lower - curr.val))
        elif (lower == -float("inf")):
            diff = abs(upper - curr.val)
        else:
            diff = abs(lower - curr.val)

        if diff < min_diff:
            min_diff = diff

        return min(recur(curr.left, min(curr.val, lower), curr.val, min_diff), recur(curr.right, curr.val, max(curr.val, upper), min_diff))

    return recur(root, -float("inf"), float("inf"), float("inf"))
```
### way 2: recursive approach (dfs in inorder with `pre` recording the last visited node)
```PYTHON
def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
    def recur(curr, min_diff):
        nonlocal pre

        if not curr: return min_diff
        
        left = recur(curr.left, min_diff)

        if pre:
            min_diff = min(abs(curr.val - pre.val), min_diff)
        pre = curr

        right = recur(curr.right, min_diff)
        
        return min(left, right)

    pre = None
    return recur(root, float("inf"))
```

# 501. Find Mode in Binary Search Tree
```PYTHON
def findMode(self, root: Optional[TreeNode]) -> List[int]:
    def inorder_recur(curr):
        nonlocal pre, max_count, count, res_list
        if not curr: return 

        # left 
        inorder_recur(curr.left)

        # mid
        if not pre: 
            count = 1
        elif pre.val == curr.val : 
            count += 1
        else:
            count = 1

        pre = curr

        if count > max_count:
            max_count = count
            res_list = [curr.val]
        elif count == max_count:
            res_list.append(curr.val)

        # right
        inorder_recur(curr.right)
        
        return 

    pre = None
    max_count = 0
    count = 0
    res_list = []

    inorder_recur(root)
    return res_list
```
# 236. Lowest Common Ancestor of a Binary Tree
### way 1: recursive approach (dfs in postorder with early termination case)
```PYTHON
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
    # terminating case
    if not root: 
        return

    # early termination case
    if root == p or root == q:
        return root

    # left
    left = self.lowestCommonAncestor(root.left, p, q)
    # right
    right = self.lowestCommonAncestor(root.right, p, q)

    # mid
    if left and right:
        return root
    if not left:
        return right
    if not right:
        return left
```