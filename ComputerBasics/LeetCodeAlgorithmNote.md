# 力扣刷题笔记

## 目录
[toc]

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
## 2024/4/6
### 105.从前序与中序遍历序列构造二叉树
1. **递归**
- **确定根节点**：前序遍历的第一个节点即为当前子树的根节点。
- **在中序遍历中找到根节点的位置**：根据前序遍历中的根节点的值，在中序遍历中找到对应的位置，从而确定左子树和右子树的范围。
- **递归构建左右子树**：根据确定的左右子树的范围，递归调用构建方法来构建左右子树。
- **返回根节点**：返回当前子树的根节点。
```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || inorder == null || preorder.length == 0 || inorder.length == 0) {
            return null;
        }
        return build(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }
    
    private TreeNode build(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
        if (preStart > preEnd || inStart > inEnd) {
            return null;
        }
        
        int rootVal = preorder[preStart]; // 根节点的值为前序遍历的第一个元素
        TreeNode root = new TreeNode(rootVal);
        
        int index = 0; // 在中序遍历中找到根节点的位置
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == rootVal) {
                index = i;
                break;
            }
        }
        
        // 根据中序遍历中根节点的位置，划分左右子树的范围
        int leftSize = index - inStart;
        
        // 递归构建左右子树
        root.left = build(preorder, preStart + 1, preStart + leftSize, inorder, inStart, index - 1);
        root.right = build(preorder, preStart + leftSize + 1, preEnd, inorder, index + 1, inEnd);
        
        return root;
    }
}

```
2. **递归-哈希表优化**

使用哈希表可以优化**查找根节点在中序遍历中的位置的过程**，从而提高构建二叉树的效率。具体来说，可以先构建一个哈希表，将中序遍历中每个节点的值和索引存储起来，然后在构建二叉树时，**根据先序遍历中的根节点值直接在哈希表中查找其在中序遍历中的位置**，而不再需要遍历中序遍历数组来查找。这样可以将查找根节点位置的时间复杂度从 O(n) 降低到 O(1)
```java
import java.util.HashMap;

class Solution {
    private HashMap<Integer, Integer> inorderMap;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || inorder == null || preorder.length == 0 || inorder.length == 0) {
            return null;
        }

        // 构建中序遍历哈希表
        inorderMap = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            inorderMap.put(inorder[i], i);
        }

        return build(preorder, 0, preorder.length - 1, 0, inorder.length - 1);
    }

    private TreeNode build(int[] preorder, int preStart, int preEnd, int inStart, int inEnd) {
        if (preStart > preEnd || inStart > inEnd) {
            return null;
        }

        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);

        int index = inorderMap.get(rootVal);
        int leftSize = index - inStart;

        root.left = build(preorder, preStart + 1, preStart + leftSize, inStart, index - 1);
        root.right = build(preorder, preStart + leftSize + 1, preEnd, index + 1, inEnd);

        return root;
    }
}

```
3. **迭代**

- 创建根节点并将其入栈。
- 初始化先序遍历和中序遍历的索引变量 preIndex 和 inIndex，并将它们分别初始化为 1 和 0。
- 循环执行以下步骤，直到先序遍历数组为空：
    1. 获取栈顶节点作为当前节点，并从栈中弹出。
    2. 如果当前节点的值等于中序遍历数组的值`（inorder[inIndex]）`，则将 inIndex 向后移动一位，并判断当前节点是否有右子节点，如果有，则将右子节点入栈。
    3. 否则，将当前节点的左子节点设置为先序遍历数组的下一个值，并将左子节点入栈，同时 preIndex 向后移动一位。
