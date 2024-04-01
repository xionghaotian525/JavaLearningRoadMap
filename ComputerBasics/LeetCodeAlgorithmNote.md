# 力扣刷题笔记

## 目录
[toc]

<!-- - [2024/3/27](#2024327)
  - [94.二叉树的中序遍历](#94二叉树的中序遍历)
  - [104.二叉树的最大深度](#104二叉树的最大深度)
  - [226.翻转二叉树](#226翻转二叉树)
  - [101.对称二叉树](#101对称二叉树)
- [2024/3/28](#2024328)
  - [543.二叉树的直径](#543二叉树的直径)
  - [102.二叉树的层序遍历](#102二叉树的层序遍历)
  - [108.将有序数组转化为二叉搜索树](#108将有序数组转化为二叉搜索树)
  - [98.验证二叉搜索树](#98验证二叉搜索树)
- []()
- []() -->

## 2024/3/27

### 94.二叉树的中序遍历

1. **深度优先搜索DFS**

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    // 前序遍历函数，返回节点值列表
    public List<Integer> preorderTraversal(TreeNode root) {
        // 创建结果列表
        List<Integer> res = new ArrayList<>();
        // 调用深度优先搜索函数进行遍历
        dfs(root, res);
        // 返回结果列表
        return res;
    }
    
    // 深度优先搜索函数，将节点值存储到列表中
    void dfs(TreeNode root, List<Integer> res) {
        // 如果当前节点为空，直接返回
        if (root == null) return;
        // 将当前节点的值添加到结果列表中
        res.add(root.val);
        // 递归遍历左子树
        dfs(root.left, res);
        // 递归遍历右子树
        dfs(root.right, res);
    }
}
```
2. **迭代&基于栈**

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
        // 循环直到栈为空且当前节点为null
        while (curr != null || !stack.isEmpty()) {
            // 将当前节点及其所有左子节点入栈
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            // 弹出栈顶节点，并将其值添加到结果列表中
            curr = stack.pop();
            res.add(curr.val);
            // 将当前节点指向其右子节点，继续遍历
            curr = curr.right;
        }
        return res;
    }
}

```
3. **递归**
```java
import java.util.LinkedList;
import java.util.List;

class Solution {
    // 定义一个成员变量用于存储中序遍历结果
    List<Integer> res = new LinkedList<>();
    
    // 中序遍历函数，返回中序遍历结果列表
    public List<Integer> inorderTraversal(TreeNode root) {
        // 递归遍历二叉树
        if (root != null) {
            // 先遍历左子树
            inorderTraversal(root.left);
            // 将当前节点的值添加到结果列表中
            res.add(root.val);
            // 再遍历右子树
            inorderTraversal(root.right);
        }
        // 返回中序遍历结果列表
        return res;
    }
}

```
### 104.二叉树的最大深度

1. **递归**

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int leftH = maxDepth(root.left);
        int rightH = maxDepth(root.right);
 
        return (leftH > rightH ? leftH :rightH) + 1;
    }
}
```
2. **迭代&广度优先搜索BFS**

```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            depth++;
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return depth;
    }
}

```
### 226.翻转二叉树

1. **递归**

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        // 递归地交换左右子树
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        // 交换当前节点的左右子树
        root.left = right;
        root.right = left;
        return root;
    }
}

```

2. **迭代&广度优先搜索BFS**

```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
        return root;
    }
}

```
### 101.对称二叉树

1. **递归**
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isMirror(root, root);
    }
    
    private boolean isMirror(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        }
        if (left == null || right == null) {
            return false;
        }
        return (left.val == right.val)
            && isMirror(left.left, right.right)
            && isMirror(left.right, right.left);
    }
}

```
2. **迭代&广度优先搜索BFS**
```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode left = queue.poll();
            TreeNode right = queue.poll();
            if (left == null && right == null) {
                continue;
            }
            if (left == null || right == null || left.val != right.val) {
                return false;
            }
            queue.offer(left.left);
            queue.offer(right.right);
            queue.offer(left.right);
            queue.offer(right.left);
        }
        return true;
    }
}

