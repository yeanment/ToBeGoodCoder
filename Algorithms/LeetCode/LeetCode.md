# Solve for Leetcode 

## Intro

This is used to mark the most fundamental problems in Leetcode.

## Math
### Greatest common factor/divisor && leatest common multiplier
gcd(x, y) = mod(y, x) == 0 ? x : gcd(x, mod(y, x)) =  2<sup>min(m0, n0)</sup> \* 3<sup>min(m1, n1)</sup> \* 5<sup>min(m2, n2)</sup> \* ...

lcm(x,y) =  x * y / gcd(x, y) = 2<sup>max(m0, n0)</sup> \* 3<sup>max(m1, n1)</sup> \* 5<sup>max(m2, n2)</sup> \* ...


- [1071.](https://leetcode.com/problems/greatest-common-divisor-of-strings/) [Greatest Common Divisor of Strings](greatest-common-divisor-of-strings.md)
- [1979.](https://leetcode.com/problems/find-greatest-common-divisor-of-array/) [Find Greatest Common Divisor of Array](find-greatest-common-divisor-of-array.md)


### Prime Numbers
- [204.](https://leetcode.com/problems/count-primes/) [Count Primes](count-primes.md)

### Base Conversion
- [168.](https://leetcode.com/problems/excel-sheet-column-title/) [Excel Sheet Column Title](excel-sheet-column-title.md)
- [405.](https://leetcode.com/problems/convert-a-number-to-hexadecimal/) [Convert a Number to Hexadecimal](convert-a-number-to-hexadecimal.md)
- [504.](https://leetcode.com/problems/base-7/) [Base 7](base-7.md)

### Factorial
- [172.](https://leetcode.com/problems/factorial-trailing-zeroes/) [Factorial Trailing Zeroes](factorial-trailing-zeroes.md)

### Add
- [67.](https://leetcode.com/problems/add-binary/) [Add Binary](add-binary.md)
- [415.](https://leetcode.com/problems/add-strings/) [Add Strings](add-strings.md)

### Others
- [169.](https://leetcode.com/problems/majority-element/) [Majority Element](majority-element.md)
- [238.](https://leetcode.com/problems/product-of-array-except-self/) [Product of Array Except Self](product-of-array-except-self.md)
- [326.](https://leetcode.com/problems/power-of-three/) [Power of Three](power-of-three.md)
- [367.](https://leetcode.com/problems/valid-perfect-square/) [Valid Perfect Square](valid-perfect-square.md)
- [462.](https://leetcode.com/problems/minimum-moves-to-equal-array-elements-ii/) [Minimum Moves to Equal Array Elements II](minimum-moves-to-equal-array-elements-ii.md)
- [628.](https://leetcode.com/problems/maximum-product-of-three-numbers/) [Maximum Product of Three Numbers](maximum-product-of-three-numbers.md)

## Sorting
### Merge Sorting
- [88.](https://leetcode.com/problems/merge-sorted-array/) [Merge Sorted Array](merge-sorted-array.md)

### Two Pointers
- [167.](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) [Two Sum II - Input array is sorted](two-sum-ii-input-array-is-sorted.md)
- [16.](https://leetcode.com/problems/3sum-closest/) [3Sum Closest](3sum-closest.md)
- [633.](https://leetcode.com/problems/sum-of-square-numbers/) [Sum of Square Numbers](sum-of-square-numbers.md)
- [345.](https://leetcode.com/problems/reverse-vowels-of-a-string/) [Reverse Vowels of a String](reverse-vowels-of-a-string.md)
- [680.](https://leetcode.com/problems/valid-palindrome-ii/) [Valid Palindrome II](valid-palindrome-ii.md)
- [524.](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/) [Longest Word in Dictionary through Deleting](longest-word-in-dictionary-through-deleting.md)
- [141.](https://leetcode.com/problems/linked-list-cycle/) [Linked List Cycle](linked-list-cycle.md)

## Graph
### Bipartite Graph 
- [785.](https://leetcode.com/problems/is-graph-bipartite/) [Is Graph Bipartite?](is-graph-bipartite.md)

### Topological Sort
- [207.](https://leetcode.com/problems/course-schedule/) [Course Schedule](course-schedule.md)
- [210.](https://leetcode.com/problems/course-schedule-ii/)  [Course Schedule II](course-schedule-ii.md)

### Disjoint Set
- [684.](https://leetcode.com/problems/redundant-connection/) [Redundant Connection](redundant-connection.md)

## Binary Tree
### Tree Traversal
- [102.](https://leetcode.com/problems/binary-tree-level-order-traversal) [Binary Tree Level Order Traversal](binary-tree-level-order-traversal.md)

## Dynamic Programming
### Knapsack Problems
- [416.](https://leetcode.com/problems/partition-equal-subset-sum) [Partition Equal Subset Sum](partition-equal-subset-sum.md)
- [698.](https://leetcode.com/problems/partition-to-k-equal-sum-subsets) [Partition to k Equal Sum Subsets](partition-to-k-equal-sum-subsets.md)
- [518.](https://leetcode.com/problems/coin-change-2) [Coin Charge 2](coin-change-2.md)
- [1262.](https://leetcode.com/problems/greatest-sum-divisible-by-three/) [Greatest Sum Divisible by Three](greatest-sum-divisible-by-three.md)

## Divide and Conquer
- [95.](https://leetcode.com/problems/unique-binary-search-trees-ii/) [Unique Binary Search Trees II](unique-binary-search-trees-ii.md)
- [241.](https://leetcode.com/problems/different-ways-to-add-parentheses/) [Different Ways to Add Parentheses](different-ways-to-add-parentheses.md)


<!-- ## 算法思想
- [排序](Leetcode%20题解%20-%20排序.md)
- [贪心思想](Leetcode%20题解%20-%20贪心思想.md)
- [二分查找](Leetcode%20题解%20-%20二分查找.md)
- [分治](Leetcode%20题解%20-%20分治.md)
- [搜索](Leetcode%20题解%20-%20搜索.md)
- [动态规划](Leetcode%20题解%20-%20动态规划.md)

## 数据结构相关

- [链表](Leetcode%20题解%20-%20链表.md)
- [树](Leetcode%20题解%20-%20树.md)
- [栈和队列](Leetcode%20题解%20-%20栈和队列.md)
- [哈希表](Leetcode%20题解%20-%20哈希表.md)
- [字符串](Leetcode%20题解%20-%20字符串.md)
- [数组与矩阵](Leetcode%20题解%20-%20数组与矩阵.md)
- [图](Leetcode%20题解%20-%20图.md)
- [位运算](Leetcode%20题解%20-%20位运算.md) -->

## Reference
- [Leetcode](https://leetcode.com)