- 返回根节点。
```java
import java.util.Stack;

class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || inorder == null || preorder.length == 0 || inorder.length == 0) {
            return null;
        }

        TreeNode root = new TreeNode(preorder[0]);
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        int preIndex = 1;
        int inIndex = 0;

        while (preIndex < preorder.length) {
            TreeNode curr = stack.peek();
            if (curr.val == inorder[inIndex]) {
                while (!stack.isEmpty() && stack.peek().val == inorder[inIndex]) {
                    curr = stack.pop();
                    inIndex++;
                }
                curr.right = new TreeNode(preorder[preIndex]);
                curr = curr.right;
                stack.push(curr);
                preIndex++;
            } else {
                curr.left = new TreeNode(preorder[preIndex]);
                curr = curr.left;
                stack.push(curr);
                preIndex++;
            }
        }

        return root;
    }
}

```
### 121.买卖股票的最佳时机
1. **蛮力法（超时）**
```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        for(int i = 0; i < prices.length - 1; i++) {
            int max = prices[i];
            for(int j = i + 1; j < prices.length; j++) {
                if(prices[j] > max) {
                    max = prices[j];
                }
            }
            int profit = max - prices[i];
            if(profit > maxProfit) {
                maxProfit = profit;
            }
        }
        return maxProfit;
    }
}
```
2. **动态规划**
买的那天一定是卖的那天之前的最小值。 每到一天，维护那天之前的最小值即可。
```java
class Solution {
    public int maxProfit(int[] prices) {
        int len = prices.length;
        if (len == 0) return 0;
        int[][] dp = new int[len][2];
        dp[0][0] -= prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], -prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], prices[i] + dp[i - 1][0]);
        }
        return dp[len - 1][1];
    }
}

```
3. **贪心**
```java
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0, min = Integer.MAX_VALUE;
        for (int price : prices) {
            if (price < min) min = price;
            else max = Math.max(price - min, max); // 如果当前值大于最小值，且收益也更大。
        }
        return max;
    }
}

```
## 2024/4/7
### 198.打家劫舍

1. **普通回溯（超时）**
    - 有序列表若选择偷index就不能偷前一个屋子index-1，会触发警报。而index-2及之前的屋子都是可能的选项，那么此时若偷index能获得的最大收益为：dfs(index - 2) + nums[index]，也就是idnex-2及之前所能-获得的最大收益加上index处的收益。
    - 有序列表若选择不偷index，则前一个屋子idnex-1是可能的选项，那么此时由于idnex处无收益，当前可获得的最大收益为dfs(index - 1)。
   ```java
    class Solution {
    // 1、普通回溯（递归），时间复杂度过高（过多重复分支计算），超时
        public int rob(int[] nums) {
            int len = nums.length;
            return dfs(nums, len - 1);
        }
        public int dfs(int[] nums, int index){
            // 当数组下标<0时，说明已无人家可偷，直接返回此后可获得的收益0。
            if(index < 0){return 0;}
            return Math.max(dfs(nums, index - 1), dfs(nums, index - 2) + nums[index]);
        }
    }
   ```  
2. **记忆化搜索**
    只需要利用哈希表对搜索并计算过的分支形成”记忆“就可以解决，在搜索到某个分支（某一家index）时 ，先查询哈希表cache种是否有记录，若有则说明之前搜索过程中计算过，直接返回记忆值cache[index]，若cache[index]仍未数组初始值，则将本次计算结果录入表中，防止以后遇到再次重复计算。这样相当于所有的分支可能都只计算了一遍（低层次分支被查找的次数可能会很多，但是只有首次使用时需要计算，后面只需进行时间复杂度为1的查表操作即可）。
    ```java
    class Solution {
    // 2、递归回溯+记忆化搜索（有cache数组缓存，所有分支只需要计算一遍）
        int[] cache;
        public int rob(int[] nums) {
            
            int len = nums.length;
            cache = new int[len];
            for(int i = 0; i < len; ++i){   // 初始化记忆缓存数组cache
                cache[i] = -1;
            }
            return dfs(nums, len - 1);
        }
        public int dfs(int[] nums, int index){
            // 当数组下标<0时，说明已无人家可偷，直接返回此后可获得的收益0。
            if(index < 0){return 0;}
            // 查询该分支是否计算过，首次遇到则将计算结果录入缓存，避免以后重复计算
            if(cache[index] != -1){return cache[index];}
            cache[index] = Math.max(dfs(nums, index-1), dfs(nums, index-2) + nums[index]);
            return cache[index];
        }
    }
    ```