```
## 2024/3/28

### 543.二叉树的直径
1. **错误提交**
```java
class Solution {
    public int diameterOfBinaryTree(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int leftH = getHeight(root.left);
        int rightH = getHeight(root.right);
 
        return leftH+rightH;
    }
    public int getHeight(TreeNode root){
        if(root==null){
            return 0;
        }
        int leftH= getHeight(root.left);
        int rightH=getHeight(root.right);
        if(leftH>rightH){
            return leftH + 1;
        }else{
            return rightH + 1;
        }
    }
}
```
未通过测试用例：
```java
root=[4,-7,-3,null,null,-9,-3,9,-7,-4,null,6,null,-6,-6,null,null,0,6,5,null,9,null,null,-1,-4,null,null,null,-2]
```
2. **ChatGPT修正**
>其实就是深度优先搜索**DFS**
```java
class Solution {
    private int maxDiameter = 0; // 记录直径的最大值
    
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
        getHeight(root);
        return maxDiameter;
    }
    
    private int getHeight(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftH = getHeight(root.left);
        int rightH = getHeight(root.right);
        // 更新直径的最大值
        maxDiameter = Math.max(maxDiameter, leftH + rightH);
        // 返回当前节点的高度
        return Math.max(leftH, rightH) + 1;
    }
}
```
3. **广度优先搜索BFS**
> BFS（广度优先搜索）也可以用来解决二叉树直径的问题。BFS遍历树时，我们可以使用一个队列来存储每一层的节点，并在遍历每一层节点时更新直径的最大值。
> 
>具体步骤如下：
> - 初始化一个队列，并将根节点入队。
> - 循环直到队列为空，每次循环遍历当前层的所有节点，并将它们的子节点入队。
> - 在遍历每个节点时，计算该节点的左右子树高度之和，并更新直径的最大值。
> - 返回最大直径。
```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int maxDiameter = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            int leftH = height(node.left);
            int rightH = height(node.right);
            maxDiameter = Math.max(maxDiameter, leftH + rightH);
            
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
        
        return maxDiameter;
    }
    
    private int height(TreeNode node) {
        if (node == null) {
            return 0;
        }
        return Math.max(height(node.left), height(node.right)) + 1;
    }
}

```

### 102.二叉树的层序遍历

1. **错误提交**
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue=new LinkedList<>();
        List<List<Integer>> list=new ArrayList<>();
        if(root!=null){
            queue.offer(root);
        }
        int i=getHeight(root);
        while(i!=0){
            List<Integer> innerList=new ArrayList<>();
            while(!queue.isEmpty()){           
                TreeNode top=queue.poll();
                innerList.add(top.val);
                if(top.left!=null){
                    queue.offer(top.left);
                }
                if(top.right!=null){
                    queue.offer(top.right);
                }
            }
            list.add(innerList);
            i--;
        }
        return list;
    }
    public  int getHeight(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int leftH = getHeight(root.left);
        int rightH = getHeight(root.right);
 
        return (leftH > rightH ? leftH :rightH) + 1;
    }
}
```
2. ChatGPT修改
```java
import java.util.*;

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> list = new ArrayList<>();
        if (root == null) {
            return list;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size(); // 当前层的节点个数
            List<Integer> innerList = new ArrayList<>();
            
            // 遍历当前层的所有节点
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                innerList.add(node.val);

                // 将当前节点的子节点加入队列
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }

            // 将当前层的节点值列表加入结果列表
            list.add(innerList);
        }

        return list;
    }
}

```
3. **递归**
```java
class Solution {
    private List<List<Integer>> result = new ArrayList();
    public List<List<Integer>> levelOrder(TreeNode root) {
        helper(1,root);
        return result;
    }

    private void helper(int height,TreeNode root){
        if(root == null){
            return;
        }
        //将当前结点添加进去
        if(height > result.size()){
            result.add(new ArrayList());
        }
        List<Integer> eachHeight = result.get(height-1);
        eachHeight.add(root.val);
        //子树
        helper(height+1,root.left);
        helper(height+1,root.right);
    }
}
```

