# Attack on Algorithm - 线段树 🐝 

## 定义

**线段树 (Segment Tree) : **一种基于分治思想的二叉树 , 用于在区间上进行信息统计 , 它的每一个节点都对应一个区间 `[left, right]` , left、right 通常是整数 , 每一个叶子节点表示了一个单位区间 (长度为 1) , 叶子节点对应区间上 `left==right` , 每一个非叶子节点 `[left, right]` 的左子节点表示的区间都为 `[left, (left+right)/2]` , 右子节点表示的的区间都为`[(left+right)/2+1,right]`

线段树是一棵平衡二叉树 , 树上的每个节点维护一个区间 , 根节点维护的是整个区间 , 每个节点维护的是父亲节点的区间二等分之后的其中一个子区间 , 当有 `n` 个元素时 , 对区间的操作 (单点更新、区间更新、区间查询等) 可以在 $O(log⁡2^n)$ 的时间复杂度内完成

![segment-tree-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/segment-tree-01.png?raw=true)

线段树的使用场景 : 

- 区间最值查询 , 查询某个区间内的最小值或最大值 (RMQ问题 , Range Minimum Query)
- 区间求和 , 查询某个区间的元素之和 , 并支持单点或区间更新
- 频率统计与离散化结合 , 如统计区间中某个值出现的次数问题
- 扫描线问题 , 虚拟扫描线或扫描面来解决欧几里德空间中的各种问题 , 一般被用来解决图形面积 , 周长等问题

## 实现

### 构建

由于线段树近乎是完全二叉树 , 所以很适合用顺序存储结构来实现。

构建规律如下图

![segment-tree-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/segment-tree-02.png?raw=true)

代码实现

```python
# 线段树的节点类
class TreeNode:
    def __init__(self, val=0):
        # 区间左边界
        self.left = -1
        # 区间右边界
        self.right = -1
        # 区间值
        self.val = val
        # 区间和问题的延迟更新标记
        self.lazy_tag = None

# 线段树类
class SegmentTree:
    def __init__(self, nums, function):
        self.size = len(nums)
        # 维护TreeNode数组
        self.tree = [TreeNode() for _ in range(4 * self.size)]
        # 原始数据
        self.nums = nums
        # function是一个函数, 左右区间的聚合方法
        self.function = function
        if self.size > 0:
            self._build(0, 0, self.size - 1)

    # 构建线段树, 节点的存储下标为index, 节点的区间为[left, right]
    def _build(self, index, left, right):
        self.tree[index].left = left
        self.tree[index].right = right
        # 叶子节点, 节点值为对应位置的元素值
        if left == right:
            self.tree[index].val = self.nums[left]
            return
        # 左右节点划分点
        mid = left + (right - left) // 2
        # 左子节点的存储下标
        left_index = index * 2 + 1
        # 右子节点的存储下标
        right_index = index * 2 + 2
        # 递归创建左子树
        self._build(left_index, left, mid)
        # 递归创建右子树
        self._build(right_index, mid + 1, right)
        # 向上更新节点的区间值
        self.push_up(index)

    # 向上更新下标为index的节点区间值, 节点的区间值等于该节点左右子节点元素值的聚合计算结果
    def push_up(self, index):
        # 左子节点的存储下标
        left_index = index * 2 + 1
        # 右子节点的存储下标
        right_index = index * 2 + 2
        self.tree[index].val = self.function(self.tree[left_index].val, self.tree[right_index].val)

    # 向下更新下标为index的节点所在区间的左右子节点的值和懒惰标记
    def push_down(self, index):
        lazy_tag = self.tree[index].lazy_tag
        if not lazy_tag:
            return
        # 左子节点的存储下标
        left_index = index * 2 + 1
        # 右子节点的存储下标
        right_index = index * 2 + 2
        # 更新左子节点懒惰标记
        self.tree[left_index].lazy_tag = lazy_tag
        left_size = (self.tree[left_index].right - self.tree[left_index].left + 1)
        # 更新左子节点值
        self.tree[left_index].val = lazy_tag * left_size
        # 更新右子节点懒惰标记
        self.tree[right_index].lazy_tag = lazy_tag
        right_size = (self.tree[right_index].right - self.tree[right_index].left + 1)
        # 更新右子节点值
        self.tree[right_index].val = lazy_tag * right_size
        # 更新当前节点的懒惰标记
        self.tree[index].lazy_tag = None
```

### 单点更新

修改一个元素的值 , 例如将 `nums[i]` 修改为 `val`

