---
title: Boyer Moore Majority Algorithm
date: 2022-04-05 21:27:00 +05:30
---

```
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = None
        
        for num in nums:
            if count == 0:
                candidate = num
            
            count += 1 if num == candidate else -1
        
        return candidate
```