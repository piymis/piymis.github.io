---
title: Bit Manipulation
date: 2022-02-06 13:03:00 +05:30
categories:
- dsa
---

### Check if the number is even or odd

`num & 1`

### Check if the nth bit from the right (starting from 0th position) is set or not

`num & (1<<n)`

### Set the nth bit from right

`num | (1<<n)`

### Unset the nth bit from right

`num & ~(1<<n)`
