---
layout:     post
title:      "LeetCode: Top Interview Questions - Array"
subtitle:   "这里是Array类型题的锦集"
date:       2017-07-05 12:00:00
author:     "YuXiao"
header-img: "img/post-bg-miui6.jpg"
header-mask: 0.3
catalog:    true
tags:
    - LeetCode
    - Top Interview Questions
    - LeetCode-Array
---

>2017-07-05

#### 15. 3 Sum
>Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.  
Note: The solution set must not contain duplicate triplets.  
For example, given array S = [-1, 0, 1, 2, -1, -4],  
A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

`Thought Process:`  
This question is a modification from the 2 sum question. I can start thinking if we can reduce this question into the 2 sum question. We can try to fix the first number and then use the negative of the first number as a target to find the two numbers that sum up to it. This is exactly like the 2 sum question, since the question requires there is no duplicate in the solution we cannot use the "complement stored in the array approach method", because the key have to be unique. We will try the sliding window method. Sort the array first, fix the first number, two pointers pointing to the front and back of rest of the array, then slide front* and back* according to the comparison between the sum and the target to find the other numbers.

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        int length = nums.size();
        int front;
        int back;
        for (int i = 0; i < length; i++){
            int target = -nums[i];
            // Finding answer which start from number num[i]
            front = i+1;
            back = length-1;
            while(front < back){
                int sum = nums[front] + nums[back];
                if (sum < target){
                    front++;
                }
                else if (sum > target){
                    back--;
                }
                else {
                    vector<int> one = {nums[i], nums[front], nums[back]};
                    res.push_back(one);
                    // Processing duplicates of Number 2
                    // Rolling the front pointer to the next different number forwards
                    while (front < back && nums[front] == one[1]){
                        front++;
                    }
                    // Processing duplicates of Number 3
                    // Rolling the back pointer to the next different number backwards
                    while (front < back && nums[back] == one[2]){
                        back--;
                    }
                }
            }
            // Processing duplicates of Number 1
            while (i < length-1 && nums[i] == nums[i+1]){
                i++;
            }
        }
        return res;
    }
};
```

#### 121. Best Time to Buy and Sell Stock
>Say you have an array for which the ith element is the price of a given stock on day i.  
If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.  
Example 1:
Input: [7, 1, 5, 3, 6, 4]
Output: 5  
max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
Example 2:
Input: [7, 6, 4, 3, 1]
Output: 0  
In this case, no transaction is done, i.e. max profit = 0.

`Thought Process:`  
My first thought for this question is to keep track of the highest and lowest number in the array, and the highest have to occur after the lowest one, but this make this question much more complicated than it should be. Actually we only need to keep track of the lowest number encountered so far, and keep using the current number subtract the lowest number to find the profit, update the maximum profit if needed.

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
    	if (prices.size() < 2) {
    		return 0;
    	}
    	int min = prices[0];
    	int profit = 0;
    	for (int i = 1; i < prices.size(); i++) {
    		if (prices[i] < min) {
    			min = prices[i];
    		}
    		else {
    			if (prices[i]-min > profit){
    				profit = prices[i]-min;
    			}
    		}
    	}
    	return profit;
    }
};
```

#### 122. Best Time to Buy and Sell Stock II
>Say you have an array for which the ith element is the price of a given stock on day i.  
Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

`Thought Process:`  
If we can make multiple transactions, the maximum profit can be found if we make a profit whenever there is a profit, that is do a subtraction whenever the price next day is higher than the previous one, and add the profit into the total profit.

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() < 2){
            return 0;
        }
        int prev = prices[0];
        int profit = 0;
        for (int i = 1; i < prices.size(); i++){
            if (prices[i] > prev){
                profit += prices[i] - prev;
            }
            prev = prices[i];
        }
        return profit;
    }
};
```

>2017-07-09

#### 11. Container with the Most Water
>Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.  
Note: You may not slant the container and n is at least 2.

`Thought Process:`  
The easiest way to do this is to try every combination with a O(n^2) run time. However, we never want to do that.
The better way would be we assume that height[0] and height[n-1] have the most water so far and optimize from that.
Lets assume height[0] < height[n-1], this way we can fix the right edge and go through the left edges to find the best left edge provide the most area, we can fix the right edge because area only depend on the left edge since we already fixed the highest and farthest right edge we can find that higher then left edge, so it does not effect the final area.
Then we do the side thing when we find a left edge that is higher than the right edge we fixed.

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0, r = height.size()-1, ans = 0;
        while (l < r){
            ans = max(ans, (r-l) * min(height[l], height[r]));
            if (height[l] < height[r]){
                l++;
            }
            else{
                r--;
            }
        }
        return ans;
    }
};
```

