

# Attack on Algorithm - 树 🐝 

## 定义

**树 :** 由 `n≥0*n*≥0` 个节点与节点之间的关系组成的有限集合 ; 当 `n=0`  时称为空树 , 当 `n>0` 时称为非空树

![tree-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-01.png?raw=true)

树是一种非线性数据结构 , 树结构的基本单位是节点 : 

- **边 :** 节点之间的链接
- **根节点 :** 节点与分支形成树状 , 结构的开端
- **子节点 :** 根节点之外的节点 , 度不为0
- **叶节点 :** 没有链接到其他子节点的节点 , 度为0
- **节点的度 :** 节点的子节点数量
- **树的度 :** 树中节点的最大度数
- **树的高度 :** 从根节点到叶子节点的最长路径长度 (边的数量)
- **节点深度 :** 从根节点到该节点的路径长度 (边的数量)
- **节点高度 :** 从该节点到叶子节点的最长路径长度 (边的数量)

### 二叉树

**二叉树 :** 树中各个节点的度不大于 `2` 个的有序树 , 称为二叉树 , 通常树中的分支节点被称为 **左子树** 或 **右子树**

![tree-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-02.png?raw=true)

#### 满二叉树

**满二叉树 (Full Binary Tree) : **如果所有分支节点都存在左子树和右子树 , 并且所有叶子节点都在同一层上 , 则称该二叉树为满二叉树

满二叉树满足以下特点 : 

- 叶子节点只出现在最下面一层
- 非叶子节点的度一定为 2
- 在同等深度的二叉树中 , 满二叉树的节点个数最多 , 叶子节点个数最多

![tree-03](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-03.png?raw=true)

如果我们对满二叉树的节点进行编号 , 根节点编号为 1 , 然后按照层次依次向下 , 每一层从左至右的顺序进行编号 , 则深度为 *$k$* 的满二叉树最后一个节点的编号为 $2^k-1$

#### 完全二叉树

**完全二叉树 (Complete Binary Tree) : **如果叶子节点只能出现在最下面两层 , 并且最下层的叶子节点都依次排列在该层最左边的位置上 , 具有这种特点的二叉树称为完全二叉树

完全二叉树满足以下特点 : 

- 叶子节点只能出现在最下面两层
- 最下层的叶子节点一定集中在该层最左边的位置上 
- 倒数第二层如果有叶子节点 , 则该层的叶子节点一定集中在右边的位置上 
- 如果节点的度为 1 , 则该节点只偶遇左孩子节点 , 即不存在只有右子树的情况
- 同等节点数的二叉树中 , 完全二叉树的深度最小

![tree-04](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-04.png?raw=true)

### 二叉搜索树

**二叉搜索树 (Binary Search Tree) : **也叫做二叉查找树、有序二叉树或者排序二叉树 , 是指一棵空树或者具有下列性质的二叉树 : 

- 如果任意节点的左子树不为空 , 则左子树上所有节点的值均小于它的根节点的值
- 如果任意节点的右子树不为空 , 则右子树上所有节点的值均大于它的根节点的值
- 任意节点的左子树、右子树均为二叉搜索树。

![tree-05](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-05.png?raw=true)

### AVL树

**AVL树(Adelson-Velsky and Landis Tree) : **一种结构平衡的二叉搜索树 , 即叶节点高度差的绝对值不超过 1 , 并且左右两个子树都是一棵平衡二叉搜索树 , 平衡二叉树可以在 $O(logn)$ 内完成插入、查找和删除操作

AVL 树满足以下性质 : 

- 空二叉树是一棵 AVL 树
- 如果 T 是一棵 AVL 树 , 那么其左右子树也是 AVL 树 , 并且 $∣h(ls)−h(rs)∣≤1$， $h(ls)$是左子树的高度 , $h(rs)$ 是右子树的高度
- AVL 树的高度为 $O(logn)$

![tree-06](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-06.png?raw=true)



## 二叉树存储

**二叉树**的存储结构分为两种 : 顺序存储结构和链式存储结构

### 顺序存储

二叉树的顺序存储结构使用一维数组来存储二叉树中的节点 , 节点存储位置则采用完全二叉树的节点层次编号 , 按照层次从上至下 , 每一层从左至右的顺序依次存放二叉树的数据元素 

在进行顺序存储时 , 如果对应的二叉树节点不存在 , 则设置为空节点

下图为二叉树的顺序存储结构 : 

![tree-07](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-07.png?raw=true)



节点之间的逻辑关系 : 

- 如果某二叉树节点 (非叶子节点) 的下标为 $i$ , 那么其左孩子节点下标为 $2∗i+1$ , 右孩子节点下标为 $2∗i+2$ 
- 如果某二叉树节点 (非根节点) 的下标为 $i$ , 那么其根节点下标为 $(i−1)//2$ , $//$ 表示整除

