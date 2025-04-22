# Attack on Algorithm - 队列 🐝 

## 定义

**队列 :** 一种线性表数据结构 , 是一种只允许在表的一端进行插入操作 , 而在表的另一端进行删除操作的线性表

队列是典型的 `FIFO(First In, First Out)` 数据结构

我们把队列中允许插入的一端称为**队尾** , 把允许删除的另一端称为**队头** , 当队列中没有任何数据时 , 称为**空队**

![queue-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/queue-01.png?raw=true)

队列的插入操作也称作**入队** , 新元素始终被添加在队列的末尾 ,  删除操作也被称为**出队** , 你只能移除第一个元素

![queue-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/queue-02.gif?raw=true)

队列的基本操作 : 

- `enqueue()` ：将元素添加到队列的末尾
- `dequeue()` ：移除并返回队列的第一个元素
- `peek()` ：返回队列的第一个元素但不移除它
- `size()` ：返回队列中元素的数量
- `is_empty()` ：检查队列是否为空

## 实现

队列的实现有两种方式 , 基于两种线性表的实现 : 数组和链表

以下是数组的实现方式

```python
class Queue:
    def __init__(self):
        self.items = []

    def is_empty(self):
        return len(self.items) == 0

    def enqueue(self, item):
        self.items.append(item)

    def dequeue(self):
        if self.is_empty():
            raise IndexError("dequeue from empty queue")
        return self.items.pop(0)

    def peek(self):
        if self.is_empty():
            raise IndexError("peek from empty queue")
        return self.items[0]

    def size(self):
        return len(self.items)

    def __str__(self):
        return str(self.items)
```

## 例题

- [622. 设计循环队列](https://leetcode.cn/problems/design-circular-queue/)
- [2073. 买票需要的时间](https://leetcode.cn/problems/time-needed-to-buy-tickets/)
- [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

