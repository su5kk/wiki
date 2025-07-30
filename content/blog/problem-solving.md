---
title: Approaching hard problems
date: 2025-07-30
math: true
authors:
  - name: Tim
    link: https://su5kk.github.io/wiki/about/
    image: https://github.com/su5kk.png
tags:
  - Problem solving
  - Software Engineering
  - Algorithms and Data Structures
---
In this article, I want to show a simple framework on how to approach and solve hard problems.
<!--more-->

## History lesson
In primary school, I would often participate in different academic competitions
in mathematics and languages. When I joined the competition for the first time, my
math teacher told me a simple strategy that stuck with me throughout my whole
life: solve easy problems first, then the harder ones.

In a competition environment, this strategy is pretty solid, because it allows
you to score easy points early on and not mentally drain yourself
before approaching hard problems. 

On the contrary, if you were to solve problems in
order, you may encounter a hard problem in the beginning, struggle with it, and
then even the easy problems would be a challenge because you have exhausted
yourself.

However, this evolved into a bad habit of mine: I would skip the hard problems,
but never even try to approach them after solving the easy ones.

In hindsight, there are several reasons why it used to happen:
1. I could not differentiate between hard problems (there are levels to the
   "hardness" factor)
2. I approached the hard problems with the same tactics as the easy ones: just
   try every basic pattern until you get a feeling what could solve the
   problem

## A Case Study: Merge K Sorted Lists

The tipping point for me was when I was able to solve the [Merge K sorted lists](https://leetcode.com/problems/merge-k-sorted-lists/description/) problem by myself. It is marked as a hard problem on Leetcode, I would say it is on the easy-hard spectrum of hard problems.

After reading the problem statement, I quickly realized that we need to at least
know how to merge 2 sorted lists. Then, we need to figure out a way to merge an
arbitrary number lists efficiently. I decided to write that down:

### Subproblem 1: Merge 2 sorted lists
Merging 2 sorted lists is pretty straightforward:
1. Initialize a pointer at the beginning of each list, let's say `p1` and `p2`
2. On each step, compare current values:
    - if `a[p1] < b[p2]`, append `a[p1]` to the final list and move `p1`
    - otherwise, append `b[p2]` and move `p2`

The time complexity of this algorithm is $O(n + m)$, where $n$ and $m$ are the
    lists' lengths.

So, assuming we know how to merge 2 sorted lists fast, how can we extend it to,
for example, 4 lists? 

If we were to merge lists sequentially, we would make 3 merges.
What if we merged lists in pairs, a classic divide and conquer pattern.

Then, in the first round we would
make 2 merges and then 1 merge.
So, the number of merges is the same, however, I had a gut feeling that merging sequentially is not the most efficient solution.

At this point I realized that I do not fully understand what efficient even means in the
context of this problem. Is it the number of merges or is there a different
metric for that?

### Subproblem 2: Defining efficiency

I decided to compare the sequential and DnC solutions head-to-head.
The number of merges performed is the same, so we need to optimize for a
different metric. The only other numerical parameter we have is the length of
each list.

For simplicity, let's assume that `N` is the length of all lists in
the input.
Let's run the sequential solution for `N = 5`, and `K = 4`:
```
First round: 10 ops
Second round: 15 ops
Third round: 20 ops
--------------------
Overall: 45 ops
```

Let's run the DnC solution for the same params:
```
First round: (5 + 5) + (5 + 5) = 20 ops
Second round: (10 + 10) = 20 ops
---------------------------------------
Overall: 40 ops
```

So the number of operations is indeed less for a DnC solution.
We can also derive complexity formulas from our manual calculations:

**Sequential**: 
$$
\sum_{i=2}^{k} (i \cdot n) = n \sum_{i=2}^{k} i = n \left( \frac{k(k+1)}{2} - 1 \right) = O(nk^2)
$$


**Divide and conquer**:
$$
\text{Total Operations} = (\text{Elements Processed Per Level}) \times (\text{Number of Levels}) 
$$
$$
= (n \cdot k) \times (\log_2 k) = O(nk \log k)
$$

### Bringing it all together

After I solved both subproblems, I just needed to compile the solution:
1. write a `merge(a, b)` function that merges 2 sorted lists
2. in the main loop, implement a divide and conquer to create list pairs and
   call `merge()` on each pair.


## The Framework

By now, you already know how to approach hard problems:
1. Find easy subproblems within the hard problem
2. Ensure your foundations are solid enough to solve the subproblems
3. Find a way to connect subproblems to form a complete solution
4. Do not lose context of the original problem

## Final words

The example was an algorithmic problem, but the framework is universal.

Its application in your particular case is left as an exercise for the reader.