### 108.将有序数组转化为二叉搜索树

1. **递归**

左右等分建立左右子树，中间节点作为子树根节点，递归该过程

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return nums == null ? null : buildTree(nums, 0, nums.length - 1);
    }

    private TreeNode buildTree(int[] nums, int l, int r) {
        if (l > r) {
            return null;
        }
        int m = l + (r - l) / 2;
        TreeNode root = new TreeNode(nums[m]);
        root.left = buildTree(nums, l, m - 1);
        root.right = buildTree(nums, m + 1, r);
        return root;
    }
}
```

### 98.验证二叉搜索树

1. **递归**

- 如果根节点为 null，说明是一颗空树，返回 true；
- 如果当前节点的值小于等于最小值 min 或大于等于最大值 max，说明不满足 BST 的性质，返回 false；
- 递归验证左子树，左子树的值范围更新为 (min, node.val)，即左子树的值必须小于当前节点的值；
- 递归验证右子树，右子树的值范围更新为 (node.val, max)，即右子树的值必须大于当前节点的值；
- 如果左右子树都是 BST，且当前节点的值处于正确的范围内，返回 true。
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean validate(TreeNode node, long min, long max) {
        if (node == null) {
            return true;
        }
        if (node.val <= min || node.val >= max) {
            return false;
        }
        return  validate(node.left, min, node.val) && validate(node.right, node.val, max);
    }
}
```
2. **利用中序遍历的性质**

- 如果根节点为 null，说明是一颗空树，返回 true；
- 递归验证左子树，如果左子树不是 BST，返回 false；
- 检查当前节点的值是否大于上一个节点的值，如果是，则更新上一个节点的值为当前节点的值，并继续递归验证右子树；
- 如果不满足以上条件，说明不是 BST，返回 false；
- 如果遍历完所有节点都符合条件，返回 true。

这个算法利用了 BST 的性质：对于 BST 中序遍历得到的序列是递增的。因此，通过中序遍历验证节点值的顺序是否递增，就可以判断该二叉树是否为 BST。
```java
class Solution {
    double last = -Double.MAX_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        if (isValidBST(root.left)) {
            if (last < root.val) {
                last = root.val;
                return isValidBST(root.right);
            }
        }
        return false;
    }
}
```
利用二叉搜索树中序遍历出来是一个升序数组的特性。 利用一个TreeNode记录当前节点的上一个节点的值，然后将两个值不断比较查看是否符合升序的特性，如果不符合直接返回false，反之就是一颗二叉搜索树。
```java
class Solution {
    TreeNode pre = null;
    public boolean isValidBST(TreeNode root) {
        if(root == null){
            return true;
        }
        boolean left = isValidBST(root.left);
        if(pre != null){
            if(root.val <= pre.val){
                return false;
            }
        }
        pre = root;
        boolean right = isValidBST(root.right);
        return left && right;
    }
}
```

## 2024/3/29

### 70.爬楼梯

1. **动态规划**

对于爬楼梯问题，可以使用动态规划的方法，其时间复杂度为 O(n)。具体做法是，定义一个长度为 n+1 的数组 dp，其中 dp[i] 表示爬到第 i 阶楼梯的方法数。初始状态为 dp[0] = 1 和 dp[1] = 1，因为爬到第 0 阶和第 1 阶楼梯的方法数都是 1。然后从第 2 阶开始，逐步计算 dp[i] 的值，状态转移方程为 dp[i] = dp[i-1] + dp[i-2]，即到达第 i 阶楼梯的方法数等于到达第 i-1 阶楼梯的方法数加上到达第 i-2 阶楼梯的方法数。最终 dp[n] 就是所求的结果。
```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 1) {
            return 1;
        }
        
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        
        return dp[n];
    }
}

```
2. **拓展思考**

