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

`Thought Process:`
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

`Thought Process:`
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

`Thought Process:`
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

`Thought Process:`
When we encounters questions involve find all combinations. Think the question as a tree related questions and try recursive solution first.
For the recursive solution, base case is when the number of left and right parentheses both equal to n. Otherwise as long as number of "(" is less than n, we can add "(" to the string. As long as number of ")" is less than the number of "(", we can push ")" to the string. Do not forget to pop one parenthesis when a combo is complete to get to another route of the tree.

![](http://img.blog.csdn.net/20150707112601827)

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

>2017-06-27

#### 17. Letter Combinations of a Phone Number
>Given a digit string, return all possible letter combinations that the number could represent.  
A mapping of digit to letters (just like on the telephone buttons) is given below.  
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:
Although the above answer is in lexicographical order, your answer could be in any order you want.

`Thought Process:`
This questions if one of the finding all combinations. Recursion should be tried first for this kind of question.
First we need to create a dictionary for the number pad. For each level of recursion on digits[i], we need to loop through all possible letters according to digits[i] one by one and perform recursion call in side the loop to add different letters to the current combo we working on, then finally append it to our final solution.  

```cpp
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> all;
        if (digits.size() == 0){
            return all;
        }
        map<char, string> dict = {
            {'0', " "}, {'2', "abc"}, {'3', "def"}, {'4', "ghi"}, {'5', "jkl"}, {'6', "mno"}, {'7', "pqrs"}, {'8', "tuv"}, {'9', "wxyz"}
        };
        string one(digits.size(), '\0');
        findCombinations(all, digits, one, 0, dict);
        return all;
    }

    void findCombinations(vector<string> &all, string digits, string &one, int i, map<char, string> dict){
        if (i == digits.size()){
            all.push_back(one);
            return;
        }
        string letters = dict[digits[i]];
        for (int j = 0; j < letters.size(); j++) {
            one[i] = letters[j];
            findCombinations(all, digits, one, i+1, dict);
        }
    }
};
```

>2017-06-28

#### 49. Group Anagrams
>Given an array of strings, group anagrams together.  
For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"],
Return:  
[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]
Note: All inputs will be in lower-case.

`Thought Process：`
For this question we are grouping the strings with the same letter set. First, we need to find a common key to represent the common letter set. The best key will be the set itself, which is just any of the anagrams with the letters sorted.
We store the set to a Map, key: the sorted letters of any anagram in that set. Value: an array of string contain anagram in that set.
This way we only need to scan the input array once O(n), to put all the anagrams into the map and the answer is all the values in map grouped.

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        map<string, vector<string>> hashMap;
        for (int i = 0; i < strs.size(); i++){
            string curr = strs[i];
            string curr_sorted = curr;
            sort(curr_sorted.begin(), curr_sorted.end());
            if (hashMap.find(curr_sorted) == hashMap.end()){
                vector<string> v = {curr};
                hashMap[curr_sorted] = v;
            }
            else {
                hashMap[curr_sorted].push_back(curr);
            }
        }
        vector<vector<string>> result;
        for (std::map<string, vector<string>>::iterator iter = hashMap.begin(); iter != hashMap.end(); ++iter){
            result.push_back(iter->second);
        }
        return result;
    }
};
```

#### 227. Basic Calculator
>Implement a basic calculator to evaluate a simple expression string.  
The expression string contains only non-negative integers, +, -, * , / operators and empty spaces . The integer division should truncate toward zero.  
You may assume that the given expression is always valid.  
Some examples:  
"3+2*2" = 7  
" 3/2 " = 1  
" 3+5 / 2 " = 5  
Note: Do not use the eval built-in library function.

`Thought Process:`
For this question, the order of the operations matters. We need to store numbers with '+', and '-' signs into a stack and deal with them after '* ' and '/'. If the sign is '+', push it onto the stack, if '-', push it onto stack with negative sign. If we have '* ', or '/', we calculate that number with the number on the top of the stack and push the result onto the stack. At last, we get the solution by adding up all the number in the stack.

```cpp
class Solution {
public:
    int calculate(string s) {
        stack<int> nums;
        int d = 0, result = 0;
        char sign = '+';
        for (int i = 0; i < s.size(); i++){
            if (s[i] >= '0'){
                d = d*10 + (s[i]-'0');
            }
            if (s[i] < '0' && s[i] != ' ' || i == s.size()-1){
                if (sign == '+') nums.push(d);
                else if (sign == '-') nums.push(-d);
                else if (sign == '*' || sign == '/'){
                    int temp = sign == '*' ? nums.top() * d : nums.top() / d;
                    nums.pop();
                    nums.push(temp);
                }
                sign = s[i];
                d = 0;
            }
        }
        while (!nums.empty()){
            result += nums.top();
            nums.pop();
        }
        return result;
    }
};
```

#### 5. Longest Palindromic Substring
>Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.  
Example:  
Input: "babad"  
Output: "bab"  
Note: "aba" is also a valid answer.
Example:  
Input: "cbbd"  
Output: "bb"

`Thought Process:`  
<p>This article is for intermediate readers. It introduces the following ideas:
Palindrome, Dynamic Programming and String Manipulation. Make sure you understand what a palindrome means. A palindrome is a string which reads the same in both directions. For example, <script type="math/tex; mode=display">\textrm{''aba''}</script> is a palindome, <script type="math/tex; mode=display">\textrm{''abc''}</script> is not.</p>
<hr />
<p id="approach-1-longest-common-substring-accepted"><strong>Approach #1 (Longest Common Substring) [Accepted]</strong></p>
<p><strong>Common mistake</strong></p>
<p>Some people will be tempted to come up with a quick solution, which is unfortunately flawed (however can be corrected easily):</p>
<blockquote>
<p>Reverse <script type="math/tex; mode=display">S</script> and become <script type="math/tex; mode=display">S'</script>. Find the longest common substring between <script type="math/tex; mode=display">S</script> and <script type="math/tex; mode=display">S'</script>, which must also be the longest palindromic substring.</p>
</blockquote>
<p>This seemed to work, let’s see some examples below.</p>
<p>For example, S = "caba", S' = "abac"</p>
<p>The longest common substring between S and S' is "aba", which is the answer.</p>
<p>Let’s try another example: S = "abacdfgdcaba", S' = "abacdgfdcaba"</p>
<p>The longest common substring between S and S' is "abacd" Clearly, this is not a valid palindrome.</p>
<p><strong>Algorithm</strong></p>
<p>We could see that the longest common substring method fails when there exists a reversed copy of a non-palindromic substring in some other part of <script type="math/tex; mode=display">S</script>. To rectify this, each time we find a longest common substring candidate, we check if the substring’s indices are the same as the reversed substring’s original indices. If it is, then we attempt to update the longest palindrome found so far; if not, we skip this and find the next candidate.</p>
<p>This gives us an <script type="math/tex; mode=display">O(n^2)</script> Dynamic Programming solution which uses <script type="math/tex; mode=display">O(n^2)</script> space (could be improved to use <script type="math/tex; mode=display">O(n)</script> space). Please read more about Longest Common Substring <a href="http://en.wikipedia.org/wiki/Longest_common_substring">here</a>.</p>
<hr />
<p id="approach-2-brute-force-time-limit-exceeded"><strong>Approach #2 (Brute Force) [Time Limit Exceeded]</strong></p>
<p>The obvious brute force solution is to pick all possible starting and ending positions for a substring, and verify if it is a palindrome.</p>
<p><strong>Complexity Analysis</strong></p>
<ul>
<li>
<p>Time complexity : <script type="math/tex; mode=display">O(n^3)</script>.
Assume that <script type="math/tex; mode=display">n</script> is the length of the input string, there are a total of <script type="math/tex; mode=display">\binom{n}{2} = \frac{n(n-1)}{2}</script> such substrings (excluding the trivial solution where a character itself is a palindrome). Since verifying each substring takes <script type="math/tex; mode=display">O(n)</script> time, the run time complexity is <script type="math/tex; mode=display">O(n^3)</script>.</p>
</li>
<li>
<p>Space complexity : <script type="math/tex; mode=display">O(1)</script>.</p>
</li>
</ul>
<hr />
<p id="approach-3-dynamic-programming-accepted"><strong>Approach #3 (Dynamic Programming) [Accepted]</strong></p>
<p>To improve over the brute force solution, we first observe how we can avoid unnecessary re-computation while validating palindromes. Consider the case <script type="math/tex; mode=display">\textrm{''ababa''}</script>. If we already knew that <script type="math/tex; mode=display">\textrm{''bab''}</script> is a palindrome, it is obvious that <script type="math/tex; mode=display">\textrm{''ababa''}</script> must be a palindrome since the two left and right end letters are the same.</p>
<p>We define <script type="math/tex; mode=display">P(i,j)</script> as following:</p>
<p>
<script type="math/tex; mode=display">
P(i,j) =
     \begin{cases}
       \text{true,} &\quad\text{if the substring } S_i \dots S_j \text{ is a palindrome}\\
       \text{false,} &\quad\text{otherwise.} \
     \end{cases}
</script>
</p>
<p>Therefore,</p>
<p>
<script type="math/tex; mode=display">
P(i, j) = ( P(i+1, j-1) \text{ and } S_i == S_j )
</script>
</p>
<p>The base cases are:</p>
<p>
<script type="math/tex; mode=display">
P(i, i) = true
</script>
</p>
<p>
<script type="math/tex; mode=display">
P(i, i+1) = ( S_i == S_{i+1} )
</script>
</p>
<p>This yields a straight forward DP solution, which we first initialize the one and two letters palindromes, and work our way up finding all three letters palindromes, and so on...</p>
<p><strong>Complexity Analysis</strong></p>
<ul>
<li>
<p>Time complexity : <script type="math/tex; mode=display">O(n^2)</script>.
This gives us a runtime complexity of <script type="math/tex; mode=display">O(n^2)</script>.</p>
</li>
<li>
<p>Space complexity : <script type="math/tex; mode=display">O(n^2)</script>.
It uses <script type="math/tex; mode=display">O(n^2)</script> space to store the table.</p>
</li>
</ul>
<p><strong>Additional Exercise</strong></p>
<p>Could you improve the above space complexity further and how?</p>
<hr />
<p id="approach-4-expand-around-center-accepted"><strong>Approach #4 (Expand Around Center) [Accepted]</strong></p>
<p>In fact, we could solve it in <script type="math/tex; mode=display">O(n^2)</script> time using only constant space.</p>
<p>We observe that a palindrome mirrors around its center. Therefore, a palindrome can be expanded from its center, and there are only <script type="math/tex; mode=display">2n - 1</script> such centers.</p>
<p>You might be asking why there are <script type="math/tex; mode=display">2n - 1</script> but not <script type="math/tex; mode=display">n</script> centers? The reason is the center of a palindrome can be in between two letters. Such palindromes have even number of letters (such as <script type="math/tex; mode=display">\textrm{''abba''}</script>) and its center are between the two <script type="math/tex; mode=display">\textrm{'b'}</script>s.</p>
<div class="codehilite"><pre><span></span><span class="kd">public</span> <span class="n">String</span> <span class="nf">longestPalindrome</span><span class="o">(</span><span class="n">String</span> <span class="n">s</span><span class="o">)</span> <span class="o">{</span>
    <span class="kt">int</span> <span class="n">start</span> <span class="o">=</span> <span class="mi">0</span><span class="o">,</span> <span class="n">end</span> <span class="o">=</span> <span class="mi">0</span><span class="o">;</span>
    <span class="k">for</span> <span class="o">(</span><span class="kt">int</span> <span class="n">i</span> <span class="o">=</span> <span class="mi">0</span><span class="o">;</span> <span class="n">i</span> <span class="o">&lt;</span> <span class="n">s</span><span class="o">.</span><span class="na">length</span><span class="o">();</span> <span class="n">i</span><span class="o">++)</span> <span class="o">{</span>
        <span class="kt">int</span> <span class="n">len1</span> <span class="o">=</span> <span class="n">expandAroundCenter</span><span class="o">(</span><span class="n">s</span><span class="o">,</span> <span class="n">i</span><span class="o">,</span> <span class="n">i</span><span class="o">);</span>
        <span class="kt">int</span> <span class="n">len2</span> <span class="o">=</span> <span class="n">expandAroundCenter</span><span class="o">(</span><span class="n">s</span><span class="o">,</span> <span class="n">i</span><span class="o">,</span> <span class="n">i</span> <span class="o">+</span> <span class="mi">1</span><span class="o">);</span>
        <span class="kt">int</span> <span class="n">len</span> <span class="o">=</span> <span class="n">Math</span><span class="o">.</span><span class="na">max</span><span class="o">(</span><span class="n">len1</span><span class="o">,</span> <span class="n">len2</span><span class="o">);</span>
        <span class="k">if</span> <span class="o">(</span><span class="n">len</span> <span class="o">&gt;</span> <span class="n">end</span> <span class="o">-</span> <span class="n">start</span><span class="o">)</span> <span class="o">{</span>
            <span class="n">start</span> <span class="o">=</span> <span class="n">i</span> <span class="o">-</span> <span class="o">(</span><span class="n">len</span> <span class="o">-</span> <span class="mi">1</span><span class="o">)</span> <span class="o">/</span> <span class="mi">2</span><span class="o">;</span>
            <span class="n">end</span> <span class="o">=</span> <span class="n">i</span> <span class="o">+</span> <span class="n">len</span> <span class="o">/</span> <span class="mi">2</span><span class="o">;</span>
        <span class="o">}</span>
    <span class="o">}</span>
    <span class="k">return</span> <span class="n">s</span><span class="o">.</span><span class="na">substring</span><span class="o">(</span><span class="n">start</span><span class="o">,</span> <span class="n">end</span> <span class="o">+</span> <span class="mi">1</span><span class="o">);</span>
<span class="o">}</span>

<span class="kd">private</span> <span class="kt">int</span> <span class="nf">expandAroundCenter</span><span class="o">(</span><span class="n">String</span> <span class="n">s</span><span class="o">,</span> <span class="kt">int</span> <span class="n">left</span><span class="o">,</span> <span class="kt">int</span> <span class="n">right</span><span class="o">)</span> <span class="o">{</span>
    <span class="kt">int</span> <span class="n">L</span> <span class="o">=</span> <span class="n">left</span><span class="o">,</span> <span class="n">R</span> <span class="o">=</span> <span class="n">right</span><span class="o">;</span>
    <span class="k">while</span> <span class="o">(</span><span class="n">L</span> <span class="o">&gt;=</span> <span class="mi">0</span> <span class="o">&amp;&amp;</span> <span class="n">R</span> <span class="o">&lt;</span> <span class="n">s</span><span class="o">.</span><span class="na">length</span><span class="o">()</span> <span class="o">&amp;&amp;</span> <span class="n">s</span><span class="o">.</span><span class="na">charAt</span><span class="o">(</span><span class="n">L</span><span class="o">)</span> <span class="o">==</span> <span class="n">s</span><span class="o">.</span><span class="na">charAt</span><span class="o">(</span><span class="n">R</span><span class="o">))</span> <span class="o">{</span>
        <span class="n">L</span><span class="o">--;</span>
        <span class="n">R</span><span class="o">++;</span>
    <span class="o">}</span>
    <span class="k">return</span> <span class="n">R</span> <span class="o">-</span> <span class="n">L</span> <span class="o">-</span> <span class="mi">1</span><span class="o">;</span>
<span class="o">}</span>
</pre></div>


<p><strong>Complexity Analysis</strong></p>
<ul>
<li>
<p>Time complexity : <script type="math/tex; mode=display">O(n^2)</script>.
Since expanding a palindrome around its center could take <script type="math/tex; mode=display">O(n)</script> time, the overall complexity is <script type="math/tex; mode=display">O(n^2)</script>.</p>
</li>
<li>
<p>Space complexity : <script type="math/tex; mode=display">O(1)</script>.</p>
</li>
</ul>
<p id="approach-5-manachers-algorithm-accepted"><strong>Approach #5 (Manacher's Algorithm) [Accepted]</strong></p>
<p>There is even an <script type="math/tex; mode=display">O(n)</script> algorithm called Manacher's algorithm, explained <a href="http://articles.leetcode.com/longest-palindromic-substring-part-ii/">here in detail</a>. However, it is a non-trivial algorithm, and no one expects you to come up with this algorithm in a 45 minutes coding session. But, please go ahead and understand it, I promise it will be a lot of fun.</p>

<a href="https://leetcode.com/articles/longest-palindromic-substring/">Original post can be find here.</a>

`My Solution:`  
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int dp[s.size()][s.size()] = {0};
        int left = 0, right = 0, len = 0;
        for (int i = 0; i < s.size(); ++i){
            for (int j = 0; j < i; ++j){
                dp[j][i] = (s[i] == s[j] && (i - j < 2 || dp[j + 1][i - 1]));
                if (dp[j][i] && i-j+1 > len){
                    len = i-j+1;
                    left = j;
                    right = i;
                }
            }
            dp[i][i] = 1;
        }
        return s.substr(left, right-left+1);
    }
};
```

