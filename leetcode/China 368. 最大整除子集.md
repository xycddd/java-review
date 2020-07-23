给出一个由无重复的正整数组成的集合，找出其中最大的整除子集，子集中任意一对 (Si，Sj) 都要满足：Si % Sj = 0 或 Sj % Si = 0。<br>

如果有多个目标子集，返回其中任何一个均可。<br>
 
** 示例 1:**

输入: [1,2,3]<br>
输出: [1,2] (当然, [1,3] 也正确<br>

** 示例 2:**

输入: [1,2,4,8]<br>
输出: [1,2,4,8]<br>
链接：https://leetcode-cn.com/problems/largest-divisible-subset<br>

#### 算法解析
大神的解析，这个记录路径的方式真的6<br>
https://leetcode-cn.com/problems/largest-divisible-subset/solution/ge-ren-ti-jie-dpcon2-by-leolee/
```
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        vector<int> dp(nums.size(),1);
        vector<int> last(nums.size(),-1);
        sort(nums.begin(),nums.end());
        int ans=0,end=-1;

        for(int i=0;i<nums.size();i++){
            for(int j=0;j<i;j++){
                if(nums[i]%nums[j]== 0 && dp[i]<=dp[j]){
                    dp[i]=dp[j]+1;
                    last[i]=j;
                }
            }
            if(dp[i]>ans){
                ans=dp[i];
                end=i;
            }
        }

        vector<int> res;
        for(int i=end;i!=-1;i=last[i]){
            res.push_back(nums[i]);
        }
        return res;
    } 
};
```
