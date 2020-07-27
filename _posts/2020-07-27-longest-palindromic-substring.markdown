---
layout: post
title:  "Longest Palindromic Substring (C++) - Coding Problem"
date:   2020-07-27 15:30:41 -0400
categories: Algorithms
---

The following coding problem is taken from [Leetcode](https://leetcode.com/problems/longest-palindromic-substring/)

This problem asks us to find the longest substring of a given string that is a palindrome.

A palindrome is a sequence whose elements are "mirrored", as in each index stores an identical element to the one stored at the equivalent position from the opposite side of the center (Eg. "racecar" is a palindrome. So is "cbttbc")

## Solution using expanding windows (C++)

{% highlight cpp %}

#include <iostream>
#include <string>
using namespace std;

string expand_palindrome(int i, int j, string &str) {

    while (i >= 0 && j < str.size() && str.at(i) == str.at(j))
    {
        i--;
        j++;
    }
    i++;

    return str.substr(i, j - i);
}

string longestPalindrome(string s) {

    if (s.size() == 1)
        return string(1, s.at(0));

    string longest_pal = "";
    for (int i = 0; i < (signed)s.size() - 1; i++) {

        if (s.at(i) == s.at(i + 1)) {

            string pal = expand_palindrome(i, i + 1, s);
            if (pal.size() > longest_pal.size()) {
                longest_pal = pal;
            }
        }

        string pal = expand_palindrome(i, i, s);
        if (pal.size() > longest_pal.size()) {
            longest_pal = pal;
        }
    }

    return longest_pal;
}

int main() {

    cout << "longest palindrome of 'cabad' is " << longestPalindrome("cabad") << endl;

    return 0;
}

//---output---
//  aba

{% endhighlight %}