Java的话因为返回值为int，n=46时，结果会溢出，因此n < 46，那么就有：
```java
public int climbStairs(int n) {
    
    int result = 0;
    
    switch(n){
    case 1: result = 1; break;
    case 2: result = 2; break;
    case 3: result = 3; break;
    case 4: result = 5; break;
    case 5: result = 8; break;
    case 6: result = 13; break;
    case 7: result = 21; break;
    case 8: result = 34; break;
    case 9: result = 55; break;
    case 10: result = 89; break;
    case 11: result = 144; break;
    case 12: result = 233; break;
    case 13: result = 377; break;
    case 14: result = 610; break;
    case 15: result = 987; break;
    case 16: result = 1597; break;
    case 17: result = 2584; break;
    case 18: result = 4181; break;
    case 19: result = 6765; break;
    case 20: result = 10946; break;
    case 21: result = 17711; break;
    case 22: result = 28657; break;
    case 23: result = 46368; break;
    case 24: result = 75025; break;
    case 25: result = 121393; break;
    case 26: result = 196418; break;
    case 27: result = 317811; break;
    case 28: result = 514229; break;
    case 29: result = 832040; break;
    case 30: result = 1346269; break;
    case 31: result = 2178309; break;
    case 32: result = 3524578; break;
    case 33: result = 5702887; break;
    case 34: result = 9227465; break;
    case 35: result = 14930352; break;
    case 36: result = 24157817; break;
    case 37: result = 39088169; break;
    case 38: result = 63245986; break;
    case 39: result = 102334155; break;
    case 40: result = 165580141; break;
    case 41: result = 267914296; break;
    case 42: result = 433494437; break;
    case 43: result = 701408733; break;
    case 44: result = 1134903170; break;
    case 45: result = 1836311903; break;
    
    }
    return result;
}
```
### 118.杨辉三角

1. **动态规划**

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> list = new ArrayList<>();
        for(int i=0;i<numRows;i++){
            List<Integer> row=new ArrayList<>();
            for(int j=0;j<=i;j++){
                if(j==0||i==j){ //必须要有j==0
                    row.add(1);
                }else{
                    int num = list.get(i-1).get(j-1)+list.get(i-1).get(j);
                    row.add(num);
                }
            }
            list.add(row);
        }
        return list;
    }
}
```

### 230.二叉搜索树中第k小的元素

1. **二叉树转化为数组**

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        int[] array=treeToArray(root);
        return array[k-1];
    }
    public int[] treeToArray(TreeNode root) {
            List<Integer> list = new ArrayList<>();
            inOrderTraversal(root, list);
            int[] result = new int[list.size()];
            for (int i = 0; i < list.size(); i++) {
                result[i] = list.get(i);
            }
            return result;
        }
        
        private void inOrderTraversal(TreeNode root, List<Integer> list) {
            if (root == null) {
                return;
            }
            inOrderTraversal(root.left, list);
            list.add(root.val);
            inOrderTraversal(root.right, list);
        }
}
```

2. **中序遍历**

中序遍历是二叉搜索树的一个重要性质：它会按照升序的顺序访问树中的节点。因此，在二叉搜索树中，中序遍历会从最小的节点开始，逐渐访问到最大的节点。这样，如果我们进行中序遍历，并记录遍历到的第 k 个节点，就可以得到第 k 小的元素。

