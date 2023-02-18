# 559. Maximum Depth of N-ary Tree
The maximum depth of a n-nary tree is equal to the height of the root node. (see more details on [104. Maximum Depth of Binary Tree](./Day13_bfs_binary_tree.md/#104-maximum-depth-of-binary-tree))
```PYTHON
def maxDepth(self, root: 'Node') -> int:
    def get_height(curr):
        if not curr:
            return 0
        
        max_depth = 0
        for child in curr.children:
            child_height = get_height(child) 
            if child_height > max_depth:
                max_depth = child_height
        
        return 1 + max_depth
    
    return get_height(root)
```