对于完全二叉树 (尤其是满二叉树) 来说 , 采用顺序存储结构比较合适 , 它能充分利用存储空间 ; 而对于一般二叉树 , 如果需要设置很多的空节点 , 则采用顺序存储结构就会浪费很多存储空间 , 并且 , 由于顺序存储结构固有的一些缺陷 , 会使得二叉树的插入、删除等操作不方便 , 效率也比较低 

对于二叉树来说 , 当树的形态和大小经常发生动态变化时 , 更适合采用链式存储结构

### 链式存储

二叉树采用链式存储结构时 , 每个链节点包含一个用于数据域 $val$ , 存储节点信息 ; 还包含两个指针域 `left` 和 `right` , 分别指向左右两个孩子节点 , 当左孩子或者右孩子不存在时 , 相应指针域值为空

下图为二叉树的链式存储结构 : 

![tree-08](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-08.png?raw=true)



## 二叉树实现

主流的还是以链式存储来实现

```python
class TreeNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

class BinaryTree:
    def __init__(self):
        self.root = None

    def insert(self, data):
        """插入一个新节点"""
        if not self.root:
            self.root = TreeNode(data)
        else:
            self._insert(self.root, data)
		# 递归插入
    def _insert(self, node, data):
        # 这里可以根据需要选择插入位置，这里简单地选择左子树或右子树
        if not node.left:
            node.left = TreeNode(data)
        elif not node.right:
            node.right = TreeNode(data)
        else:
            # 递归地插入到左子树或右子树
            self._insert(node.left, data)

    def delete(self, data):
        """删除一个节点"""
        self.root = self._delete(self.root, data)

    # 递归删除
    def _delete(self, node, data):
        if not node:
            return node
        if data < node.data:
            node.left = self._delete(node.left, data)
        elif data > node.data:
            node.right = self._delete(node.right, data)
        else:
            if not node.left:
                return node.right
            elif not node.right:
                return node.left

            # 找到右子树中的最小节点
            temp = node
            while temp.left:
                temp = temp.left

            node.data = temp.data
            node.right = self._delete(node.right, temp.data)
        return node

    def search(self, data):
        """查找一个节点"""
        return self._search(self.root, data)
      
		# 递归查找
    def _search(self, node, data):
        if not node or node.data == data:
            return node
        if data < node.data:
            return self._search(node.left, data)
        return self._search(node.right, data)
```

## 二叉树遍历

### 前序遍历

二叉树的前序遍历规则为 : 

- 如果二叉树为空 , 则返回
- 如果二叉树不为空 , 则 : 
  1. 访问根节点
  2. 以前序遍历的方式遍历根节点的左子树
  3. 以前序遍历的方式遍历根节点的右子树。

![tree-09](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-09.png?raw=true)

```python
def pre_order_traversal(self, node):
    """前序遍历"""
    if node:
        print(node.data, end=" -> ")
        self.pre_order_traversal(node.left)
        self.pre_order_traversal(node.right)
```

### 中序遍历

二叉树的中序遍历规则为 : 

- 如果二叉树为空 , 则返回
- 如果二叉树不为空 , 则 : 
  1. 以中序遍历的方式遍历根节点的左子树
  2. 访问根节点
  3. 以中序遍历的方式遍历根节点的右子树

![tree-10](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-10.png?raw=true)

```python
def in_order_traversal(self, node):
    """中序遍历"""
    if node:
        self.in_order_traversal(node.left)
        print(node.data, end=" -> ")
        self.in_order_traversal(node.right)
```

### 后序遍历

二叉树的后序遍历规则为：

- 如果二叉树为空 , 则返回
- 如果二叉树不为空 , 则 : 
  1. 以后序遍历的方式遍历根节点的左子树。
  2. 以后序遍历的方式遍历根节点的右子树
  3. 访问根节点。

![tree-11](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-11.png?raw=true)

```python
def post_order_traversal(self, node):
    """后序遍历"""
    if node:
        self.post_order_traversal(node.left)
        self.post_order_traversal(node.right)
        print(node.data, end=" -> ")
```

### 层序遍历

二叉树的层序遍历规则为 : 

- 如果二叉树为空 , 则返回
- 如果二叉树不为空 , 则 : 
  1. 先依次访问二叉树第 1 层的节点
  2. 然后依次访问二叉树第 2 层的节点
  3. ……
  4. 依次下去，最后依次访问二叉树最下面一层的节点

![tree-12](https://github.com/attack-on-backend/algorithm/blob/master/assert/tree-12.png?raw=true)

```python
def level_order_traversal(self):
    """层次遍历"""
    if not self.root:
        return

    queue = [self.root]
    while queue:
        node = queue.pop(0)
        print(node.data, end=" -> ")
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
```

## 例题

- [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)
- [112. 路径总和](https://leetcode.cn/problems/path-sum/)
- [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

