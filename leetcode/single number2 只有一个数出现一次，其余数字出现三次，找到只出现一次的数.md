#### 题目：只有一个数出现一次，其余数字出现三次，找到只出现一次的数

Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

Input: [2,2,3,2]
Output: 3
**Example 2:**

Input: [0,1,0,1,0,1,99]
Output: 99
#### 算法解析
剑指offer中的思路，如果一个数出现三次，那么它的二进制表示的每一位也出现三次，如果把所有出现三次的数字的二进制表示的每一位都分别加起来，那么每一位的
和都能被3整除。将所有数字的二进制位都加起来，如果某一位的和能被3整除，那么那个值出现一次的数字二进制表示中对应的那一位是0，否则就是1
```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        if(nums.size()==0)
            return 0;
        int cnt[32]={0};
        for(int i=0;i<nums.size();i++){
            for(int j=0;j<32;j++){
                if((nums[i]>>j & 1)==1){
                    cnt[j]++;
                }
            }
        }
        
        int res=0;
        for(int i=0;i<32;i++){
            res+=(cnt[i]%3 << i);
        }
        return res;
    }
};
```