更新步骤 : 

1. 如果是叶子节点 , 满足 `left==right` , 则更新该节点的值
2. 如果是非叶子节点 , 则判断应该在左子树中更新 , 还是应该在右子树中更新
3. 在对应的左子树或右子树中更新节点值
4. 左右子树更新返回之后 , 向上更新节点的区间值 (区间和、区间最大值、区间最小值等) , 区间值等于该节点左右子节点元素值的聚合计算结果

代码实现

```python
    # 单点更新, 将nums[i]更改为val
    def update_point(self, i, val):
      self.nums[i] = val
      self._update_point(i, val, 0, 0, self.size - 1)

    # 单点更新, 将nums[i]更改为val, 节点的存储下标为index, 节点的区间为[left, right]
    def _update_point(self, i, val, index, left, right):
        if self.tree[index].left == self.tree[index].right:
            # 叶子节点, 节点值修改为val
            self.tree[index].val = val
            return
        # 左右节点划分点
        mid = left + (right - left) // 2
        # 左子节点的存储下标
        left_index = index * 2 + 1
        # 右子节点的存储下标
        right_index = index * 2 + 2

        if i <= mid:
            # 在左子树中更新节点值
            self._update_point(i, val, left_index, left, mid)
        else:
            # 在右子树中更新节点值
            self._update_point(i, val, right_index, mid + 1, right)
        # 向上更新节点的区间值
        self.push_up(index)
```

### 区间查询

```python
    # 区间查询, 查询区间为[q_left, q_right]的区间值
    def query_interval(self, q_left, q_right):
        return self._query_interval(q_left, q_right, 0, 0, self.size - 1)

    # 区间查询, 在线段树的[left, right]区间范围中搜索区间为[q_left, q_right]的区间值
    def _query_interval(self, q_left, q_right, index, left, right):
        # 节点所在区间被[q_left, q_right]所覆盖
        if left >= q_left and right <= q_right:
            # 直接返回节点值
            return self.tree[index].val
        # 节点所在区间与[q_left, q_right]无关
        if right < q_left or left > q_right:
            return 0

        self.push_down(index)
        # 左右节点划分点
        mid = left + (right - left) // 2
        # 左子节点的存储下标
        left_index = index * 2 + 1
        # 右子节点的存储下标
        right_index = index * 2 + 2
        # 左子树查询结果
        res_left = 0
        # 右子树查询结果
        res_right = 0
        # 在左子树中查询
        if q_left <= mid:
            res_left = self._query_interval(q_left, q_right, left_index, left, mid)
        # 在右子树中查询
        if q_right > mid:
            res_right = self._query_interval(q_left, q_right, right_index, mid + 1, right)
        # 返回左右子树元素值的聚合计算结果
        return self.function(res_left, res_right)
```

### 区间更新

```python
    # 区间更新, 将区间为[q_left, q_right]上的元素值修改为val
    def update_interval(self, q_left, q_right, val):
        self._update_interval(q_left, q_right, val, 0, 0, self.size - 1)

    # 区间更新
    def _update_interval(self, q_left, q_right, val, index, left, right):
        # 节点所在区间被[q_left, q_right]所覆盖
        if left >= q_left and right <= q_right:
            # 当前节点所在区间大小
            interval_size = (right - left + 1)
            # 当前节点所在区间每个元素值改为 val
            self.tree[index].val = interval_size * val
            # 将当前节点的延迟标记为区间值
            self.tree[index].lazy_tag = val
            return
        # 节点所在区间与[q_left, q_right]无关
        if right < q_left or left > q_right:
            return 0

        self.push_down(index)
        # 左右节点划分点
        mid = left + (right - left) // 2
        # 左子节点的存储下标
        left_index = index * 2 + 1
        # 右子节点的存储下标
        right_index = index * 2 + 2
        # 在左子树中更新区间值
        if q_left <= mid:
            self._update_interval(q_left, q_right, val, left_index, left, mid)
        # 在右子树中更新区间值
        if q_right > mid:
            self._update_interval(q_left, q_right, val, right_index, mid + 1, right)

        self.push_up(index)
```

## 例题

- [303. 区域和检索 - 数组不可变](https://leetcode.cn/problems/range-sum-query-immutable/)
- [307. 区域和检索 - 数组可修改](https://leetcode.cn/problems/range-sum-query-mutable/)
- [729. 我的日程安排表 I](https://leetcode.cn/problems/my-calendar-i/)

