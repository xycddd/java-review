#### 题目描述
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,
#### 算法解析
使用一个大顶堆存储数据，当大顶堆中的数量超过k之后，就将最大的数字移出，保证大顶堆中的数字数量为k个，当数组遍历完成之后，堆中剩下的元素就是最小的
k个数字，还可以使用基于Partition的算法，参考leetcode 215. Kth Largest Element in an Array.md
```
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> list=new ArrayList<>();
        if(input == null || k>input.length)
            return list;
        PriorityQueue<Integer> priorityQueue=new PriorityQueue<Integer>((a,b)->{
            return b-a;
        });
        
        for(int num:input){
            priorityQueue.offer(num);
            if(priorityQueue.size()>k){
                priorityQueue.poll();
            }
        }
        while(priorityQueue.size() >0){
            list.add(0,priorityQueue.poll());
        }
        return list;
    }
}
```
