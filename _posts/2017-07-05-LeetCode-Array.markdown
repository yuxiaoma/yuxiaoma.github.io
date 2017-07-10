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