1. **动态规划**
    递归改递推优化，省去递归算法自顶向下的搜索过程（按灵神的说法是省去“递”，直接“归”），直接从第一家开始正向推导，cache[i + 2] = Math.max(cache[ i + 1], cache[ i ] + nums[i])。注意当计算cache[0]和cache[1]时，若按正常递推公式将会导致下标出现负数越界，所以可以将cache设置为n+2长，头两位初试默认为0占位，从cache[2]开始一一对应nums的下标进行递推计算，最终所求结果即为cache数组最后一位。
    ```java
    class Solution {
        public int rob(int[] nums) {
            // 自顶向下递归改为自底向上递推，从初始位置直接开始正向推算，但空间复杂度还是O(n)。
            int n = nums.length;
            // 由于计算cache[0]cache[1]时会出现数组下标为负的情况，所以将cache数组直接设定为n+2长，从cache[2]作为计算起点，之前的0和1位默认为零凑位，防止下标越界。
            int[] cache = new int[n + 2];
            for(int i = 0; i < n; ++i){
                cache[i + 2] = Math.max(cache[i + 1], cache[i] + nums[i]);
            }
            return cache[n + 1];
        }
    }
    ```
2. **dp压缩**
    递推再优化：前面引入cache记忆存储数组后时间复杂度已来到O(n)，但是空间复杂度O(n)还可以继续优化，因为可以发现，在迭代递推过程中，计算每一步cache[i + 2]时候只用到了他之前的两位cache[ i ]和cache[i+1]，以及对位的nums[i]，也就是说只需要用两个临时变量pre1，pre2交替存储当前位之前的cache即可，不需要始终维护整个n+2长度的记忆数组。
    ```java
    class Solution {
        public int rob(int[] nums) {
            // 滚动数组：递推再次优化空间，使用O(1)级别额外空间解决（舍弃cache数组，用两个临时变量交替存储i位及i+1位）
            int n = nums.length;
            int pre1 = 0, pre2 = 0, res = 0;
            for(int i = 0; i < n; ++i){
                res = Math.max(pre2, pre1 + nums[i]);
                pre1 = pre2;
                pre2 = res;
            }
            return res;
        }
    }
    ```
## 2024/4/9

### 416.分割等和子集

1. **动态规划**
    动态规划是解决这个问题的常用方法，可以根据数组元素生成一个和为总和一半的子集来考虑。步骤如下：
   - 首先计算数组的总和，如果总和是奇数，那么不可能划分成两个和相等的子集，直接返回`false`。
   - 如果总和是偶数，我们的任务就变成了找一个子集，其和为总和的一半。
   - 创建一个布尔数组`dp`，其中`dp[j]`表示是否存在一个子集的和为`j`。初始化`dp[0]`为`true`，因为不取任何元素时和为0肯定是可以实现的。
   - 遍历数组，对于每个元素，逆序更新`dp`数组：从`sum/2`遍历到当前元素大小，这是为了保证每个元素只被使用一次。如果`dp[j]`之前已经为`true`，或者在没有包括当前元素的情况下，`dp[j - 当前元素的值]`为`true`，那么在加上当前元素后，`dp[j]`应标记为`true`。
   - 最后，如果`dp[sum/2]`是`true`，那么返回`true`，否则返回`false`。
   ```java
    class Solution {
        public boolean canPartition(int[] nums) {
            int sum = 0;
            for (int num : nums) {
                sum += num;
            }
            // 如果总和是奇数，不能分割成两个和相等的子集
            if ((sum & 1) == 1) {
                return false;
            }

            sum /= 2;
            boolean[] dp = new boolean[sum + 1];
            dp[0] = true;

            for (int num : nums) {
                // 从sum遍历到num，防止重复使用元素
                for (int i = sum; i >= num; i--) {
                    // 转移方程
                    dp[i] = dp[i] || dp[i - num];
                }
            }

            return dp[sum];
        }
    }
   ``` 
