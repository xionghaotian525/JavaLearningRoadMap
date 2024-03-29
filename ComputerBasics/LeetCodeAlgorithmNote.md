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
>左右等分建立左右子树，中间节点作为子树根节点，递归该过程

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

## 2024/3/29

