# DataStructure
>数据结构

## 目录

- [DataStructure](#datastructure)
  - [目录](#目录)
  - [一、线性结构](#一线性结构)
  - [二、树形结构](#二树形结构)
    - [树](#树)
    - [二叉树](#二叉树)
    - [平衡查找树](#平衡查找树)
      - [平衡二叉树](#平衡二叉树)
        - [AVL](#avl)
        - [红黑树](#红黑树)
      - [多路查找树（特殊）](#多路查找树特殊)
        - [B树](#b树)
        - [B+树](#b树-1)
    - [trie树（前缀树）](#trie树前缀树)
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

12. **遍历（Traversal）：** 访问树中所有节点的过程。常见的遍历方式包括**前序遍历**（pre-order）、**中序遍历**（in-order）、**后序遍历**（post-order）和**层序遍历**（level-order）等。（补充：BFS（层序遍历），DFS，归属于图遍历算法）。

![树的基本概念](/Res/images/树的基本概念.png "树的基本概念")

**树的性质：**

1. 树中的节点数等于所有结点的度数之和加1
2. 度为$m$的树中第$i$层上最多有$m^{i-1}$个结点（$i\geq1$）
3. 高度为$h$的$m$次树最多有$\frac{m^h-1}{m-1}$个节点
4. 具有$n$个结点的$m$次树的最小高度为$\lceil \log_m{(n(m-1)+1)} \rceil$

    - 注：$\lceil x \rceil$表示大于等于x的最小整数。

**树的存储结构&实现**

1. **链式存储结构：** 每个节点通过指针或引用指向其子节点。链式存储结构可以分为单链表结构、双链表结构和多叉树结构。
   - 单链表结构：每个节点包含指向其一个子节点的引用。
   - 双链表结构：每个节点包含指向其左子节点和右子节点的引用。
   - 多叉树结构：每个节点包含指向其多个子节点的引用，可以是数组、列表或其他数据结构。

2. **顺序存储结构：** 使用数组来存储树的节点。通过数组的索引关系来表示节点之间的父子关系。
   - 索引为i的节点的左子节点在索引2*i+1处，右子节点在索引2*i+2处。
   - 索引为i的节点的父节点在索引(i-1)/2处。

3. **堆存储结构：** 堆是一种特殊的树结构，通常使用数组来实现。堆可以分为最大堆和最小堆。
   - 最大堆：每个节点的值都大于或等于其子节点的值。
   - 最小堆：每个节点的值都小于或等于其子节点的值。
   - 使用数组来表示堆，根节点存储在索引0处，具有以下性质：
     - 索引为i的节点的左子节点在索引`2*i+1`处，右子节点在索引`2*i+2`处。
     - 索引为i的节点的父节点在索引(i-1)/2处。

4. **其他存储结构：** 还有一些特殊的存储结构，例如树状数组（Binary Indexed Tree）和线段树（Segment Tree），它们具有特定的用途和实现方式。

选择合适的存储结构取决于具体的应用场景和性能需求。链式存储结构适合动态插入和删除操作，而顺序存储结构适合频繁的随机访问操作。

**树的基本操作：**
1. 创建
```java
// 树节点类
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    
    public TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}
// 树类
class Tree {
    TreeNode root;
    
    public Tree() {
        this.root = null;
    }
}
```
2. 查找
    - 在树中查找特定值的节点。通常，可以使用递归或迭代的方式进行查找操作。
    - 如果树是二叉搜索树（BST），查找操作可以利用BST的特性，在左子树中查找小于当前节点值的节点，在右子树中查找大于当前节点值的节点。
3. 插入
    - 将一个新节点插入到树中的适当位置，以保持树的结构不变。
    - 对于二叉搜索树（BST），插入操作需要遵循BST的规则：新节点的值小于当前节点值则插入到左子树，大于当前节点值则插入到右子树。
4. 删除
    - 从树中移除指定值的节点。
    - 删除操作需要考虑被删除节点的位置和其子树的调整，以保持树的结构和性质不变。
    - 删除操作通常分为三种情况：
       - 若被删除节点为叶子节点，直接删除即可。
       - 若被删除节点只有一个子节点，将其子节点移动到被删除节点的位置。
       - 若被删除节点有两个子节点，可以选择将被删除节点的前驱或后继节点替代被删除节点，并删除原前驱或后继节点。
5. 遍历
   1. 前序遍历
   ![](/Res/images/树的先序遍历.png) 
   2. 中序遍历
   ![](/Res/images/树的中序遍历.png) 
   3. 后序遍历
   ![](/Res/images/树的后序遍历.png) 
   4. 层序遍历(广度优先搜索BFS)
   ![](/Res/images/树的层次遍历.png) 


### 二叉树
**二叉树的基本概念**

- 一棵二叉树是结点的一个有限集合，该集合或者为空，或者是由一个根节点加上两棵别称    为左子树和右子树的二叉树组成。

- 二叉树的特点：
    - 每个结点最多有两棵子树，即二叉树不存在度大于2的结点。
    - 二叉树的子树有左右之分，其子树的次序不能颠倒。

**二叉树的性质**

**特殊的二叉树**



1. **满二叉树（Full Binary Tree）**：
   - 满二叉树是一种特殊的二叉树，其中每个节点要么是叶子节点，要么具有两个子节点。
   - 除了叶子节点外，每个节点都有两个子节点。
   - 满二叉树的深度（高度）通常是最小的，因为它尽可能地填充了每一层的节点。

![](/Res/images/满二叉树.png)

2. **完全二叉树（Complete Binary Tree）**：
   - 完全二叉树是一种二叉树，除了最后一层外，其他每一层都被完全填充，并且所有节点都尽可能地向左对齐。
   - 最后一层上的节点在左侧连续排列，缺少的位置只能出现在最右侧。
   - 完全二叉树常用于堆数据结构的实现。
![](/Res/images/完全二叉树.png)

3. **二叉搜索树（Binary Search Tree，BST）**：
   - 二叉搜索树是一种特殊的二叉树，其中每个节点的值大于其左子树中的所有节点的值，且小于其右子树中的所有节点的值。
   - 由于其特定的结构，二叉搜索树支持高效的插入、删除和查找操作。
   - 基本操作

4. **线索二叉树（Threaded Binary Tree）**：
   - 线索二叉树是对普通二叉树进行改进，以便在遍历树时提供更快的访问。
   - 在线索二叉树中，除了左子节点和右子节点之外，每个节点还有一个指向中序遍历下的前驱节点和后继节点的指针。
   - 线索化可以使得中序遍历的过程中，无需使用递归或栈来实现。

这些二叉树结构在计算机科学中都有着重要的应用，满二叉树和完全二叉树通常用于算法分析，而二叉搜索树和线索二叉树则用于提供高效的数据操作。

**二叉树的存储结构&实现**

1. 顺序存储结构
2. 链式存储结构

**二叉树的基本操作**
1. 创建
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}

    public TreeNode(int val) {
        this.val = val;
    }

    TreeNode(int val,TreeNode left,TreeNoded right){
        this.val=val;
        this.left=left;
        this.right=right;
    }
}

```
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
   2. 中序遍历
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
   3. 后序遍历
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
   4. 层序遍历
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

### 平衡查找树
#### 平衡二叉树
##### AVL
##### 红黑树
#### 多路查找树（特殊）
##### B树
##### <a id = "bplus"></a>B+树 
### trie树（前缀树）
### 字典树
### 哈夫曼树
### 堆

## 三、图形结构
