
---
layout: post
title: "Unequal Triplets and Beyond: Finding Distinct Element Groups in Arrays"
date: 2024-09-14 12:00:00
categories: blog
---
# Unequal Triplets and Beyond: Finding Distinct Element Groups in Arrays

based on [#Leetcode question](https://leetcode.com/problems/number-of-unequal-triplets-in-array/description/)
## Problem Statement

You are given a 0-indexed array of positive integers `nums`. The task is to find the number of **triplets** `(i, j, k)` that meet the following conditions:

1. \( 0 \leq i < j < k < \text{nums.length} \)
2. The numbers at the positions \( \text{nums}[i], \text{nums}[j], \text{nums}[k] \) are **pairwise distinct**. This means:
   - \( \text{nums}[i] \neq \text{nums}[j] \)
   - \( \text{nums}[i] \neq \text{nums}[k] \)
   - \( \text{nums}[j] \neq \text{nums}[k] \)

The goal is to return the number of such valid triplets. After solving this, we also explore the possibility of extending this logic to handle larger groups, like quadruplets or even quintuplets.

## Approach 1: Brute Force

A simple way to solve this problem is by iterating over all possible combinations of triplets and checking if the elements are distinct. This brute force approach can be implemented in **O(n³)** time complexity.

### Code:

```python
def unequalTripletsBruteForce(nums):
    n = len(nums)
    count = 0
    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                if nums[i] != nums[j] and nums[i] != nums[k] and nums[j] != nums[k]:
                    count += 1
    return count
```

While this approach works for small arrays, it is inefficient for larger arrays, given its cubic time complexity. This brings us to an optimized solution using **frequency counting**.


## Simple Optmisation: doesn't change complexity much
```
class Solution:
    def unequalTriplets(self, nums: List[int]) -> int:
        n = len(nums)
        cnt = 0
        for i in range(n - 2):
            for j in range(i + 1, n - 1):
                if nums[i] != nums[j]:
                    for k in range(j + 1, n):
                        if nums[j] != nums[k] and nums[k] != nums[i]:
                            cnt += 1

        return cnt
```
## Optimized Approach: Frequency Count and Two-Pointer Logic

We can use the frequency of each unique element in the array to optimize the solution. This drastically reduces the complexity to **O(n)** by avoiding the need to check all possible combinations.

### Key Insight:
Instead of iterating over all triplets, we can:
1. Count the occurrence (frequency) of each element in the array.
2. Partition the array into **three parts**:
   - Elements to the left (already processed),
   - The current element,
   - Elements to the right (yet to be processed).

We compute the number of distinct triplets by calculating how many valid triplets can be formed with one element from each of these three parts.

### Optimized Code for Triplets:

```python
from collections import Counter
from typing import List

class Solution:
    def unequalTriplets(self, nums: List[int]) -> int:
        freq = Counter(nums)

        left = 0
        right = len(nums)
        res = 0
        for _, count in freq.items():
            right -= count
            res += left * count * right
            left += count
        
        return res
```

### Explanation:
1. **Frequency Calculation (`freq = Counter(nums)`)**:
   - We use the `Counter` to calculate the frequency of each element in the array.

2. **Main Loop**:
   - For each element in the frequency table:
     - We subtract the frequency of the current element from `right`, which tracks how many elements are yet to be processed.
     - We calculate the number of valid triplets that can be formed with the current element using `left * count * right`, where:
       - `left` is the number of elements already processed,
       - `count` is the frequency of the current element,
       - `right` is the number of elements remaining.

3. **Final Result**:
   - After looping through all unique elements, the variable `res` holds the total number of valid distinct triplets.

### Time Complexity:
- The time complexity of this solution is **O(n)**, where `n` is the number of elements in the array.

## Extending the Solution to Larger Groups (Quadruplets, Quintuplets, etc.)

What if you wanted to count distinct **quadruplets** or even larger groups? The logic can be extended, but it requires some modifications.

### Generalizing for Larger Groups:
For quadruplets, the idea remains similar:
- Instead of dividing the array into **three parts** (left, current element, right), we now divide it into **four parts**:
  - Elements processed so far (`left1`),
  - Elements processed in the previous iteration (`left2`),
  - The current element (`current`),
  - The remaining elements (`right`).

### Code for Quadruplets:

```python
from collections import Counter
from typing import List

class Solution:
    def unequalQuadruplets(self, nums: List[int]) -> int:
        freq = Counter(nums)

        left1 = 0
        left2 = 0
        right = len(nums)
        res = 0
        for _, count in freq.items():
            right -= count
            res += left1 * left2 * count * right
            left2 += left1 * count
            left1 += count

        return res
```

### Explanation:
1. **`left1` and `left2`** track how many elements have been processed in the first two and third positions of the quadruplet, respectively.
2. **`right`** tracks how many elements remain to be processed.
3. The formula `res += left1 * left2 * count * right` computes the number of valid quadruplets with one element from each partition.

### Generalizing Further:
To extend the logic to groups of size `k`, we would continue adding more "left" partitions and update the result by multiplying the counts of distinct elements from each section.

## Conclusion

This problem showcases the power of optimizing brute force solutions by leveraging frequency counting and partitioning. While brute force works for small inputs, the optimized approach provides a scalable solution. Moreover, the technique can be extended to count larger groups of distinct elements, such as quadruplets, quintuplets, and beyond.

## Key Takeaways:
1. **Frequency counting** is a powerful tool for optimizing problems involving distinct element combinations.
2. The partitioning of arrays into multiple sections allows us to efficiently calculate the number of distinct groups.
3. This solution scales linearly with the input size, making it suitable for large datasets.

By understanding and applying these principles, you can tackle a wide variety of problems involving distinct element groupings in arrays!

Thanks to :
- [Source](https://leetcode.com/problems/number-of-unequal-triplets-in-array/solutions/5538953/easiest-faster-lesser-c-python3-java-c-python-c-explained-beats/)
- Similar questions 
   - Q - [#1460]( https://leetcode.com/problems/make-two-arrays-equal-by-reversing-subarrays/solutions/5538697/easiestfaster-lesser-cpython3javacpythoncexplained-beats)
   
   - Q - [#1534](https://leetcode.com/problems/count-good-triplets/solutions/5538845/easiestfaster-lesser-cpython3javacpythoncexplained-beats)
   
   - Q - [#2367](https://leetcode.com/problems/number-of-arithmetic-triplets/solutions/5539085/easiestfaster-lesser-cpython3javacpythoncexplained-beats)
   
   - Q - [#2908](https://leetcode.com/problems/minimum-sum-of-mountain-triplets-i/solutions/5539207/easiestfaster-lesser-cpython3javacpythoncexplained-beats)