换句话说，利用中序遍历的性质，我们可以在遍历过程中找到第 k 小的元素，而不需要遍历整个树。这样做的好处是节省了时间，因为在一般情况下，中序遍历的时间复杂度是 O(n)，其中 n 是树中节点的数量，而直接遍历整棵树需要 O(n) 的时间。
```java
public class Solution {
    private int count = 0;
    private int result = 0;
    
    public int kthSmallest(TreeNode root, int k) {
        inOrderTraversal(root, k);
        return result;
    }
    
    private void inOrderTraversal(TreeNode root, int k) {
        if (root == null) {
            return;
        }
        inOrderTraversal(root.left, k);
        count++;
        if (count == k) {
            result = root.val;
            return;
        }
        inOrderTraversal(root.right, k);
    }
}

```

### 114.二叉树展开为链表

1. **先序遍历**

- 定义一个空列表 list，用于存储二叉树的节点；
- 使用先序遍历将二叉树的节点按顺序添加到 list 中；
- 遍历 list，对于列表中的每个节点，将其左子树设为 null，右子树设为列表中的下一个节点（如果存在）。
```java
class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> list = new ArrayList<>();
        preOrder(root,list);
        for(int i=0;i<list.size()-1;i++){
            list.get(i).left=null;
            list.get(i).right = list.get(i+1);
        }
    }
    public void preOrder(TreeNode root,List<TreeNode> list){
        if(root==null){
            return;
        }
        list.add(root);
        preOrder(root.left,list);
        preOrder(root.right,list);
    }
}
```

2. **深度优先搜索**
```java
class Solution {
    TreeNode link = new TreeNode();
    public void flatten(TreeNode root) {
        if(root == null) return;
        link.left = null;
        link.right = root;
        link = link.right;
        
        TreeNode left = root.left; // 因为dfs时会改变left和right 指针，所以需要提前保存
        TreeNode right = root.right;
        flatten(left);
        flatten(right);
    }
}
```
## 2024/4/1

### 115.最小栈

1. **使用链表**
```java
class MinStack {
    private Node head;
    public MinStack() {

    }
    
    public void push(int x) {
        if(head==null){
            head=new Node(x,x);
        }
        else{
            head=new Node(x,Math.min(x,head.min),head);
        }
    }
    
    public void pop() {
        head=head.next;
    }
    
    public int top() {
        return head.val;
    }
    
    public int getMin() {
        return head.min;
    }

    private class Node{
        int val;
        int min;
        Node next;

        private Node(int val,int min){
            this(val,min,null);
        }
        private Node(int val,int min ,Node next){
            this.val=val;
            this.min=min;
            this.next=next;
        }
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

2. **使用数组**
使用数组实现栈，内部维护栈顶，最小值指针。
```java
class MinStack {

   private  int[] stack;
   private  int top; // 栈顶
   private int min; //最小值指针
   
   public MinStack() {
       this.stack = new int[30000];
       this.top = -1;
       this.min = -1;
   }
   
   public void push(int val) {
       //压栈
       this.stack[++top] = val;
       
       if(min == -1){//判断是否是第一个元素
           min = top;
       }else{
           if(stack[min] > val){ //比较大小
               min = top;  //更新指针
           }
       }
   }
   
   public void pop() {
       top --; //出栈
       if(top == -1){//判断栈是否为空
           min = -1;
       }else if(min > top){//判断最小值指针是否合法
           
           long min_val = Long.MAX_VALUE;
           for(int i = 0;i<= top;i++){//重新找最小
               if(min_val > stack[i]){
                   min_val = stack[i];
                   min = i;
               }
           }
       }
   }
   
   public int top() {
       return stack[top];
   }
   
   public int getMin() {
       return stack[min];
   }
} 
```
### 23.合并K个升序链表
1. **傻瓜式**
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        List<Integer> list=new ArrayList<>();
        for(int i=0;i<lists.length;i++){
            ListNode n=lists[i];
            while(n!=null){
                list.add(n.val);
                n=n.next;
            }
        }
        Collections.sort(list);
        ListNode ans=new ListNode(-1),h=ans;
        for(int i=0;i<list.size();i++){
            h.next=new ListNode(list.get(i));
            h=h.next;
        }
        return ans.next;
    }
}
```