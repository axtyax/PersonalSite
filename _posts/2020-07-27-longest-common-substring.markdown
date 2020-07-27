---
layout: post
title:  "Longest Common Substring Algorithm (C++, Dynamic Programming)"
date:   2020-07-27 15:30:41 -0400
categories: Algorithms
---

In algorithmic problems, it's common to be faced with the task of finding the longest common subsequence of elements shared between two arrays. When dealing with strings, this is called the "longest common substring problem", which is often how the task is presented in coding problems.

This algorithm will run in **O(nm)** time, where *n* and *m* are lengths of the first and second string, respectively.

### Example

{% highlight cpp %}

input
    "this is my first string"
    "that is my other sentence"

output
    "is my" (length: 5)


{% endhighlight %}

## Solution with Dynamic Programming

Take the following input, for instance:

    S = "str 1"
    T = "s my string 2"

First, we compare the first letter of the first string with each letter of the second.
Doing this, we notice that the second string has the same letter, *'s'*, at index 0 and 5.

Here we will use an array, *L*, with dimensions *S.size()* and *T.size()* to record these findings.
Store *1* in *L[0][0]* and *L[0][5]* to indicate these locations represent two common substrings of length *1*.

Advancing to the character *t* of the first array, scan the second array for a match. We see the second array has a *t* at index 6.
This time, instead of setting *L[0][6]* to *1*, we will set it to *L[0][6] = L[0][5]+1*, indicating that this position represents a common substring of length *2*.

This process is repeated to the end of the first array. To find the longest common substring, we can either scan the DP table at the end of the algorithm, or store the position of the longest substring so far while the algorithm is running. I will use the latter method in the code example below, because it's more efficient.

{% highlight cpp %}


#include <iostream>
#include <vector>
using namespace std;

string longest_common_substring(string str1, string str2) {

    vector<vector<int>> substrs(str1.size(), vector<int>(str2.size(),0));

    int lcs_length = 0;
    int lcs_end = 0;

    for (int i = 0; i < str1.size(); i++) {
        for (int j = 0; j < str2.size(); j++) {

            if (str1[i] == str2[j]) {
                if (j > 0 && i > 0)
                    substrs[i][j] = substrs[i-1][j-1] + 1;
                else 
                    substrs[i][j] = 1;
            }

            if (substrs[i][j] > lcs_length) {
                lcs_length = substrs[i][j];
                lcs_end = j+1;
            }

        }
    }

    return str2.substr(lcs_end-lcs_length,lcs_length);

}

int main() {

    cout << "LCS of 'this is my first string' and 'that is my other sentence' is: " <<
        longest_common_substring("this is my first string","that is my other sentence") << endl;

    cout << "LCS of 'asagasgasdasgsss' and 'dfsdhsasadsfasda' is: " <<
        longest_common_substring("asagasgasdasgsss","dfsdhsasadsfasda") << endl;

    return 0;
}

//---output---
//  LCS of 'this is my first string' and 'that is my other sentence' is:  is my 
//  LCS of 'asagasgasdasgsss' and 'dfsdhsasadsfasda' is: asda

{% endhighlight %}
