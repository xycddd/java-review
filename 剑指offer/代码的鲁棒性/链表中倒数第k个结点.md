#### 题目描述
输入一个链表，输出该链表中倒数第k个结点。
#### 算法解析
设置两个指针，快指针先走k步，慢指针再开始走，注意边界情况
```
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        if(head == null || k==0) return null;
        ListNode slow=head,fast=head;
        for(int i=0;i<k-1;i++){
            if(fast.next!=null)
                fast=fast.next;
            else
                return null;
        }
        while(fast.next!=null){
            slow=slow.next;
            fast=fast.next;
        }
        return slow;
    }
}
```