#### 217. Contians Duplicate
>Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

`Thought Process:`  
This question is very easy, just build a lookup table that store integers that we have already encountered, return true if we already have it in the lookup table.

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        map<int, int> lookup;
        for (int i = 0; i < nums.size(); i++){
            if (lookup.find(nums[i]) != lookup.end()){
                return true;
            }
            lookup[nums[i]] = 0;
        }
        return false;
    }
};
```

#### 162. Find Peak Element
>A peak element is an element that is greater than its neighbors.  
Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.  
The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.  
You may imagine that num[-1] = num[n] = -∞.  
For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.  
click to show spoilers.  
Note:  
Your solution should be in logarithmic complexity.

`Thought Process:`  
Since in the note it mentioned the solution should be in logarithmic complexity, we immediately got a hint that maybe we can solve this problem by bisection method.  
The idea is that we find the mid of the array first, compare it's left and right, if both are smaller then we have found a peak, if left one is larger that means that must be a peak on the left side of mid, since we have the assumption num[-1] = -∞. We can split the array from array and do the same thing again on the left side until we find the peak. We will do the same thing accordingly if the right number is larger.

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) return 0;
        int start=0, end=n-1, mid=0;

        while (start <= end){
            mid = start + (end - start)/2;
            if ((mid == 0 || nums[mid] > nums[mid-1]) && (mid == n-1 || nums[mid] > nums[mid+1])){
                return mid;
            }
            else if (mid > 0 && nums[mid-1] > nums[mid]){
                end = mid-1;
            }
            else {
                start = mid+1;
            }
        }
        return mid;
    }
};
```

#### 287. Find the Duplicate Number
>Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.  
Note:  
You must not modify the array (assume the array is read only).  
You must use only constant, O(1) extra space.  
Your runtime complexity should be less than O(n2).  
There is only one duplicate number in the array, but it could be repeated more than once.

`Thought Process:`  
[Detailed explanation can be find here](https://discuss.leetcode.com/topic/25580/two-solutions-with-explanation-o-nlog-n-and-o-n-time-o-1-space-without-changing-the-input-array)

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int low = 1, high = nums.size() - 1;
        while (low < high) {
            int mid = low + (high - low) * 0.5;
            int cnt = 0;
            for (auto a : nums) {
                if (a <= mid) ++cnt;
            }
            if (cnt <= mid) low = mid + 1;
            else high = mid;
        }
        return low;
    }
};
```

>2017-07-10

#### 380. Insert Delete GetRandom O(1)
```cpp
// Design a data structure that supports all following operations in average O(1) time.
//
// insert(val): Inserts an item val to the set if not already present.
// remove(val): Removes an item val from the set if present.
// getRandom: Returns a random element from current set of elements. Each element must have the same probability of being returned.

class RandomizedSet {
public:
    // Initialize your data structure here.
    RandomizedSet() {}

    // Inserts a value to the set. Returns true if the set did not already contain the specified element.
    bool insert(int val) {
        if (m.count(val)) return false;
        nums.push_back(val);
        m[val] = nums.size() - 1;
        return true;
    }

    // Removes a value from the set. Returns true if the set contained the specified element.
    bool remove(int val) {
        if (!m.count(val)) return false;
        int last = nums.back();
        m[last] = m[val];
        nums[m[val]] = last;
        nums.pop_back();
        m.erase(val);
        return true;
    }

    // Get a random element from the set.
    int getRandom() {
        return nums[rand() % nums.size()];
    }
private:
    vector<int> nums;
    unordered_map<int, int> m;
};

 // Your RandomizedSet object will be instantiated and called as such:
 // RandomizedSet obj = new RandomizedSet();
 // bool param_1 = obj.insert(val);
 // bool param_2 = obj.remove(val);
 // int param_3 = obj.getRandom();

