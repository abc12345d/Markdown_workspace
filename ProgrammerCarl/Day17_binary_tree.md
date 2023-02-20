# 654. Maximum Binary Tree
### way 1:
```PYTHON
def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
    if not nums:
        return 

    index = 0
    max_index = -1
    max_v = -1
    while index < len(nums):
        if nums[index] > max_v:
            max_index = index
            max_v = nums[index] 
        index += 1

    left_node = self.constructMaximumBinaryTree(nums[:max_index])
    right_node = self.constructMaximumBinaryTree(nums[max_index + 1:])
    return TreeNode(val = max_v, left = left_node, right = right_node)
```
### way 2: 
```PYTHON
def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
    def constructWithPointer(nums, start_pointer, end_pointer):
        # range is [start_pointer, end_pointer]
        if start_pointer > end_pointer: 
            return
    
        index = start_pointer
        max_index = -1
        max_v = -1
        while index <= end_pointer:
            if nums[index] > max_v:
                max_index = index
                max_v = nums[index] 
            index += 1

        left_node = constructWithPointer(nums, start_pointer, max_index - 1)
        right_node = constructWithPointer(nums, max_index + 1, end_pointer)
        return TreeNode(val = max_v, left = left_node, right = right_node)

    return constructWithPointer(nums, 0, len(nums) - 1)
```

# 617. Merge Two Binary Trees
### way 1: construct a new merged tree
This requires extra O(n) for constructing the new merged tree compared to way 2.
```PYTHON
def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
    
    if not root1: return root2
    if not root2: return root1
    
    node = TreeNode(val = root1.val + root2.val)
    node.left = self.mergeTrees(root1.left, root2.left)
    node.right = self.mergeTrees(root1.right, root2.right)

    return node
```
### way 2: overwrite the `root1` binary tree
```PYTHON
def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
    
    if not root1: return root2
    if not root2: return root1
    
    root1.val += root2.val
    root1.left = self.mergeTrees(root1.left, root2.left)
    root1.right = self.mergeTrees(root1.right, root2.right)

    return root1
```
# 700. Search in a Binary Search Tree
### way 1: recursive approach
```PYTHON
def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
    if not root:
        return 
        
    if root.val == val:
        return root
    
    if root.val > val:
        return self.searchBST(root.left, val)
    else:
        return self.searchBST(root.right, val)
```
### way 2: iteractive approach
```PYTHON
def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
    curr = root
    while curr:
        if curr.val == val:
            return curr
        elif curr.val > val:
            curr = curr.left
        else:
            curr = curr.right
    
    return
```

# 98. Validate Binary Search Tree
The characteristic of Binary search tree:\
(1) All the nodes in left subtree < current node \
(2) All the nodes in right subtree > current node 

### way 1: serialise binary search tree in inorder, then check if the serialised list is monotonic
```PYTHON
def isValidBST(self, root: Optional[TreeNode]) -> bool:
    def serialiseBT(curr):
        nonlocal binary_tree_list
        if not curr:
            return
        serialiseBT(curr.left)
        binary_tree_list.append(curr.val)
        serialiseBT(curr.right)

    
    binary_tree_list = []
    serialiseBT(root)
    for i in range(1, len(binary_tree_list)):
        if binary_tree_list[i] <= binary_tree_list[i - 1]:
            return False
    
    return True
```
### way 2: recursive approach (dfs in postorder with narrower valid range as input for next layer)
```PYTHON
def isValidBST(self, root: Optional[TreeNode]) -> bool:
    def recur(curr, lower, upper):
        if not curr: return True

        # lower < curr.val < upper
        if curr.val <= lower:
            return False
        
        if curr.val >= upper:
            return False
        
        return recur(curr.left, lower, curr.val) and recur(curr.right, curr.val, upper)

    return recur(root, -float("inf"), float("inf"))
```
