在二叉树中，根节点位于深度 0 处，每个深度为 k 的节点的子节点位于深度 k+1 处。<br>

如果二叉树的两个节点深度相同，但父节点不同，则它们是一对堂兄弟节点。<br>

我们给出了具有唯一值的二叉树的根节点 root，以及树中两个不同节点的值 x 和 y。<br>

只有与值 x 和 y 对应的节点是堂兄弟节点时，才返回 true。否则，返回 false。<br>


**示例 1：**<br>

![Alt text](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-01.png)
输入：root = [1,2,3,4], x = 4, y = 3<br>
输出：false<br>
**示例 2：**<br>

![Alt text](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-02.png)
输入：root = [1,2,3,null,4,null,5], x = 5, y = 4<br>
输出：true<br>
**示例 3：**<br>

![Alt text](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/q1248-03.png)
输入：root = [1,2,3,null,4], x = 2, y = 3<br>
输出：false<br>
 

**提示：**<br>

二叉树的节点数介于 2 到 100 之间。<br>
每个节点的值都是唯一的、范围为 1 到 100 的整数。<br>


链接：https://leetcode-cn.com/problems/cousins-in-binary-tree<br>

#### 算法解析
https://leetcode-cn.com/problems/cousins-in-binary-tree/solution/er-cha-shu-de-tang-xiong-di-jie-dian-by-leetcode/<br>
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    Map<Integer,Integer> depth;
    Map<Integer, TreeNode>parent;

    public boolean isCousins(TreeNode root, int x, int y) {
        depth = new HashMap<>();
        parent = new HashMap<>();
        dfs(root,null);
        return (depth.get(x) == depth.get(y) && parent.get(x)!=parent.get(y));
    }

    public void dfs(TreeNode node,TreeNode par){
        if(node!=null){
            int deep=(par == null ? 0 : 1+depth.get(par.val) );
            depth.put(node.val,deep);
            parent.put(node.val,par);
            if(node.left!=null)
                dfs(node.left,node);
            if(node.right!=null)
                dfs(node.right,node);
        }
    }
}
```
