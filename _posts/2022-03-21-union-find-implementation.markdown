---
title: Union Find implementation
date: 2022-03-21 01:09:00 +05:30
categories:
- dsa
---

```
class UF:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, i):
        if self.parent[i] != i:
            self.parent[i] = self.find(self.parent[i])
            
        return self.parent[i]
    
    def union(self, i, j):
        ir, jr = self.find(i), self.find(j)
        
        if ir == jr:
            return False
        elif self.rank[ir] < self.rank[jr]:
            self.parent[ir] = jr
        elif self.rank[ir] > self.rank[jr]:
            self.parent[jr] = ir
        else:
            self.parent[jr] = ir
            self.rank[ir] += 1
        return True
        
uf = UF(4)
print(uf.parent)
uf.union(1,2)
print(uf.parent)
```