---
layout: post
title:  "Coding Problem: Median of two sorted arrays"
date:   2020-07-23 17:50:41 -0400
categories: Algorithms
---


The following problem is taken from [Leetcode](https://leetcode.com/problems/median-of-two-sorted-arrays). The problem deals with taking two independently sorted arrays of values (with potentially different sizes), and returning the median of the combined set of values in the two arrays. Furthermore, the problem asks for an implementation in **O(log(n+m))** time, where *n* and *m* are the sizes of the two arrays.

## The definition of median

The median of a set of numbers is a value in that set such that exactly half of the remaining numbers are greater than that value, and the other half are less. In the case of a set with an even number of values, we take the "middle" two values and average them to get the median.

## Partitioning the arrays

In finding a median, we will effectively be partitioning the entire set of numbers into two partitions, one that is entirely less than the median and one that is greater. We will use two values, *i* and *j* to denote positions in which to partition the first and second array, respectively.

**Example:**
<div class="center" style="font-size: 22px">
i = 3 <br/>
j = 4 <br/>
[ <span class="color_red">1 2 4</span> | <span class="color_blue">5 10 15 25</span> ] <br/>
[ <span class="color_red">2 5 6 8</span> | <span class="color_blue">9 10 12 16 22 34</span> ] <br>
</div>

## An important property of sorted arrays

To help solve this problem, lets remind ourselves of an important property of sorted arrays. That is, the ability to perform *binary search*.

When 