```

>2017-07-11  

#### 53. Maximum Subarray
Qventus Interview Question

>Find the contiguous subarray within an array (containing at least one number) which has the largest sum.  
For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.  
click to show more practice.  
More practice:
If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

`Thought Process:`  
We create a int sum to keep track of the "sum" of adding one more element from the array, if the new "sum" is larger then the maximum sum we already have then update the maximum sum, otherwise keep adding to it. However, if the "sum" go below 0 (negative), it will make the sum smaller for any number comes next, so we rather starting a new round for the next number, that's why we need to reset the "sum" to 0 when it goes negative.

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int sum = 0;
        int max = INT_MIN;
        for (int i = 0; i < nums.size(); i++){
            sum += nums[i];
            if (max < sum){
                max = sum;
            }
            if (sum < 0){
                sum = 0;
            }
        }
        return max;
    }
};
```

>2017-07-12  

#### 55. Jump Game

>Given an array of non-negative integers, you are initially positioned at the first index of the array.  
Each element in the array represents your maximum jump length at that position.  
Determine if you are able to reach the last index.  
For example:
A = [2,3,1,1,4], return true.  
A = [3,2,1,0,4], return false.

`Thought Process:`  
We can think of the array as a tree, every position in the array is node in the tree. If one position is able to jump to another position, there is a edge between the two nodes respect to the positions. We need to do Breath-First-Search to find out the "n-1" node is reachable.  
However, the runtime to do the BFS is equal to the number of edges in the tree, which is O(mn), n refer to the range of number in the array, since it doesn't make sense to have m > n, we can write the runtime as O(n^2).  
This is obviously not the best solution, we find that this tree does not contain cycle and we are doing the BFS in a sequential order 0,1,2...,n-1. We can optimize the solution into the following code.

```cpp
// 初始化标记数组
for (int i = 1; i < n; i++) canJump[i] = false;
canJump[0] = true;
// 宽度优先搜索就是以0..n-1的顺序依次进队的
for (int i = 0; i < n; i++) {
    // 仅判断标记为进队的结点
    if (!canJump[i]) continue;
    // 依次枚举每条连出的边，将目标结点标记为进队（可抵达）
    for (int j = 0; j < nums[i]; j++) {
        canJump[i + j] = true;
    }
}
```
However, this optimization is only on the code side, the runtime is still O(n^2).
After observation we find that any reachable position k is reached by a sequential positions {0,1,2,...,k}, k is the maximum reachable position so far. From that we can create a array of boolean to remember the reachable status, canJump. If the maximum reachable position is k, then canJump[0...k] = true, canJump[k+1...n] = false. So we actually only need to remember the position of k, there is no need to store every position as a boolean in the array.
Following is the optimized code:

```cpp
// 初始化k
int k = 0;
for (int i = 0; i < n; i++) {
    if (i > k) break;
    // 依次枚举每条连出的边，更新k
    for (int j = 0; j < nums[i]; j++) {
        k = max(k, i + j);
    }
}
```

>Since k is the only the maximum, we can optimize this loop

```cpp
for (int j = 0; j < nums[i]; j++) {
    k = max(k, i + j);
}
```
>into this

```cpp
k = max(k, i + nums[i]);
```

There, we have a optimized O(n) runtime and space solution.

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        // 初始化k
        int k = 0, n = nums.size();
        // 本质上是宽度优先搜索
        for (int i = 0; i < n && i <= k; i++) {
            // 利用k表示当前所有队列中的元素（0 ... k）
            k = max(k, i + nums[i]);
        }
        // 根据k判断答案
        return k >= nums.size() - 1;
    }
};
```

#### 169. Majority Element

>Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.  
You may assume that the array is non-empty and the majority element always exist in the array.

`Thought Process:`  
We traverse through the numbers and use a hashmap to count the occurrence of each number. If the occurrence is larger than n/2 then we find the number.

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        map<int, int> lookup;
        for (int i = 0; i < nums.size(); i++){
            if (lookup.find(nums[i]) == lookup.end()){
                lookup[nums[i]] = 1;
            }
            else {
                lookup[nums[i]] += 1;
            }

            if (lookup[nums[i]] > nums.size()/2){
                return nums[i];
            }
        }
        return nums[0];
    }
};
```

>2017-07-13  

#### 152. Maximum Product Subarray

>Find the contiguous subarray within an array (containing at least one number) which has the largest product.  
For example, given the array [2,3,-2,4],
the contiguous subarray [2,3] has the largest product = 6.

