# 235. Lowest Common Ancestor of a Binary Search Tree
```PYTHON
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
    if not root:
        return 
    
    if p.val < root.val and q.val < root.val:
        return self.lowestCommonAncestor(root.left, p, q)
    
    if p.val > root.val and q.val > root.val:
        return self.lowestCommonAncestor(root.right, p, q)
    
    return root
```

# 701. Insert into a Binary Search Tree
```PYTHON
def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
    
    if not root:
        return TreeNode(val = val)

    if val < root.val:
        root.left = self.insertIntoBST(root.left, val)
    else:
        root.right = self.insertIntoBST(root.right, val)

    return root
```

# 450. Delete Node in a BST
```PYTHON
def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
    if not root:
        return 
    
    if root.val == key:
        # delete leaf node
        if not root.left and not root.right:
            return 

        # delete node with only right subtree
        elif not root.left:
            return root.right
        # delete node with only left subtree
        elif not root.right:
            return root.left
        
        # delete node contains left and right subtrees
        else:   
            temp = root.right
            while temp.left:
                temp = temp.left
            temp.left = root.left
            root = root.right

    if key < root.val:
        root.left = self.deleteNode(root.left, key)
    else:
        root.right = self.deleteNode(root.right, key)

    return root
```