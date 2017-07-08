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