`Thought Process:`  
With multiplication, we have the property that -i * j = -ij, and -i * -j = ij, so we have to keep track of the two status of the current best product, the maximum and minimum. Also we need to compare the maximum and minimum with the current integer itself to find the maximum product we have so far.

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int maxProd = nums[0], curr_max = nums[0], curr_min = nums[0];
        for (int i = 1; i < nums.size(); i++){
            int temp = curr_max;
            curr_max = max(nums[i], max(curr_max*nums[i], curr_min*nums[i]));
            curr_min = min(nums[i], min(temp*nums[i], curr_min*nums[i]));
            maxProd = max(maxProd, curr_max);
        }
        return maxProd;
    }
};
```

#### 56. Merge Intervals
>Given a collection of intervals, merge all overlapping intervals.  
For example,
Given [1,3],[2,6],[8,10],[15,18],
return [1,6],[8,10],[15,18].

`Thought Process:`  
We sort the range first then it is very easy to check if the intervals are overlapping. Runtime nlog(n) + n, O(nlog(n)).

```cpp
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Solution {
    static bool compare(Interval a, Interval b){
        return a.start < b.start || (a.start == b.start && a.end < b.end);
    }
public:
    vector<Interval> merge(vector<Interval>& intervals) {
        int n = intervals.size();
        vector<Interval> result;
        if (n == 0) return result;
        sort(intervals.begin(), intervals.end(), compare);

        Interval prev = intervals[0];
        for (int i = 1; i < n; i++){
            if (prev.end >= intervals[i].start){
                prev.end = max(prev.end, intervals[i].end);
            }
            else {
                result.push_back(prev);
                prev = intervals[i];
            }
        }
        result.push_back(prev);
        return result;
    }
};
```

>2017-07-14  

#### 88. Merge Sorted Array
>Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.  
Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

`Thought Process:`  
Since we already know that nums1 have enough space to hold additional elements from nums2. We assume nums1 have m+n space. Most people will try to merge the arrays from the smallest to largest, but that way we will need to shift the array whenever we merge an element. If we merge the arrays from largest to smallest at the position nums1[m+n-1], no shifts needed.

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m-1, j = n-1;
        int position = m+n-1;
        while (j >= 0) {
            if (nums1[i] > nums2[j] && i >= 0){
                nums1[position] = nums1[i];
                i--;
            }
            else {
                nums1[position] = nums2[j];
                j--;
            }
            position--;
        }
    }
};
```

#### 268. Missing Number
>Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.  
For example,
Given nums = [0, 1, 3] return 2.  
Note:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

`Thought Process:`
This question is purely testing your math. Since we know "n" we can calculate the sum of 1+2+...+n = ((1+n)n)/2. Then we can find the missing number by using the sum we got from the equation subtract the sum of all integers in the array.

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        int totalSum = ((1+n) * n)/2;
        for (int i = 0; i < n; i++){
            totalSum -= nums[i];
        }
        return totalSum;
    }
};
```

#### 283. Move Zeros
>Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.  
For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].  
Note:  
You must do this in-place without making a copy of the array.
Minimize the total number of operations.

`Thought Process:`
When we know that we need to solve the problem in-place, we know we need to switch the elements around with-in the array. A good idea is to get a chunk of zeros and keep track of the starting position and end position of the chunk, then just switch the number to the front of the zero-chunk if it is non-zero, and add it to the chunk if it is zero.

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int zeroBegin = 0, zeroEnd = 0;
        if (nums.size() < 2){
            return;
        }
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] != 0){
                int temp = nums[i];
                nums[i] = 0;
                nums[zeroBegin] = temp;
                zeroBegin++;
                zeroEnd = i;
            }
            else {
                zeroEnd++;
            }
        }
    }
};
```

>2017-07-19  

#### 118. Pascal's Triangle
>Given numRows, generate the first numRows of Pascal's triangle.  
For example, given numRows = 5,
Return  
[  
     [1],  
    [1,1],  
   [1,2,1],  
  [1,3,3,1],  
 [1,4,6,4,1]  
]

