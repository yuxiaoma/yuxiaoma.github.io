---
layout:     post
title:      "LeetCode: Top Interview Questions - String"
subtitle:   "这里是String类型题的锦集"
date:       2017-06-23 12:00:00
author:     "YuXiao"
header-img: "img/post-bg-miui6.jpg"
header-mask: 0.3
catalog:    true
tags:
    - LeetCode
    - Top Interview Questions
    - LeetCode-String
---

#### 13. Roman To integer
Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.

`Thought Process`
First, we need to find the pattern of the Roman Numeral.  
I:1 V:5 X:10 L:50 C:100 D:500 M:1000  
The pattern I found is that if the roman numeral letter at the left is smaller than the one on the right, the left one become negative when adding them up.  
Example:  
IV: 5-1=4 ; VI: 5+1=6
The idea is build a dictionary first then adding up all the roman letters according to the pattern we found above.

```cpp
class Solution {
public:
    int romanToInt(string s) {
        map<char, int> lookup = {
            {'I', 1}, {'V', 5}, {'X', 10}, {'L', 50}, {'C', 100}, {'D', 500}, {'M', 1000}
        };
        int sum = 0, pre = INT_MIN;
        for (int i = s.size()-1; i >= 0; i--){
            int num = lookup[s[i]];
            if (num >= pre) {sum += num;} else { sum -= num;}
            pre = num;
        }
        return sum;
    }
};
```  


#### 38. Count and Say
The count-and-say sequence is the sequence of integers with the first five terms as following:  
1.     1
2.     11
3.     21
4.     1211
5.     111221
1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
Given an integer n, generate the nth term of the count-and-say sequence.  
Note: Each term of the sequence of integers will be represented as a string.  
Example 1:  
Input: 1
Output: "1"
Example 2:  
Input: 4
Output: "1211"  

`Thought Process`
For all the sequence kind of questions, recursive solution would be the best to try first. All we need to do is to set the base case to 1, then "Count and Say" the n-1th term.

```cpp
class Solution {
public:
    string countAndSay(int n) {
        if (n == 1) return "1";
        else {
            string prev = countAndSay(n-1);
            string say = "";
            int count = 1;
            char d = prev[0];
            for (int i=1; i<prev.size(); i++){
                if (prev[i] == d){
                    count++;
                }
                else {
                    say += to_string(count);
                    say += d;
                    count = 1;
                    d = prev[i];
                }
            }
            say += to_string(count);
            say += d;
            return say;
        }
    }
};
```  


#### 344. Reverse String
Write a function that takes a string as input and returns the string reversed.  
Example:
Given s = "hello", return "olleh".

```cpp
class Solution {
public:
    string reverseString(string s) {
        int length = s.size();
        string rev = s;
        for (int i = 0, j = length-1; i < j; i++, j--){
            char temp = rev[i];
            rev[i] = rev[j];
            rev[j] = temp;
        }
        return rev;
    }
};
```
