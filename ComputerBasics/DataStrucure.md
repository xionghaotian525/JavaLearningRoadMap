# DataStructure
>数据结构

## 目录

- [DataStructure](#datastructure)
  - [目录](#目录)
  - [一、线性结构](#一线性结构)
  - [二、树形结构](#二树形结构)
    - [树](#树)
    - [二叉树](#二叉树)
    - [二叉搜索树](#二叉搜索树)
    - [平衡查找树](#平衡查找树)
      - [平衡二叉树](#平衡二叉树)
        - [AVL](#avl)
        - [红黑树](#红黑树)
      - [多路查找树](#多路查找树)
        - [B树](#b树)
        - [B+树](#b树-1)
    - [字典树](#字典树)
    - [哈夫曼树](#哈夫曼树)
    - [堆](#堆)
  - [三、图形结构](#三图形结构)

## 一、线性结构

>复习时总结

## 二、树形结构

### 树

**树的基本概念：**

树（Tree）是一种重要的数据结构，它由节点（Node）和连接这些节点的边（Edge）组成。

1. **节点（Node）：** 树的基本单位，包含数据元素和指向其他节点的链接（子节点）。

2. **节点的度与树的度：** 节点的度是指该节点拥有的子节点（或称为子树）的数量;树的度是指树中各个节点的度的最大值。通常将度为m的树称为**m次树（m-tree）**。

3. **根节点（Root Node）：** 树的顶端节点，是整个树的**起始节点**，没有父节点。

4. **父节点（Parent Node）：** 某个节点直接指向的节点称为其父节点。

5. **子节点（Child Node）：** 某个节点直接指向的节点称为其子节点。

6. **兄弟节点：** 具有同一个父节点的子节点互为兄弟节点

7. **叶子节点（Leaf Node）：** 没有子节点的节点称为叶子节点。

8. **路径（Path）：** 在树中，路径（Path）是指从树中的一个节点到另一个节点的序列。路径的长度（Path Length）是指沿着路径走过的边的数量。

9. **结点层次（Level）：** 也称为节点深度，根节点的层次为1，每个节点的层次是其父节点的层次加1。

10. **树的高度（Height）：** 也称为树的深度,树中节点的最大层次称为树的高度或树的深度。

11. **子树（Subtree）：** 以某个节点为根节点的所有节点和边构成的树。

12. **遍历（Traversal）：** 访问树中所有节点的过程。常见的遍历方式包括**前序遍历**（pre-order）、**中序遍历**（in-order）、**后序遍历**（post-order）和**层序遍历**（level-order）等。

![树的基本概念](/Res/images/树的基本概念.png "树的基本概念")

**树的性质：**

待学习markdown中公式的表示方法

**树的存储结构**
1. 双亲表示法
2. 孩子表示法
3. 孩子兄弟表示法
![孩子兄弟表示法](/Res/images/树的存储结构孩子兄弟表示法2.png)

**树的基本操作：**
1. 创建
2. 查找
3. 插入
4. 删除
5. 遍历
   1. 前序遍历
   ![](/Res/images/树的先序遍历.png) 
   3. 中序遍历
   ![](/Res/images/树的中序遍历.png) 
   5. 后序遍历
   ![](/Res/images/树的后序遍历.png) 
   7. 层序遍历
   ![](/Res/images/树的层次遍历.png) 


### 二叉树
**二叉树的基本概念**

**二叉树的性质**

**特殊的二叉树**
1. 满二叉树
2. 完全二叉树
**二叉树的存储**
1. 使用数组
2. 使用链表

**二叉树的基本操作**
1. 创建
2. 查找
   1. 获取二叉树节点个数
   ```java
    public int size(Node root)
    {
        if(root==null)
        return 0;
        return size(root.left)+size(root.right)+1;
    }
   ``` 
   2. 获取叶子结点个数
    ```java
    // 获取叶子节点的个数
    public int getLeafNodeCount(Node root)
    {
        if(root==null)
        {
            return 0;
        }
        if(root.left==null&&root.right==null)
            return 1;
        return getLeafNodeCount(root.left)+getLeafNodeCount(root.right);
    }
    ```
   3. 获取第k层节点个数
    ```java
    public int getKLevelNodeCount(TreeNode root, int k) {
        if(root == null) {
            return 0;
        }
        if(k == 1) {
            return 1;
        }
        return getKLevelNodeCount(root.left,k-1)
                + getKLevelNodeCount(root.right,k-1);
    }
    ``` 
   4. 获取二叉树高度
    ```java
    public  int getHeight(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int leftH = getHeight(root.left);
        int rightH = getHeight(root.right);
 
        return (leftH > rightH ? leftH :rightH) + 1;
    }
    ``` 
   5. 检测是否存在value值 
    ```java
     public TreeNode find(TreeNode root,int val) {
        if(root == null) return null;
        if(root.val == val) {
            return root;
        }
        TreeNode leftL = find(root.left,val);
        if(leftL != null) {
            return leftL;
        }
        TreeNode leftLR = find(root.right,val);
        if(leftLR != null) {
            return leftLR;
        }
        return null;
    }
    ```
3. 插入
4. 删除
5. 遍历
   1. 前序遍历
   ```java
    //递归方法
    public void preOrder(TreeNode root) {
        if(root == null) return;
        System.out.print(root.val+" ");
        preOrder(root.left);
        preOrder(root.right);
    }
    //非递归方法实现，利用栈的原理
    public void preOrderIteration(TreeNode head) {
    if(head==null){
       return;
    }
    Stack<TreeNode> stack=new Stack<>();
    stack.push(head);
        while(！stack.isEmpty())
        {
            TreeNOde node=stack.pop();
            System.out.print(node.val+" ");
            if(node.right!=null)
            {
                stack.push(node.left);
            }
            if(node.left!=null){
                stack.push(node.right);
            }
        }
    }
   ``` 
   3. 中序遍历
   ```java
    //递归方法
    public void inOrder(TreeNode root) {
        if(root == null) return;
        inOrder(root.left);
        System.out.print(root.val+" ");
        inOrder(root.right);
    }
    //非递归方法实现
     public void inOrderIteration(TreeNode head) {
        if(head==null){
        return;
        }
        Stack<TreeNode> stack=new Stack<>();
        while(!stack.isEmpty())
        {
            while(cur!=null)
            {
            stack.push(cur);
            cur=cur.left;
            }
            TreeNode node=stack.pop();
            System.out.print(node.val+"");
            if(node.right!=null)
            {
                cur=node.right;
            }
        }
    }
   ``` 
   5. 后序遍历
   ```java
    //递归方法
    public void postOrder(TreeNode root) {
        if(root == null) return;
        postOrder(root.left);
        postOrder(root.right);
        System.out.print(root.val+" ");
    }
    //非递归方法实现
   ```  
   7. 层序遍历
   ```java
    public void levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
       if(root != null) {
           queue.offer(root);
       }
       while (!queue.isEmpty()) {
           TreeNode top = queue.poll();
           System.out.print(top.val+" ");
           if(top.left != null) {
               queue.offer(top.left);
           }
           if(top.right != null) {
               queue.offer(top.right);
           }
       }
 
    }
   ``` 

### 二叉搜索树
### 平衡查找树
#### 平衡二叉树
##### AVL
##### 红黑树
#### 多路查找树
##### B树
##### <a id = "bplus"></a>B+树 
### 字典树
### 哈夫曼树
### 堆

## 三、图形结构
