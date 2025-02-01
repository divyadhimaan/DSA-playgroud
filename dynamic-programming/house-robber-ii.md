# House Robber - II
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a `circle`. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

Practice [Link](https://leetcode.com/problems/house-robber-ii/description/)


### Recursive

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int robUtil(TreeNode* root)
    {
        if(!root)
            return 0;
        
        int rob = 0, notRob = 0;

        rob += root->val;

        if(root->left)
            rob += robUtil(root->left->left) + robUtil(root->left->right);
        if(root->right)
            rob += robUtil(root->right->left) + robUtil(root->right->right);

        notRob = robUtil(root->left) + robUtil(root->right, memo);

        return max(rob, notRob);
    }

    int rob(TreeNode* root) {
        return robUtil(root);
    }
};
```

> Time Complexity: O(2^n) -> TLE
> 
> Space Complexity: O(n)


### Memoized version

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int robUtil(TreeNode* root, unordered_map<TreeNode*, int> &memo)
    {
        if(!root)
            return 0;

        if(memo.find(root) != memo.end())
            return memo[root];
        
        int rob = 0, notRob = 0;

        rob += root->val;

        if(root->left)
            rob += robUtil(root->left->left, memo) + robUtil(root->left->right, memo);
        if(root->right)
            rob += robUtil(root->right->left, memo) + robUtil(root->right->right, memo);

        notRob = robUtil(root->left, memo) + robUtil(root->right, memo);

        return memo[root] = max(rob, notRob);
    }

    int rob(TreeNode* root) {
        unordered_map<TreeNode*, int> memo;
        return robUtil(root, memo);
    }
};

```

> Time Complexity: O(logn)
> 
> Space Complexity: O(n)

