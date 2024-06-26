# DataStructure
>数据结构

## 目录
[toc]
<!-- - [DataStructure](#datastructure)
  - [目录](#目录)
  - [一、线性结构](#一线性结构)
  - [二、树形结构](#二树形结构)
    - [2.1 树](#21树)
    - [2.2 二叉树](#22二叉树)
    - [2.3 平衡查找树](#23平衡查找树)
      - [2.3.1平衡二叉查找树](#231平衡二叉查找树)
        - [2.3.1.1 AVL](#2311avl)
        - [2.3.1.2 红黑树](#2312红黑树)
      - [2.3.2多路平衡查找树](#232多路平衡查找树)
        - [2.3.2.1 B树](#2321b树)
        - [2.3.2.2 B+树](#2322b树)
    - [2.4 trie树（前缀树）](#24trie树前缀树)
    - [2.5 字典树](#25字典树)
    - [2.6 哈夫曼树](#26哈夫曼树)
    - [2.7 堆](#27堆)
  - [三、图形结构](#三图形结构) -->

## 一、线性结构

### 1.1 数组

#### 1.1.1 一维数组

#### 1.1.2 二维数组

### 1.2 链表

#### 1.2.1 单链表
#### 1.2.2 双链表
#### 1.2.3 循环链表

### 1.3 字符串

### 1.4 栈

#### 1.4.1 基于数组实现
#### 1.4.2 基于链表实现

### 1.5 队列

#### 1.5.1 基于数组实现
#### 1.5.2 基于链表实现

## 二、树形结构

### 2.1 树

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
    - 通常用符号 \(\lfloor x \rfloor\) 表示小于等于 \(x\) 的最大整数。

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


### 2.2 二叉树
**二叉树的基本概念**

- 一棵二叉树是结点的一个有限集合，该集合或者为空，或者是由一个根节点加上两棵别称    为左子树和右子树的二叉树组成。

- 二叉树的特点：
    - 每个结点最多有两棵子树，即二叉树不存在度大于2的结点。
    - 二叉树的子树有左右之分，其子树的次序不能颠倒。

**二叉树的性质**

1. 非空二叉树上的叶子结点数等于**双分支结点数**+1
2. 非空二叉树的第$i$层上最多有个$2^{i-1}$个节点$(i\geq1)$
3. 高度为$h$的二叉树最多有$2^h-1$个结点$(h\geq1)$
4. 完全二叉树中层序编号为$i$的结点$(1 \leq i \leq n,n\geq1,n为节点数)$有以下性质
   1. 若$i\leq \lfloor n/2 \rfloor$,即$2i\leq n$,则编号为$i$的结点为分支结点，否则为叶子结点。
   2. 若$n$为奇数，则每个分支结点都既有左孩子结点，又有右孩子结点；若$n$为偶数，则编号最大的分支结点(编号为$\lfloor n/2 \rfloor$)只有左孩子结点，没有右孩子结点，其余结点都有左孩子结点和右孩子结点。
   3. 若编号为$i$的结点有左孩子结点，则左孩子结点的编号为$2i$；若编号为$i$的结点有右孩子结点，则右孩子结点的编号为$2i+1$。
   4. 除根节点以外，若一个结点的编号为$i$，则它的双亲结点的编号为$\lfloor i/2 \rfloor$。
5. 具有n个$(n>0)$结点的完全二叉树的高度为$\lfloor log_2(n+1) \rfloor$或$\lfloor log_2n \rfloor+1$

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
   - 基本操作-查找
![](/Res/images/二叉搜索树-查找.png)
    
    > 思路：
    > 如果根节点不为空
    >看根节点的值是否等于value,等于返回true
    >根节点的值>value，继续在根节点左子树查找
    >根节点的值<value，继续在根节点右子树查找

    ```java
        //查找
        public TreeNode serach(int val){
                TreeNode cur = root;
                while(cur != null){
                    if(cur.val > val){
                        cur = cur.left;
                    } else if (cur.val < val) {
                        cur = cur.right;
                    }
                    else{
                        return cur;
                    }
                }
                return null;
            }
    ```
   - 基本操作-插入
![](/Res/images/二叉搜索树-插入.png)

    > 思路：
    > 如果要插入的节点为空，root == null，直接插入，返回true
    >定义一个parent和cur，从root开始查找，当cur为空时，找到要插入的位置，即parent的下一个节点

    ```java
        //插入
        public boolean insert(int key){
        if(root == null){
            TreeNode root = new TreeNode(key);
            return true;
        }
        TreeNode parent = null;
        TreeNode cur = root;
        while(cur != null){
            if(cur.val > key){
                parent = cur;
                cur = cur.left;
            } else if (cur.val < key) {
                parent = cur;
                cur = cur.right;
            }
            else{
                return false;
            }
        }
        TreeNode node = new TreeNode(key);
        if(parent.val < key){
            parent.right = node;
        }
        else{
            parent.left = node;
        }
        return true;
    }
    ```
   - 基本操作-删除
    >思路：
    > 1. 删除叶子节点：如果要删除的节点是叶子节点（即没有子节点），直接删除即可。
    > 2. 删除只有一个子节点的节点：如果要删除的节点只有一个子节点，将该节点的父节点指向该节点的子节点即可。
    > 3. 删除有两个子节点的节点：如果要删除的节点有两个子节点，可以选择以下两种策略之一： 
    >    - 找到该节点右子树中的最小节点（或左子树中的最大节点），将其值复制到要删除的节点上，然后递归删除右子树中的最小节点（或左子树中的最大节点）。
    >    - 找到该节点左子树中的最大节点（或右子树中的最小节点），将其值复制到要删除的节点上，然后递归删除左子树中的最大节点（或右子树中的最小节点）。

    ```java
    class TreeNode {
        int val;        // 节点值
        TreeNode left, right;   // 左右子节点
        public TreeNode(int val) {
            this.val = val;
            left = right = null;
        }
    }
    class BST {
        TreeNode root;  // 根节点
        // 删除节点
        public TreeNode deleteNode(TreeNode root, int key) {
            if (root == null) return root;  // 若根节点为空，直接返回

            // 在左子树中删除
            if (key < root.val) {
                root.left = deleteNode(root.left, key);
            } 
            // 在右子树中删除
            else if (key > root.val) {
                root.right = deleteNode(root.right, key);
            } 
            // 找到要删除的节点
            else {
                // 如果左子树为空，直接返回右子树
                if (root.left == null) return root.right;
                // 如果右子树为空，直接返回左子树
                else if (root.right == null) return root.left;
                // 如果左右子树均不为空，找到右子树中的最小节点
                root.val = findMin(root.right).val;
                // 在右子树中递归删除最小节点
                root.right = deleteNode(root.right, root.val);
            }
            return root;
        }
        // 找到以node为根节点的子树中的最小节点
        private TreeNode findMin(TreeNode node) {
            while (node.left != null) {
                node = node.left;
            }
            return node;
        }
    }
    ```
    - 二叉搜索树转化为数组

    ```java
    import java.util.*;

    public class Solution {
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

1. **线索二叉树（Threaded Binary Tree）**：
   - 线索二叉树是对普通二叉树进行改进，以便在遍历树时提供更快的访问。
   - 在线索二叉树中，除了左子节点和右子节点之外，每个节点还有一个指向中序遍历下的前驱节点和后继节点的指针。
   - 线索化可以使得中序遍历的过程中，无需使用递归或栈来实现。
![](/Res/images/线索二叉树1.png)
![](/Res/images/线索二叉树2.png)
   - 线索二叉树的基本操作
    线索二叉树是对普通二叉树的一种改进，它通过添加额外的指针，使得遍历操作更加高效。主要的基本操作包括：

     1. 线索化：线索化是将普通二叉树转化为线索二叉树的过程。在线索化过程中，**将原本为空的指针指向前驱节点或后继节点**，以方便后续的遍历操作。

     2. 中序线索化：中序线索化是指在中序遍历的基础上线索化二叉树。对于每个节点，将其左子树的最右节点（前驱节点）和右子树的最左节点（后继节点）进行连接。

     3. 中序遍历：中序遍历是线索二叉树中最常用的遍历方式，可以按照节点的大小顺序输出所有节点的值。由于线索化的存在，中序遍历不再需要递归或者使用栈，因此效率更高。

     4. 先序遍历、后序遍历：除了中序遍历外，线索二叉树也可以进行先序遍历和后序遍历。先序遍历顺序是根节点、左子树、右子树，后序遍历顺序是左子树、右子树、根节点。

     5. 查找：线索二叉树的查找操作与普通二叉树相同，可以根据节点的值进行查找，也可以根据遍历顺序进行查找。

     6. 插入和删除：线索二叉树的插入和删除操作与普通二叉树类似，但需要保持线索的正确性。在插入和删除节点时，需要更新线索指针，以保证线索二叉树的线索化状态不变。  

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
    //递归
    class Solution {
        public List<Integer> preorderTraversal(TreeNode root) {
            List<Integer> result = new ArrayList<>();
            preorder(root, result);
            return result;
        }
        
        private void preorder(TreeNode root, List<Integer> result) {
            if (root == null) {
                return;
            }
            // 先访问根节点
            result.add(root.val);
            // 递归遍历左子树
            preorder(root.left, result);
            // 递归遍历右子树
            preorder(root.right, result);
        }
    }

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
        while(!stack.isEmpty())
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
    //递归
    class Solution {
        public List<Integer> inorderTraversal(TreeNode root) {
            List<Integer> result = new ArrayList<>();
            inorder(root, result);
            return result;
        }
        
        private void inorder(TreeNode root, List<Integer> result) {
            if (root == null) {
                return;
            }
            // 递归遍历左子树
            inorder(root.left, result);
            // 访问根节点
            result.add(root.val);
            // 递归遍历右子树
            inorder(root.right, result);
        }
    }
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
    //递归
    class Solution {
        public List<Integer> postorderTraversal(TreeNode root) {
            List<Integer> result = new ArrayList<>();
            postorder(root, result);
            return result;
        }
        
        private void postorder(TreeNode root, List<Integer> result) {
            if (root == null) {
                return;
            }
            // 递归遍历左子树
            postorder(root.left, result);
            // 递归遍历右子树
            postorder(root.right, result);
            // 访问根节点
            result.add(root.val);
        }
    }
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
    //chatGPT
    class Solution {
        public List<List<Integer>> levelOrder(TreeNode root) {
            List<List<Integer>> result = new ArrayList<>();
            if (root == null) {
                return result;
            }
            
            Queue<TreeNode> queue = new LinkedList<>();
            queue.offer(root);
            
            while (!queue.isEmpty()) {
                int size = queue.size();
                List<Integer> level = new ArrayList<>();
                for (int i = 0; i < size; i++) {
                    TreeNode node = queue.poll();
                    level.add(node.val);
                    if (node.left != null) {
                        queue.offer(node.left);
                    }
                    if (node.right != null) {
                        queue.offer(node.right);
                    }
                }
                result.add(level);
            }
            
            return result;
        }
    }
    //打印
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
### 2.3 平衡查找树

1. 平衡查找树是一类数据结构，在这类数据结构中，树的高度或深度保持在对数级别，以确保各种动态集合操作（如插入、删除、查找等）能够在对数时间内进行。平衡查找树的关键特点是，它们通过特定的算法来维持树的平衡性，从而避免退化成与链表相似的性能，即避免了最坏情况下的线性时间操作。
2. 在平衡查找树中，"平衡"并不意味着树完全平衡，而是指差异在可接受的范围内，通常是子树高度的一个固定界限。这样，即便是在插入或删除节点后，这种数据结构也可以通过自平衡操作（如旋转）来保持其高度较低，从而保持操作效率。
3. AVL树和红黑树是两种最典型的平衡查找树
4. 平衡查找树的目的是为了优化查找操作，以及与此相关的其他动态集合操作的性能，特别是在频繁改动数据集的应用场景中。
#### 2.3.1 平衡二叉查找树
##### 2.3.1.1 AVL
1. **AVL树的概念和特点：**   

AVL树是第一个被发明的自平衡二叉搜索树，在插入和删除节点时，AVL树会通过一系列旋转操作来保证树的平衡，确保任何时候任何节点的两个子树的高度差不超过1。这个特性使得AVL树在最坏的情况下也能保证查找、插入、删除操作时间复杂度为O(log n)。

2. **AVL树的基本操作：**
  
- 查找：和普通的二叉搜索树一样进行。
- 插入：首先像在普通二叉搜索树一样插入节点。之后，检查沿着从插入点到根节点路径上的每个节点，确保它们满足AVL树的平衡条件。如果发现某节点不平衡，进行相应的旋转操作。
- 删除：删除操作首先也是按照二叉搜索树的删除方式进行。删除之后，沿着从删除点到根节点路径上的每个节点进行平衡性检查，并进行必要的旋转操作。
- 旋转操作：AVL树通过四种旋转操作来保持平衡：左旋转、右旋转、左右旋转和右左旋转。
3. **AVL的代码实现：**
```java
class AVLNode {
    int key, height;
    AVLNode left, right;

    AVLNode(int d) {
        key = d;
        height = 1;
    }
}

class AVLTree {
    AVLNode root;

    // 获取节点的高度
    int height(AVLNode N) {
        if (N == null)
            return 0;
        return N.height;
    }

    // 右旋操作
    AVLNode rightRotate(AVLNode y) {
        AVLNode x = y.left;
        AVLNode T2 = x.right;

        // 执行旋转
        x.right = y;
        y.left = T2;

        // 更新高度
        y.height = Math.max(height(y.left), height(y.right)) + 1;
        x.height = Math.max(height(x.left), height(x.right)) + 1;

        // 返回新的根
        return x;
    }

    // 左旋操作
    AVLNode leftRotate(AVLNode x) {
        AVLNode y = x.right;
        AVLNode T2 = y.left;

        // 执行旋转
        y.left = x;
        x.right = T2;

        // 更新高度
        x.height = Math.max(height(x.left), height(x.right)) + 1;
        y.height = Math.max(height(y.left), height(y.right)) + 1;

        // 返回新的根
        return y;
    }

    // 获取平衡因子
    int getBalance(AVLNode N) {
        if (N == null)
            return 0;
        return height(N.left) - height(N.right);
    }

    // 插入节点
    AVLNode insert(AVLNode node, int key) {
        /* 1. 执行常规BST插入 */
        if (node == null)
            return (new AVLNode(key));

        if (key < node.key)
            node.left = insert(node.left, key);
        else if (key > node.key)
            node.right = insert(node.right, key);
        else // 不允许出现重复的关键字
            return node;

        /* 2. 更新节点的高度 */
        node.height = 1 + Math.max(height(node.left),
                                  height(node.right));

        /* 3. 获取节点的平衡因子，检查该节点是否失衡 */
        int balance = getBalance(node);

        // 如果节点失衡，有四种情况：

        // 左左情况
        if (balance > 1 && key < node.left.key)
            return rightRotate(node);

        // 右右情况
        if (balance < -1 && key > node.right.key)
            return leftRotate(node);

        // 左右情况
        if (balance > 1 && key > node.left.key) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }

        // 右左情况
        if (balance < -1 && key < node.right.key) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }

        /* 返回节点指针 */
        return node;
    }

    // 其他方法如删除节点等...
}
```
##### 2.3.1.2 红黑树
1. **红黑树的概念**

红黑树相较于AVL树提供了更加宽松的平衡条件，允许路径长度的差异更大。它通过确保树中不存在两个连续的红节点并且任一节点到其每个叶子节点的所有路径上包含相同数目的黑节点，来维持平衡。

2. **红黑树的特点** 
    - 节点颜色：每个节点要么是红的，要么是黑的。
    - 根节点：树的根节点是黑色的。
    - 红色节点规则：如果一个节点是红色的，那么它的两个子节点都是黑色的。
    - 黑色高度：从任一节点到其叶子的所有路径都包含相同数目的黑色节点。
    - 叶子节点：所有叶子节点（NIL或空节点）都是黑色的。

3. **红黑树的代码实现**

#### 2.3.2 多路平衡查找树
##### 2.3.2.1 B树
##### 2.3.2.2 B+树 
### 2.4 trie树（前缀树）
### 2.5 字典树
### 2.6 哈夫曼树
### 2.7 堆

## 三、图形结构
