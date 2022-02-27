---
title: Tree traversals
date: 2022-02-27 11:21:00 +05:30
---

### Inorder Iterative

```
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        res = []
        stack = []
        
        while stack or root:
            while root:
                stack.append(root)
                root = root.left
                
            root = stack.pop()
            res.append(root.val)
            root = root.right
        return res
```

### Preorder Iterative

```
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        res = []
        
        stack = [root]
        
        while stack:
            root = stack.pop()
            
            if root:
                res.append(root.val)
                if root.right:
                    stack.append(root.right)
                
                if root.left:
                    stack.append(root.left)
                    
        return res
```

### Postorder Iterative

```

```
