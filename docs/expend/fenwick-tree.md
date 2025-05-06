# Attack on Algorithm - 树状数组 🐝 

## 定义

**树状数组 (Binary Indexed Tree, 简称 BIT) :** 又称为 Fenwick Tree , 是一种用于高效处理前缀和查询和 单点更新操作的数据结构 ; 它可以以 $O(log n)$ 的时间得到任意前缀和 , 并同时支持在 $O(log n)$ 时间内支持动态单点值的修改 (增加或减少)

![fenwick-tree-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/fenwick-tree-01.png?raw=true)

树状数组名字虽然又有树 , 又有数组 , 但是它实际上物理形式还是数组 , 不过每个节点的含义是树的关系 , 如上图 , 树状数组中父子节点下标关系是 $parent=son+2^k$ , 其中 $k$ 是子节点下标对应二进制末尾 `0` 的个数

上图中 , A 和 B 都是数组 , A数组正常存储数据 , B数组是树状数组

树状数组使用场景 : 

- 前缀和问题
- 逆序对统计
- 频率统计

## 实现

```python
class FenwickTree:
    def __init__(self, size):
        self.n = size
        self.tree = [0] * (self.n + 1)

    def update(self, index, delta):
        # 将原数组 index 位置的元素增加 delta
        while index <= self.n:
            self.tree[index] += delta
            # 向上跳 lowbit(index)
            index += index & -index

    def query(self, index):
        # 查询前 index 项的和（包含 index）
        res = 0
        while index > 0:
            res += self.tree[index]
            # 向下跳 lowbit(index)
            index -= index & -index
        return res

    def range_query(self, left, right):
        # 查询区间 [left, right] 的和
        return self.query(right) - self.query(left - 1)

if __name__ == '__main__':
    # 初始数组长度为 5
    ft = FenwickTree(5)

    # 模拟原始数组 A = [1, 2, 3, 4, 5]
    for i in range(1, 6):
        ft.update(i, i)

    # 查询前缀和
    print(ft.query(3))  # 输出 1+2+3 = 6

    # 查询区间和
    print(ft.range_query(2, 4))  # 输出 2+3+4 = 9

    # 更新第三个元素 +2
    ft.update(3, 2)
    print(ft.range_query(1, 5))  # 输出 1+2+5+4+5 = 17
```

## 例题

- [1395. 统计作战单位数](https://leetcode.cn/problems/count-number-of-teams/)
- [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)
- [673. 最长递增子序列的个数](https://leetcode.cn/problems/number-of-longest-increasing-subsequence/)