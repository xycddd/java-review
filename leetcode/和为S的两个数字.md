#### 题目描述
输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。
#### 输出描述:
对应每个测试案例，输出两个数，小的先输出。
#### 算法解析
设置两个指针指向头和尾，如果两个数字之和大于sum,尾指针向前移动，小于sum头指针向前移动，等于sum时，记录两个值和乘积大小
```
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        
        ArrayList<Integer> res=new ArrayList<>();
        if(array == null || array.length == 0)
            return res;
        int left=0,right=array.length-1;
        int res1=0,res2=0,minValue=Integer.MAX_VALUE;
        while(left<right){
            if(array[left]+array[right] >sum)
                right--;
            else if(array[left]+array[right]<sum)
                left++;
            else{
                 if(minValue > array[left] * array[right]){
                    res1=array[left];
                    res2=array[right];
                    minValue=array[left]*array[right];
                }
                left++;
            }
        }
        if(res1 != 0 && res2!=0){
            res.add(res1);
            res.add(res2);
        }
        return res;
    }
}
```
