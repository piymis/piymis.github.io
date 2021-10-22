---
title: Python data-structure notes
date: 2021-10-22 22:00:00 +05:30
---

### Stack
```
stack = [] # Stack created using list in python
stack.pop() # Stack Pop
stack.append() # Stack Push

### Heaps
```
import heapq # Need to import the library
heap = [] # Heap (min-heap) creation from list
heapq.heappush(heap, val) # Push element to heap
heapq.heappop(heap, val) # Pop min values element from heap, as it is a min heap
```