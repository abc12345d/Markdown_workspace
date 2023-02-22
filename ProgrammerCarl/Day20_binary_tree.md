# 669. Trim a Binary Search Tree
```PYTHON
def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
    if not root: 
        return root
    
    if root.val < low:
        return self.trimBST(root.right, low, high)
    
    if root.val > high:
        return self.trimBST(root.left, low, high)

    root.left = self.trimBST(root.left, low, high)
    root.right = self.trimBST(root.right, low, high)

    return root

```
# 108. Convert Sorted Array to Binary Search Tree
```PYTHON
def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
    def recur(left, right, nums):
        if left > right:
            return None
        
        mid = (left + right) // 2

        node = TreeNode(val = nums[mid])
        node.left = recur(left, mid - 1, nums)
        node.right = recur(mid + 1, right, nums)

        return node

    return recur(0, len(nums) - 1, nums)
```
# 538. Convert BST to Greater Tree
```PYTHON
def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
    def recur(curr):
        nonlocal accum
        if not curr:
            return 

        right = recur(curr.right)

        temp = curr.val
        curr.val += accum
        accum += temp

        left = recur(curr.left)

        return curr
    
    accum = 0
    return recur(root)
```