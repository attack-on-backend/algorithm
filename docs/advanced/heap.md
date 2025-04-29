# Attack on Algorithm - 堆 🐝 

## 定义

**堆 (Heap) :** 堆是一种特殊的完全二叉树 , 堆中的每一个节点的值都必须大于等于或者小于等于其孩子节点的值 , 以此可以分为大项堆和小项堆

**大项堆 :** 任意节点值 >= 其子节点值

**小项堆 :** 任意节点值 <= 其子节点值

![heap-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/heap-01.png?raw=true)

## 实现

对于完全二叉树 , 使用数组来存储能够充分利用存储空间

![heap-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/heap-02.png?raw=true)

当我们使用数组来表示堆时 , 堆中元素的节点编号与数组的索引关系为 : 

- 如果某二叉树节点 (非叶子节点) 的下标为 $i$ , 那么其左孩子节点下标为 $2×i+1$ , 右孩子节点下标为 $2×i+2$
- 如果某二叉树节点 (非根结点) 的下标为 $i$ , 那么其根节点下标为 $\frac{i - 1}{2}$ (向下取整) 

```python
# 小项堆
class MinHeap:
    def __init__(self):
        self.heap = []

    # 插入元素
    def insert(self, value):
        self.heap.append(value)
        self._sift_up(len(self.heap) - 1)

    # 删除根节点
    def delete(self):
        if not self.heap:
            return None
        min_val = self.heap[0]
        last_val = self.heap.pop()
        if self.heap:
            self.heap[0] = last_val
            self._sift_down(0)
        return min_val

    # 查找最小值（根节点）
    def get_min(self):
        if not self.heap:
            return None
        return self.heap[0]

    # 上浮操作
    def _sift_up(self, index):
        while index > 0:
            parent_index = (index - 1) // 2
            if self.heap[parent_index] <= self.heap[index]:
                break
            self.heap[parent_index], self.heap[index] = self.heap[index], self.heap[parent_index]
            index = parent_index

    # 下沉操作
    def _sift_down(self, index):
        size = len(self.heap)
        while True:
            left = 2 * index + 1
            right = 2 * index + 2
            smallest = index
            if left < size and self.heap[left] < self.heap[smallest]:
                smallest = left
            if right < size and self.heap[right] < self.heap[smallest]:
                smallest = right
            if smallest == index:
                break
            self.heap[index], self.heap[smallest] = self.heap[smallest], self.heap[index]
            index = smallest

    # 构建堆, 从无序数组构建
    def build_heap(self, array):
        self.heap = array[:]
        for i in range(len(self.heap) // 2 - 1, -1, -1):
            self._sift_down(i)

    # 打印堆
    def print_heap(self):
        print(self.heap)

# 示例操作
if __name__ == "__main__":
    # 创建一个空的小顶堆
    min_heap = MinHeap()

    # 插入元素
    min_heap.insert(10)
    min_heap.insert(5)
    min_heap.insert(3)
    min_heap.insert(2)
    min_heap.insert(7)
    print("插入元素后的堆:")
    min_heap.print_heap()  # 输出: [2, 5, 3, 10, 7]

    # 查找最小值
    print("最小值:", min_heap.get_min())  # 输出: 2

    # 删除最小值
    print("删除最小值后的堆:")
    min_heap.delete()
    min_heap.print_heap()  # 输出: [3, 5, 7, 10]

    # 构建堆
    array = [9, 5, 6, 2, 3]
    min_heap.build_heap(array)
    print("构建堆后的结果:")
    min_heap.print_heap()  # 输出: [2, 3, 6, 9, 5]
```

上浮逻辑 : 

1. 将新元素添加到堆的末尾 , 保持完全二叉树的结构
2. 从新插入的元素节点开始 , 将该节点与其父节点进行比较
   1. 如果新节点的值大于其父节点的值 , 则交换它们 , 以保持最大堆的特性
   2. 如果新节点的值小于等于其父节点的值 , 说明已满足最大堆的特性 , 此时结束
3. 重复上述比较和交换步骤 , 直到新节点不再大于其父节点 , 或者达到了堆的根节点

下沉逻辑 : 

1. 将堆顶元素 (即根节点) 与堆的末尾元素交换
2. 移除堆末尾的元素 (之前的堆顶) , 即将其从堆中剔除
3. 从新的堆顶元素开始 , 将其与其较大的子节点进行比较
   1. 如果当前节点的值小于其较大的子节点 , 则将它们交换 , 这一步是为了将新的堆顶元素下沉到适当的位置 , 以保持最大堆的特性
   2. 如果当前节点的值大于等于其较大的子节点 , 说明已满足最大堆的特性 , 此时结束
4. 重复上述比较和交换步骤 , 直到新的堆顶元素不再小于其子节点 , 或者达到了堆的底部

## 例题

- [2558. 从数量最多的堆取走礼物](https://leetcode.cn/problems/take-gifts-from-the-richest-pile/)
- [912. 排序数组](https://leetcode.cn/problems/sort-an-array/)
- [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

