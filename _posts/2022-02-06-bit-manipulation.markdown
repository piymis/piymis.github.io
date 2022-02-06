---
title: Bit Manipulation
date: 2022-02-06 13:03:00 +05:30
categories:
- dsa
---

## Two's complement

```
-num = ~num + 1
~num = -num + 1
```

## Check if the number is even or odd

`num & 1`

## Check if the nth bit from the right (starting from 0th position) is set or not

`num & (1<<n)`

## Set the nth bit from right

`num | (1<<n)`

## Unset the nth bit from right

`num & ~(1<<n)`

## Toggle the nth bit from right

`num ^ (1<<n)`

## Unset rightmost set bit

`num & (num-1)`

## Isolate the rightmost set bit

`num & (-num)`

## Isolate the rightmost 0 bit

`~num & (num+1)

## Set the rightmost 0 bit

`num | (num+1)

### References

* https://emre.me/computer-science/bit-manipulation-tricks/#toggle-the-n-th-bit