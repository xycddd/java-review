#### 题目：数组中只有一个数字出现一次，其余数字出现两次，找到出现一次的数字（时间O(n),空间O(1)）
Given a non-empty array of integers, every element appears twice except for one. Find that single one.

**Note:**<br>
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?<br>

**Example 1:**<br>

Input: [2,2,1]<br>
Output: 1<br>
**Example 2:**<br>

Input: [4,1,2,1,2]<br>
Output: 4<br>
#### 算法解析
使用逻辑异或，相同为0，不同为1，将所有书进行异或，相同的数变为0,0和其它数还是它本身
```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res=0;
        for(int i=0;i<nums.size();i++){
            res^=nums[i];
        }
        return res;
    }
};
```
