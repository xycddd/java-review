我们正在玩一个猜数游戏，游戏规则如下：<br>
我从 1 到 n 之间选择一个数字，你来猜我选了哪个数字。<br>
每次你猜错了，我都会告诉你，我选的数字比你的大了或者小了。<br>
然而，当你猜了数字 x 并且猜错了的时候，你需要支付金额为 x 的现金。直到你猜到我选的数字，你才算赢得了这个游戏。<br>
** 示例:**<br>

n = 10, 我选择了8.<br>

第一轮: 你猜我选择的数字是5，我会告诉你，我的数字更大一些，然后你需要支付5块。<br>
第二轮: 你猜是7，我告诉你，我的数字更大一些，你支付7块。<br>
第三轮: 你猜是9，我告诉你，我的数字更小一些，你支付9块。<br>

游戏结束。8 就是我选的数字。<br>

你最终要支付 5 + 7 + 9 = 21 块钱。<br>
给定 n ≥ 1，计算你至少需要拥有多少现金才能确保你能赢得这个游戏。<br>

链接：https://leetcode-cn.com/problems/guess-number-higher-or-lower-ii<br>
```
class Solution {
public:
    int getMoneyAmount(int n) {
        if(n==1)
            return 0;
        int dp[n+1][n+1];
        for(int i=0;i<=n;i++){
            for(int j=0;j<=n;j++){
                dp[i][j]=INT_MAX;
            }
        }

        for(int i=0;i<=n;i++){
            dp[i][i]=0;
        }

        for(int j=2;j<=n;j++){
            for(int i=j-1;i>=1;i--){
                for(int k=i+1;k<=j-1;k++){
                    dp[i][j]=min(k+max(dp[i][k-1],dp[k+1][j]),dp[i][j]);
                }
                dp[i][j]=min(dp[i][j],i+dp[i+1][j]);
                dp[i][j]=min(dp[i][j],j+dp[i][j-1]);
            }
        }
        return dp[1][n];
    }
};
```
