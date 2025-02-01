# House Robber - III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called `root`.

Besides the `root`, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.

Given the root of the binary tree, return **the maximum amount of money the thief can rob without alerting the police.**

Practice [Link](https://leetcode.com/problems/house-robber-iii/description/)


### Recursive

```cpp
class Solution {
public:
    int robHouses(vector<int> &nums, int start, int end)
    {
        if(start>end)
            return 0;

        int take = nums[end] + robHouses(nums, start, end-2);
        int notTake = robHouses(nums, start, end-1);
        return max(take, notTake);
    }

    int rob(vector<int>& nums) {
        return max(robHouses(nums, 1, nums.size()-1), robHouses(nums, 0, nums.size()-2));
    }

};
```

> Time Complexity: O(2^n)
> 
> Space Complexity: O(n)


### Memoized version

```cpp
class Solution {
public:
    int robHouses(vector<int> &nums, int start, int end, vector<int> &memo)
    {
        if(start>end)
            return 0;

        if(memo[end]!=-1)
            return memo[end];

        int take = nums[end] + robHouses(nums, start, end-2, memo);
        int notTake = robHouses(nums,start, end-1, memo);
        return memo[end] = max(take, notTake);
    }

    int rob(vector<int>& nums) {
        if(nums.size()==1)
            return nums[0];
        
        vector<int> memo1(nums.size(),-1);
        vector<int> memo2(nums.size(),-1);
        return max(robHouses(nums, 1, nums.size()-1, memo1), robHouses(nums, 0, nums.size()-2, memo2));
    }

};

```

> Time Complexity: O(n)
> 
> Space Complexity: O(n)



### Tabulation version

```cpp
class Solution {
public:
    int robHouses(vector<int>& nums, int start, int end) {
        
        int n = end-start+1;
        if(n==0)
            return 0;
        if(n==1)
            return nums[start];

        vector<int> dp(n, 0);
        dp[0] = nums[start];
        dp[1] = max(nums[start], nums[start+1]);

        for(int idx=2;idx<n;idx++)
        {
            dp[idx] = max(dp[idx-1], nums[start+idx] + dp[idx-2]);
        }

        return dp[n-1];
    }
    int rob(vector<int>& nums) {
        int n= nums.size();
        if(n==0)
            return 0;
        if(n==1)
            return nums[0];
        
        int case1 = robHouses(nums, 0, n-2);
        int case2 = robHouses(nums, 1, n-1);

        return max(case1, case2);
    }
};

```

> Time Complexity: O(n)
> 
> Space Complexity: O(n)

