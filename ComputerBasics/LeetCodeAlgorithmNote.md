# 力扣刷题笔记

## 目录

- [2024/3/27]()
  - [94.二叉树的中序遍历](#94二叉树的中序遍历)
  - [104.二叉树的最大深度](#104二叉树的最大深度)
  - [226.翻转二叉树](#226翻转二叉树)
  - [101.对称二叉树]()
- []()
- []()
- []()

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