`Thought Process:`  
This question we can just imitate how we build the triangle using hand. In every level, 1 at the beginning and end, curr[i] = prev[i-1] * prev[i].

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> result;
        if (numRows >= 1) {
            vector<int> one = {1};
            result.push_back(one);
        }
        if (numRows >= 2) {
            vector<int> one = {1,1};
            result.push_back(one);
        }
        if (numRows >= 3) {
            for (int rows = 3; rows <= numRows; rows++){
                vector<int> prev = result[rows-2];
                vector<int> one(rows);
                one[0] = 1;
                for (int i = 1; i < rows-1; i++){
                    one[i] = prev[i-1] + prev[i];
                }
                one[rows-1] = 1;
                result.push_back(one);
            }
        }
        return result;
    }
};
```

#### 66. Plus One
>Given a non-negative integer represented as a non-empty array of digits, plus one to the integer.  
You may assume the integer do not contain any leading zero, except the number 0 itself.  
The digits are stored such that the most significant digit is at the head of the list.

`Thought Process:`  
We can just imitate addition by hand, but pay special attention when the highest digit need carry-over to another higher digit. We need build another array to add in that extra digit.

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        vector<int> dummy(digits), result;
        int n = digits.size()-1;
        dummy[n] += 1;
        int check = dummy[n];
        while (n > 0 && dummy[n] >= 10) {
            dummy[n-1]++;
            dummy[n] %= 10;
            check = dummy[n-1];
            n--;
        }
        if (dummy[0] >= 10){
            dummy[0] %= 10;
            result.push_back(1);
            for (int i = 0; i < dummy.size(); i++){
                result.push_back(dummy[i]);
            }
            return result;
        }
        else {
            return dummy;
        }
    }
};
```

#### 238. Product of Array Except Self
>Given an array of n integers where n > 1, nums, return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].  
Solve it without division and in O(n).  
For example, given [1,2,3,4], return [24,12,8,6].  
Follow up:  
Could you solve it with constant space complexity? (Note: The output array does not count as extra space for the purpose of space complexity analysis.)

`Thought Process:`  
If we want the product of array except self at position i, we can get this product by simply multiply the product of all numbers at left of i (nums[0...i-1]) by all numbers at right of i (nums[i+1...n]).
[Detailed explanation can be find here](http://www.cnblogs.com/grandyang/p/4650187.html)

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> fwd(n, 1), bwd(n, 1), res(n);
        for (int i = 0; i < n - 1; ++i) {
            fwd[i + 1] = fwd[i] * nums[i];
        }
        for (int i = n - 1; i > 0; --i) {
            bwd[i - 1] = bwd[i] * nums[i];
        }
        for (int i = 0; i < n; ++i) {
            res[i] = fwd[i] * bwd[i];
        }
        return res;
    }
};
```

>Space complexity optimized. Use the return array "res" as container of all left product and all right product.

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> res(nums.size(), 1);
        for (int i = 1; i < res.size(); i++){
          res[i] = res[i-1] * nums[i-1];
        }
        int right = 1;
        for (int i = res.size()-1; i >= 0; i--){
          res[i] = res[i] * right;
          right = right * nums[i];
        }
        return res;
    }
};
```

#### 26. Remove Duplicates from Sorted Array
>Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.  
Do not allocate extra space for another array, you must do this in place with constant memory.  
For example,  
Given input array nums = [1,1,2],  
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

`Thought Process:`  
We need a index i to traverse the array and another index j to store the end of non-duplicated array we build so far. We then keep adding distinct number to end of index j and increment j.

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int pre_length = nums.size();
        if (pre_length < 2){
            return pre_length;
        }
        int j = 0;
        for (int i = 1; i < pre_length; i++){
            if (nums[i] != nums[j]){
                j++;
                nums[j] = nums[i];
            }
        }
        return j + 1;
    }
};
```

#### Rotate Array
>Rotate an array of n elements to the right by k steps.  
For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].  
Note:  
Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.  
[show hint]  
Hint:  
Could you do it in-place with O(1) extra space?
Related problem: Reverse Words in a String II

`Thought Process:`  
For this question, assume the original array is AB，to find BA, we need to use the property that BA = （A-1 B-1）-1 （-1 is the reverse of).

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k%n;
        reverse(nums, 0, n-k-1);
        reverse(nums, n-k, n-1);
        reverse(nums, 0, n-1);
    }

    void reverse(vector<int>& nums, int start, int end){
        while (start < end){
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
};
```