#### 3. Longest Substring without Repeating Characters
>Given a string, find the length of the longest substring without repeating characters.  
Examples:  
Given "abcabcbb", the answer is "abc", which the length is 3.  
Given "bbbbb", the answer is "b", with the length of 1.  
Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

`Thought Process:`  
[Multiple great approaches can be find here](https://leetcode.com/articles/longest-substring-without-repeating-characters)

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<char, int> lookup;
        int maxLen = 0, roundBegin = 0;
        for (int i = 0; i < s.size(); i++){
            if (!(lookup.find(s[i]) == lookup.end())){
                roundBegin = max(roundBegin, lookup[s[i]]);
            }
            lookup[s[i]] = i+1;
            maxLen = max(maxLen, i-roundBegin+1);
        }
        return maxLen;
    }
};
```

>2017-07-03

#### 91. Decode Ways
>A message containing letters from A-Z is being encoded to numbers using the following mapping:  
'A' -> 1
'B' -> 2
...
'Z' -> 26
Given an encoded message containing digits, determine the total number of ways to decode it.  
For example,
Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).  
The number of ways decoding "12" is 2.

`Thought Process:`
[Multiple great approaches can be find here](https://leetcode.com/articles/longest-substring-without-repeating-characters)

```cpp
class Solution {
public:
    int numDecodings(string s) {
      if (s.size() == 0) return 0;
      vector<int> dp(s.size()+1);
      //base case
      dp[0] = 1;
      for (int i = 1; i <= s.size(); i++){
        //Inductive steps
        if (s[i-1] > '0'){
          dp[i] += s[i-1];
        }
        if (i > 1 && s.substr(i-2,2) >= "10" && s.substr(i-2, 2) <= "26"){
          f[i] += s[i-2];
        }
      }
      return f[s.size()];
    }
};
```

#### 8. String to Integer (atoi)
>Implement atoi to convert a string to an integer.  
Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.  
Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.  
Update (2015-02-10):
The signature of the C++ function had been updated. If you still see your function signature accepts a const char * argument, please click the reload button  to reset your code definition.  
spoilers alert... click to show requirements for atoi.  
Requirements for atoi:
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.  
The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.  
If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.  
If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.

`Thought Process:`
This question is all about getting the right requirements from the interview and find all the corner cases.

```cpp
class Solution {
public:
    int myAtoi(string str) {
        int length = str.size();
        long long res = 0;
        int p = 0;
        int sign = 1;

        while (str[p] == ' ') p++;
        if (str[p] == '+' || str[p] == '-'){
            if (str[p] == '-') sign = -1;
            p++;
        }
        for (int i = p; i < length; i++){
            if ((str[i] >= '0') && (str[i] <= '9')){
                res = res*10 + (str[i]-'0');
                if (res > INT_MAX) {
                    if (sign == 1) return INT_MAX;
                    if (sign == -1) return INT_MIN;
                }
            }
            else break;
        }
        return res*sign;
    }
};
```
