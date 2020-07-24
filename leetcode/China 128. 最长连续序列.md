给定一个未排序的整数数组，找出最长连续序列的长度。<br>

要求算法的时间复杂度为 O(n)。<br>

**示例:**<br>

输入: [100, 4, 200, 1, 3, 2]<br>
输出: 4<br>
解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。<br>

链接：https://leetcode-cn.com/problems/longest-consecutive-sequence<br>


#### 算法解析
首先set集合将过滤重复元素，同时降低查找所需要的时间，我们要找最长的连续序列，每次枚举的时候如果能够找到序列的头，那么就可以在O(n)时间内解决问题。如果确定一个数num是否是连续序列的头<br>
在set集合中查找num-1是否存在，如果存在证明它不是起始元素，跳过，否则使用while循环检查num+1，num+2,...num+n，是否存在与set集合中

```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        set<int> st;
        for(int num:nums){
            st.insert(num);
        }
        int res=0;

        for(int num:nums){
            if(!st.count(num-1)){
                int curLen=1;
                int temp=num+1;
                while(st.count(temp)){
                    curLen++;
                    temp++;
                }
                res=max(res,curLen);
            }
        }
        return res;
    }
    
};
```
