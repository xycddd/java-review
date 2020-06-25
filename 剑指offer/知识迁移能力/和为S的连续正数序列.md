#### 题目描述
小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!
#### 输出描述:
输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序
#### 算法解析
用两个数small和big分别表示序列的最小值和最大值，初始化small为1，big为2。如果从small到big的序列和大于s，去掉较小的值，如果小于s，增加big。序列至少要
两个数字，一直增加small到(1+sum)/2为止
```
public static ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> res=new ArrayList<>();
        if(sum<3) return res;

        int small=1,big=2;
        int mid=(1+sum)/2;
        int curSum=small+big;


        while(small<mid){
            if(curSum == sum){
                ArrayList<Integer> temp=new ArrayList<>();
                for(int i=small;i<=big;i++)
                    temp.add(i);
                res.add(temp);
                temp.clear();
            }
            while(curSum > sum && small <mid){
                curSum-=small;
                small++;
                if(curSum ==sum){
                    ArrayList<Integer> temp=new ArrayList<>();
                    for(int i=small;i<=big;i++)
                        temp.add(i);
                    res.add(temp);
                    temp.clear();
                }
            }
            big++;
            curSum+=big;
        }
        return res;
    }
```
中文版
```
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>> res;
        int mid=(1+target)/2;
        int small=1,big=2;
        int sum=small+big;
        vector<int> ans{1,2};
        while(small<mid){
            if(sum<target){
                big++;
                ans.push_back(big);
                sum+=big;
            }
            else if(sum == target){
                res.push_back(ans);
                ans.erase(ans.begin());
                sum-=small;
                small++;
            }else{
                sum-=small;
                small++;
                ans.erase(ans.begin());
            }
        }
        return res;

    }
};
```
