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

>2017-06-23

#### 13. Roman To integer
>Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.

`Thought Process:`  
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
>The count-and-say sequence is the sequence of integers with the first five terms as following:  
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

`Thought Process:`  
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
>Write a function that takes a string as input and returns the string reversed.  
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

>2017-06-24

#### 20. Valid Parentheses
>Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

`Thought Process:`  
First, we need to create a stack to store the open parenthesis we encounter. The the next char is the close parenthesis of the the one we stored, then remove the pair from the stack, if not, we push it to the top of the stack. At last, we check if the stack is empty.

```cpp
class Solution {
public:
    bool isValid(string s) {
        map <char, char> ref = {
            {'(', ')'}, {'{', '}'}, {'[', ']'}
        };
        stack<char> stack;
        string open = "({[";
        for(int i = 0; i < s.size(); i++){
            char check = s[i];
            if (open.find(check) != std::string::npos){
                stack.push(check);
            }
            else {
                if (!stack.empty() && ref[stack.top()] == check){
                    stack.pop();
                }
                else {
                    return false;
                }
            }
        }
        return stack.empty();
    }
};
```

>2017-06-25

#### 14. Longest Common Prefix
>Write a function to find the longest common prefix string amongst an array of strings.

`Thought Process`
1. Find prefix of first two strings.
2. Use this prefix to compare and update with the rest of the strings.

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int length = strs.size();
        if (length == 0) return "";
        if (length == 1) return strs[0];
        string prefix = findPrefix(strs[0], strs[1]);
        for (int i = 2; i < length; i++){
            prefix = findPrefix(prefix, strs[i]);
            if (prefix == ""){
                return prefix;
            }
        }
        return prefix;
    }

    string findPrefix(string s1, string s2){
        string prefix = "";
        for (int i = 0; i < min(s1.size(), s2.size()); i++){
            if (s1[i] == s2[i]){
                prefix += s1[i];
            }
            else {
                break;
            }
        }
        return prefix;
    }
};

// Shorting and faster, but harder to understand
string longestCommonPrefix(vector<string>& strs) {
        if (strs.size()==0) return "";
        for (int i=0; i<strs[0].length(); i++)
            for (int j=0; j<strs.size(); j++)
                if ( !(i<strs[j].length() && strs[0][i]==strs[j][i]) )
                    return strs[0].substr(0, i);
        return strs[0];
}
```

#### 28. Implement strStr()
>Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

`Thought Process`
Get the substring of "haystack" with the length of "needle" one by one and compare with "needle", if found, return position.

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int h_length = haystack.size();
        int n_length = needle.size();
        std::string haystack_copy = haystack;
        if (n_length == 0) return 0;
        if (h_length == 0 || h_length < n_length) return -1;
        for (int i = 0; i < (h_length - n_length + 1); i++){
            if (needle == haystack_copy.substr(i, n_length)) return i;
        }
        return -1;
    }
};
```

#### 125. Valid Palindrome
>Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.  
For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.  
Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.  
For the purpose of this problem, we define empty string as valid palindrome.

`Thought Process`
1. Get rid of all the spaces and special characters
2. Make all letters in lowercase
3. Reverse the string and see if it is the same as the original string.

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
       int left=0, right=s.size()-1;
        while(left<right) {
            while(left<s.size() && !isalnum(s[left])) left++;
            while(right>=0 && !isalnum(s[right])) right--;
            if(left>=right) return true;
            if(tolower(s[left++])!=tolower(s[right--])) return false;
        }
        return true;
    }
};
```

#### 22. Generate Parentheses
>Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.  
For example, given n = 3, a solution set is:  
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]

`Thought Process`  

When we encounters questions involve find all combinations. Think the question as a tree related questions and try recursive solution first.
For the recursive solution, base case is when the number of left and right parentheses both equal to n. Otherwise as long as number of "(" is less than n, we can add "(" to the string. As long as number of ")" is less than the number of "(", we can push ")" to the string. Do not forget to pop one parenthesis when a combo is complete to get to another route of the tree.
![](img/GenerateParentheses.jpg)

```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> all;
        string combo;
        makeCombo(all, combo, 0, 0, n);
        return all;
    }

    void makeCombo(vector<string> &all, string &combo, int L, int R, int n){
        if (L == n && R == n){
            all.push_back(combo);
            return;
        }
        if (L < n){
            combo.push_back('(');
            makeCombo(all, combo, L+1, R, n);
            combo.pop_back();
        }
        if (R < L){
            combo.push_back(')');
            makeCombo(all, combo, L, R+1, n);
            combo.pop_back();
        }
    }
};
```
