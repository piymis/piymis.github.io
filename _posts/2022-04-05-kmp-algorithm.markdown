---
title: KMP Algorithm
date: 2022-04-05 20:50:00 +05:30
---

```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        m = len(haystack)
        n = len(needle)
        if not needle:
            return 0
        
        if not haystack or m < n:
            return -1

        def helper(pattern):
            res = [0] * len(pattern)
            
            i = 1
            j = 0
            
            while i < len(pattern):
                if pattern[i] == pattern[j]:
                    res[i] = j + 1
                    i += 1
                    j += 1
                elif j > 0:
                    j = res[j-1]
                else:
                    i += 1
                    
            return res
        
        
        suffix_arr = helper(needle)
        
        i = j = 0
        
        while i < m and j < n:
            if haystack[i] == needle[j]:
                i += 1
                j += 1
            elif j > 0:
                j = suffix_arr[j-1]
            else:
                i += 1
        
        
        return i - j if j == n else -1
 
```
    