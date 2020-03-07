---
layout: post
title: Multiplying 2 digit numbers with 11
subtitle: Part of the series on mental arithmetic tricks
tags: [arithmetic, tricks, notes]
comments: false
---


## Pseudocode
```
1. num := number to multiply by 11
2. sum := sum of first and second digits of num
3. if sum < 9:
4.	insert sum between first and second digits of num
5. else:
6.	insert second digit of sum between first and second digits of num
7.	add 1 to first digit of num
```


## Examples

* 42 * 11:
  ```
  1. sum = 4 + 2 = 6
  2. product = 462
  ```

* 99 * 11:
  ```
  1. sum = 9 + 9 = 18
  2. product = 1089
  ```