1. **回溯(超时)**

    回溯法也可以解决此问题，通过尝试所有可能的分割方式来找到答案。
   - 考虑数组中的每一个数字，我们有两种选择：将其放入子集1，或者子集2。
   - 我们从第一个数字开始，递归地尝试两种可能性。如果在任何点子集1的和等于子集2的和，我们找到了答案。
   - 如果我们到达列表的末尾而没有找到答案，则返回false。

    注意：回溯法在大数组中可能会因为时间复杂度是指数级的而变得不实际。

    - 首先计算数组元素的总和。
    - 判断总和是否为偶数，因为只有总和为偶数，才可能划分为两个和相等的数组。
    - `canPartitionFrom` 是递归函数，它尝试添加数组中的元素到当前子集的和中或不添加，然后看看是否能够达到和为总和一半的情况。
    - 如果当前和等于总和的一半，说明找到了一个子集的和为总和的一半，返回`true`。
    - 如果当前和大于总和的一半或者所有元素都使用完成，而没有找到解决方案，则返回`false`。
    - 回溯通过尝试在子集中包括当前元素或排除当前元素，递归的进行下一个元素直至达到目标和或确定无法分割为等和子集。

    ```java
    class Solution {
        public boolean canPartition(int[] nums) {
            int total = 0;

            // 计算数组的总和
            for (int num : nums) {
                total += num;
            }
            
            // 如果总和不是偶数，则不能平分
            if (total % 2 != 0) {
                return false;
            }

            // 使用回溯法找到解
            return canPartitionFrom(0, nums, 0, total);
        }
        
        private boolean canPartitionFrom(int index, int[] nums, int currentSum, int total) {
            // 如果当前计算和的两倍等于总和，表示找到了平分的两个子集
            if (currentSum * 2 == total) {
                return true;
            }

            // 如果当前和超过了总和的一半或者下标超出数组长度，返回false
            if (currentSum > total / 2 || index >= nums.length) {
                return false;
            }

            // 选择当前数字，或者不选择当前数字，尝试递归找到答案
            return canPartitionFrom(index + 1, nums, currentSum + nums[index], total) ||
                canPartitionFrom(index + 1, nums, currentSum, total);
        }
    }
    ```
3. **二进制表示(超时)**

    另一个有趣的方法是使用整数的二进制表示来判断子集。每个数字可以选择“在子集中”（1）或“不在子集中”（0）。通过生成所有可能的二进制组合和相应的子集，我们可以检查是否存在一个子集其和为总和的一半。

    这种方法也会遇到时间复杂度和空间复杂度的问题，因为可能的子集数量是指数级的。

    ```java
    class Solution {
        public boolean canPartition(int[] nums) {
            int sum = 0;

            for (int num : nums) {
                sum += num;
            }

            // 数组总和必须是偶数，才可能分割成两个和相等的子集
            if (sum % 2 != 0) {
                return false;
            }

            int halfSum = sum / 2;
            int n = nums.length;

            // 穷举所有可能的组合
            for (int i = 1; i < (1 << n); i++) {
                int subsetSum = 0;

                for (int j = 0; j < n; j++) {
                    // 如果数字j在组合中，累加当前数字
                    if ((i & (1 << j)) != 0) {
                        subsetSum += nums[j];
                    }
                }

                // 检查当前组合的和是否等于总和的一半
                if (subsetSum == halfSum) {
                    return true;
                }
            }

            // 如果没有找到和为halfSum的子集，返回false
            return false;
        }
    }
    ```
