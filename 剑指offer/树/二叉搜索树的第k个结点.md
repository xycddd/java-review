#### 题目描述
给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。
#### 算法解析
使用一个大顶堆存储k+1个数，每次当数量大于k时弹出最大的数字，最后堆中剩下k个数字，堆顶的数字就是第k小的数字
```
import java.util.*;
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    
    List<TreeNode> list=new ArrayList<>();
    TreeNode KthNode(TreeNode pRoot, int k)
    {
        inOrder(pRoot);
        
        if(k<1 || k>list.size())
            return null;
        PriorityQueue<TreeNode> priorityQueue=new PriorityQueue<>(k+1,(a,b)->{
            return b.val-a.val ;
        });
        for(TreeNode num:list){
            priorityQueue.add(num);
            if(priorityQueue.size()==k+1){
                priorityQueue.poll();
            }
        }
        return priorityQueue.peek();
    }

    void inOrder(TreeNode root){
        if(root != null){
            inOrder(root.left);
            list.add(root);
            inOrder(root.right);
        }
    }

}
```
