---
title: Tree traversals
date: 2022-02-27 11:21:00 +05:30
---

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