4. **记忆化搜索(超时)**

    这是动态规划和回溯的组合，有时被称为记忆化递归。我们对每个元素使用递归，但是将中间结果缓存下来，避免重复计算，这样可以提高效率。

    - `Map<Pair, Boolean> memo`是一个`HashMap`，使用自定义的`Pair`类做键，其中`Pair`类包含了索引`int index`和当前的和`int sum`。
    - `canPartition`函数在每次递归调用时，会创建一个新的`Pair`实例作为状态的键，查找memo哈希表中是否有对应的值。
    - `Pair`类需要合适地实现`equals()`和`hashCode()`方法，确保`HashMap`可以正确地根据键的等价性存储和检索值。

    ```java
    import java.util.HashMap;
    import java.util.Map;
    import java.util.Objects;

    public class Solution {
        private Map<Pair, Boolean> memo = new HashMap<>();

        public boolean canPartition(int[] nums) {
            int sum = 0;
            for (int num : nums) {
                sum += num;
            }

            if (sum % 2 != 0) {
                return false;
            }

            return canPartition(nums, 0, sum / 2);
        }

        private boolean canPartition(int[] nums, int index, int currentSum) {
            if (currentSum == 0) {
                return true;
            }

            if (index >= nums.length || currentSum < 0) {
                return false;
            }

            Pair key = new Pair(index, currentSum);

            if (memo.containsKey(key)) {
                return memo.get(key);
            }

            boolean result = canPartition(nums, index + 1, currentSum - nums[index]) || 
                            canPartition(nums, index + 1, currentSum);
            memo.put(key, result);

            return result;
        }

        // 定义私有类来作为哈希表的键
        private static class Pair {
            int index;
            int sum;
            
            Pair(int index, int sum) {
                this.index = index;
                this.sum = sum;
            }

            @Override
            public boolean equals(Object o) {
                if (this == o) return true;
                if (o == null || getClass() != o.getClass()) return false;
                Pair pair = (Pair) o;
                return index == pair.index && sum == pair.sum;
            }

            @Override
            public int hashCode() {
                return Objects.hash(index, sum);
            }
        }
    }
    ```
## 2024/4/10

### 199.二叉树的右视图

1. **BFS**
- `rightSideView` 方法将返回一个包含二叉树右视图的整数列表。
- 我们首先检查根节点是否为`null`。如果是，就返回一个空列表。
- 接着，我们创建一个队列来进行层序遍历，并将根节点加入队列中。
- 然后，当队列不为空时，我们遍历每一层，对于每一层中的节点，我们从左- 到右进行遍历。但是，我们只添加每层最后一个节点的值到视图列表中，因为按照层序逻辑，每层的最后一个节点在其视图中是可见的。
- 在内部循环中，如果节点有左子节点和右子节点，我们先将左子节点加入队列，然后加入右子节点。这确保了在每一层遍历时，右子节点是最后处理的。

```java
public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> view = new ArrayList<>();
        if (root == null) {
            return view;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int levelLength = queue.size();
            for (int i = 0; i < levelLength; ++i) {
                TreeNode currentNode = queue.poll();
                // 如果是当前层的右侧节点，则加入视图列表中
                if (i == levelLength - 1) {
                    view.add(currentNode.val);
                }
                // 将子节点加入队列中。先左后右可以确保右侧节点最后处理
                if (currentNode.left != null) queue.offer(currentNode.left);
                if (currentNode.right != null) queue.offer(currentNode.right);
            }
        }

        return view;
    }
}
```
2. **DFS**

```java
class Solution {
    List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        rightView(root, result, 0);
        return result;
    }

    private void rightView(TreeNode curr, List<Integer> result, int currDepth){
        if (curr == null) {
            return;
        }
        // 如果当前层次还没有节点加入到结果中，添加当前节点
        if (currDepth == result.size()) {
            result.add(curr.val);
        }
        // 首先访问右子节点，然后访问左子节点
        rightView(curr.right, result, currDepth + 1);
        rightView(curr.left, result, currDepth + 1);
    }
}
```
## 2024/4/11
### 437.路径总和III
