# Attack on Algorithm - 优先队列 🐝 

## 定义

**优先队列 (Priority Queue) :** 一种特殊的队列 , 在优先队列中 , 元素被赋予优先级 , 当访问队列元素时 , 具有最高优先级的元素最先出队

优先队列与普通队列最大的不同点在于出队顺序 : 

- 普通队列的出队顺序跟入队顺序相关 , 符合先进先出 (First in, First out) 的规则
- 优先队列的出队顺序跟入队顺序无关 , 优先队列是按照元素的优先级来决定出队顺序的 , 优先级高的元素优先出队 , 优先级低的元素后出队 , 优先队列符合 最高级先出 (First in, Largest out) 的规则

![priority-queue-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/priority-queue-01.png?raw=true)

优先队列的应用场景非常多 , 比如 : 

- 数据压缩 : 赫夫曼编码算法
- 最短路径算法 : `Dijkstra` 算法
- 最小生成树算法 : `Prim` 算法
- 任务调度器 : 根据优先级执行系统任务
- 事件驱动仿真 : 顾客排队算法
- 排序问题 : 查找第 `k` 个最小元素

## 实现

优先队列的实现方式有很多种 , 不过最常用的是使用堆实现优先队列

最小堆 : 

- 父节点的值小于或等于其子节点的值
- 插入和删除操作的时间复杂度为 $O(log n)$
- 查找最小元素的时间复杂度为 $O(1)$ 

最大堆

- 父节点的值大于或等于其子节点的值。
- 插入和删除操作的时间复杂度为 $O(log n)$
- 查找最大元素的时间复杂度为 $O(1)$

```python
import heapq
# 最小堆实现
class MinPriorityQueue:
    def __init__(self):
        self.heap = []

    def insert(self, element, priority):
        heapq.heappush(self.heap, (priority, element))

    def extract_min(self):
        if not self.heap:
            raise IndexError("extract_min from an empty priority queue")
        priority, element = heapq.heappop(self.heap)
        return element

    def peek_min(self):
        if not self.heap:
            raise IndexError("peek_min from an empty priority queue")
        priority, element = self.heap[0]
        return element

    def is_empty(self):
        return len(self.heap) == 0

    def __len__(self):
        return len(self.heap)

    def __str__(self):
        return "MinPriorityQueue([" + ", ".join(f"({priority}, {element})" for priority, element in self.heap) + "])"

# 示例代码
if __name__ == "__main__":
    pq = MinPriorityQueue()

    # 插入元素及其优先级
    pq.insert("Task 1", 3)
    pq.insert("Task 2", 1)
    pq.insert("Task 3", 2)
    print("插入元素后的优先队列:", pq)  # 输出: 插入元素后的优先队列: MinPriorityQueue([(1, Task 2), (3, Task 1), (2, Task 3)])

    # 查找最小优先级元素
    print("最小优先级元素:", pq.peek_min())  # 输出: 最小优先级元素: Task 2

    # 删除最小优先级元素
    min_element = pq.extract_min()
    print("删除最小优先级元素:", min_element)  # 输出: 删除最小优先级元素: Task 2
    print("删除后的优先队列:", pq)  # 输出: 删除后的优先队列: MinPriorityQueue([(2, Task 3), (3, Task 1)])

    # 检查队列是否为空
    print("队列是否为空:", pq.is_empty())  # 输出: 队列是否为空: False

    # 获取队列大小
    print("队列大小:", len(pq))  # 输出: 队列大小: 2
```

## 例题

- [703. 数据流中的第 K 大元素](https://leetcode.cn/problems/kth-largest-element-in-a-stream/)
- [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)
- [451. 根据字符出现频率排序](https://leetcode.cn/problems/sort-characters-by-frequency/)