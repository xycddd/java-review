#### 题目描述
把n个筛子仍在地上，所有筛子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率
#### 算法解析
中文版给的解析地址<br>
解法来自https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/solution/nge-tou-zi-de-dian-shu-dong-tai-gui-hua-ji-qi-yo-3/
```
class Solution {
public:
    vector<double> twoSum(int n) {
        vector<vector<int>> dp(n+1,vector<int>(6*n+1));
        for (int i = 1; i <= 6; i ++) {
            dp[1][i] = 1;
        }
        for (int i = 2; i <= n; i ++) {
            for (int j = i; j <= 6*i; j ++) {
                for (int cur = 1; cur <= 6; cur ++) {
                    if (j - cur <= 0) {
                        break;
                    }
                    dp[i][j] += dp[i-1][j-cur];
                }
            }
        }
        int all = pow(6, n);
        vector<double> res;
        for (int i = n; i <= 6 * n; i ++) {
            res.push_back(dp[n][i] * 1.0 / all);
        }
        return res;
    }

};
```

解法一：基于递归的解法<br>
可以先将筛子分成两堆，第一堆只有一个，另一堆有n-1个。单独的那一堆可能出现1-6中的任意一个数字，需要计算1-6的每一种点数和剩下的n-1个骰子来计算点数和
```
public void printProbability(int n){
        if(n<1) return;
        int maxSum=6*n;
        int[] probabilities=new int[6*n-n+1];
        Arrays.fill(probabilities,0);

        Probability(n,probabilities);

        int total=(int)Math.pow(6.0,n);
        for(int i =n;i<=maxSum;i++){
            double ratio=(double)probabilities[i-n]/total;
            System.out.println(i+":"+ratio);
        }
    }

    private void Probability(int n, int[] probabilities) {
        for(int i=1;i<=6;i++){
            Probability(n,n,i,probabilities);
        }
    }

    private void Probability(int original, int current, int sum, int[] probabilities) {
        if(current == 1){
            probabilities[sum-original]++;
        }else{
            for(int i=1;i<=6;i++){
                Probability(original,current-1,i+sum,probabilities);
            }
        }
    }
```
解法二：动态规划法<br>
假设f(n, s) 表示n个骰子点数的和为s的排列情况总数，由于n个筛子的和只与n-1个筛子的和剩下的一个筛子的点数有关，可以推导出如下状态转移方程<br>
f(n,s)=f(n-1,s-1)+f(n-1,s-2)+f(n-1,s-3)+f(n-1,s-4)+f(n-1,s-5)+f(n-1,s-6)
```
```
