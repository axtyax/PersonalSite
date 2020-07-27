---
layout: post
title:  "Coding Problem: Median of two sorted arrays (C++)"
date:   2020-07-23 17:50:41 -0400
categories: Algorithms
---


The following problem is taken from [Leetcode](https://leetcode.com/problems/median-of-two-sorted-arrays). The problem deals with taking two independently sorted arrays of values (with potentially different sizes), and returning the median of the combined set of values in the two arrays. Furthermore, the problem asks for an implementation in **O(log(n+m))** time, where *n* and *m* are the sizes of the two arrays.

## The definition of median

The median of a set of numbers is a value in that set such that exactly half of the remaining numbers are greater than that value, and the other half are less. In the case of a set with an even number of values, we take the "middle" two values and average them to get the median.

## Partitioning the arrays

In finding a median, we will effectively be splitting the entire set of numbers into two partitions, one that is entirely less than the median and one that is greater. We will use two values, *i* and *j* to denote positions in which to partition the first and second array, respectively.

**Example:** 
<div class="center" style="font-size: 22px">
i = 3 <br/>
j = 4 <br/>
[ <span class="color_red">1 2 4</span> | <span class="color_blue">5 10 15 25</span> ] <br/>
[ <span class="color_red">2 5 6 8</span> | <span class="color_blue">9 10 12 16 22 34</span> ] <br>
</div>

## Determining if a given partition represents the median

Remember that the median divides the set equally (or almost equally) into two parts. In other words, any valid candidate for the median partition will be represented by values for *i* and *j*, such that the sum of the sizes of the two left partitions is either the same size as the right partitions, or exactly one greater.

Using the same arrayas as before, here would be a valid candidate:

<div class="center" style="font-size: 22px">
i = 5 <br/>
j = 4 <br/>
[ <span class="color_red">1 2 4 5 10</span> | <span class="color_blue">15 25</span> ] <br/>
[ <span class="color_red">2 5 6 8</span> | <span class="color_blue">9 10 12 16 22 34</span> ] <br>
</div>

As you can see, there are 9 elements in the left partition and 8 in the right partition. We can express this condition mathematically, as $$(2i + 2j) = n + 1$$.
Dividing both sides, we get

$$i + j = \frac{n+1}{2}$$

where $$n$$ is the total number of elements.
{:.center}

In case, you're wondering if this formula will work for both odd and even numbers of elements, try plugging in values for $$i$$, $$j$$, and $$n$$ in both situations. You'll find that integer division (ie. flooring when there's a decimal in the result), takes care of both cases.

The next step is to define a condition to check if a partition with properly sized sides divids the elements the way the median is supposed to.

Since the elements are already sorted, we only need to check elements located closest to the barriers of the partition. In this case, those elements are <span class="color_red">8</span>, <span class="color_red">10</span>, <span class="color_blue">9</span>, and <span class="color_blue">15</span>.

Remember that we must make sure all elements on the left partition are less than or equal to the elements on the right, for it to represent the median. In other words, we must check the following conditions:

$$max(arr\_i\_left) \leq min(arr\_j\_right)$$

$$max(arr\_j\_left) \leq min(arr\_i\_right)$$

To put all of this more clearly, a given partition, given by $$i$$ and $$j$$, represents the median of the two arrays iff:

1. $$i + j = \frac{n+1}{2}$$ $$$$
2. $$[max(arr\_i\_left) \leq min(arr\_j\_right)]$$ && $$[max(arr\_j\_left) \leq min(arr\_i\_right)]$$

## Finding the median

We can use the conditions to check if a random partition represents the median:

1. $$5 + 4 = \frac{17 + 1}{2} = 9 \rightarrow true$$ $$$$
2. $$[10 \leq 9]$$ && $$[8 \leq 15] \rightarrow false$$

Notice that the second condition was false, specifically because the first array's 'max' was greater than the second array's 'min'. This indicates that the values on the left side of the partition are too large, and *i* needs to be shifted left. 

Remember that the arrays are sorted, meaning we can make use of *binary search* to discover the correct partition. In this case, the correct value for *i* is less than what it currently is, so we can reduce *i* to be half way between 0 and its current position (ie. we will divide it by 2), to get $$i = 2$$.

With this new value of *i*, we need to adjust *j* so that the partition still equally divides all the elements. Using the formula we derived earlier:

$$2 + j = \frac{17+1}{2}$$

$$j = 7$$

Giving the new partition:

<div class="center" style="font-size: 22px">
i = 2 <br/>
j = 7 <br/>
[ <span class="color_red">1 2</span> | <span class="color_blue">4 5 10 15 25</span> ] <br/>
[ <span class="color_red">2 5 6 8 9 10 12</span> | <span class="color_blue">16 22 34</span> ] <br>
</div>

Now notice that <span class="color_red">12</span> is greater than <span class="color_blue">4</span>, when it should not be. Now we know *i* is too low, and we can adjust it to be halfway between its current position $$i=2$$ and its previous position of $$i=5$$, which would be $$i=3$$.

<div class="center" style="font-size: 22px">
i = 3 <br/>
j = 6 <br/>
[ <span class="color_red">1 2 4</span> | <span class="color_blue">5 10 15 25</span> ] <br/>
[ <span class="color_red">2 5 6 8 9 10</span> | <span class="color_blue">12 16 22 34</span> ] <br>
</div>

Once again notice that <span class="color_red">10</span> is greater than <span class="color_blue">5</span>, meaning *i* is still too low. Following in the binary search, we can set *i* to be halfway between its current position of $$i=3$$ and $$i=5$$, which would be $$i=4$$.

<div class="center" style="font-size: 22px">
i = 3 <br/>
j = 6 <br/>
[ <span class="color_red">1 2 4 5</span> | <span class="color_blue">10 15 25</span> ] <br/>
[ <span class="color_red">2 5 6 8 9</span> | <span class="color_blue">10 12 16 22 34</span> ] <br>
</div>

Finally, the current parition satisfies both of the conditions stated previously.

1. $$3 + 6 = \frac{17 + 1}{2} = 9 \rightarrow true$$ $$$$
2. $$[5 \leq 10]$$ && $$[9 \leq 10] \rightarrow true$$

Extracting the median value is simple. Since the left partition is one larger than the right partition (because there is an odd number of elements), the median will be the largest number in the left partition. This can only be $$max(arr\_i\_left)$$ or $$max(arr\_j\_left)$$. In this case, those values are <span class="color_red">5</span> and <span class="color_red">9</span>.

Therefor, the median of the two sorted arrays is **9**.

## C++ Code Solution