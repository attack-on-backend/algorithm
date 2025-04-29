# Attack on Algorithm - 双端队列 🐝 

## 定义

**双端队列 :** 是一种可以在两端进行插入和删除操作的特殊队列

双端队列主要有以下使用场景 : 

- 广度优先搜索 (BFS) : 优化 BFS 算法 , 如双向 BFS
- 滑动窗口问题 : 高效维护滑动窗口中的元素 , 如求滑动窗口的最大值或最小值
- 括号匹配 : 从两端逐个比较字符 , 确保括号的正确匹配
- 缓存淘汰策略 : 实现 LRU 缓存淘汰策略 , 管理最近使用的元素
- 回文检查 : 从两端逐个比较字符 , 确保字符串的对称性

## 实现

双端队列更多的是使用链表来实现 , 在扩容的情况数组实现效率更低下

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.prev = None
        self.next = None

class Deque:
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0

    def append(self, value):
        new_node = Node(value)
        if not self.head:
            self.head = self.tail = new_node
        else:
            self.tail.next = new_node
            new_node.prev = self.tail
            self.tail = new_node
        self.size += 1

    def appendleft(self, value):
        new_node = Node(value)
        if not self.head:
            self.head = self.tail = new_node
        else:
            self.head.prev = new_node
            new_node.next = self.head
            self.head = new_node
        self.size += 1

    def pop(self):
        if not self.tail:
            raise IndexError("pop from an empty deque")
        value = self.tail.value
        if self.head == self.tail:
            self.head = self.tail = None
        else:
            self.tail = self.tail.prev
            self.tail.next = None
        self.size -= 1
        return value

    def popleft(self):
        if not self.head:
            raise IndexError("popleft from an empty deque")
        value = self.head.value
        if self.head == self.tail:
            self.head = self.tail = None
        else:
            self.head = self.head.next
            self.head.prev = None
        self.size -= 1
        return value

    def peek(self):
        if not self.tail:
            raise IndexError("peek from an empty deque")
        return self.tail.value

    def peekleft(self):
        if not self.head:
            raise IndexError("peekleft from an empty deque")
        return self.head.value

    def is_empty(self):
        return self.size == 0

    def __len__(self):
        return self.size

    def __str__(self):
        values = []
        current = self.head
        while current:
            values.append(str(current.value))
            current = current.next
        return "Deque([" + ", ".join(values) + "])"


if __name__ == '__main__':
    # 创建一个空的双端队列
    d = Deque()

    # 在尾部插入元素
    d.append(1)
    d.append(2)
    d.append(3)
    print("插入尾部后的队列:", d)  # 输出: Deque([1, 2, 3])
    # 在头部插入元素
    d.appendleft(0)
    print("插入头部后的队列:", d)  # 输出: Deque([0, 1, 2, 3])
    # 从尾部删除元素
    last_element = d.pop()
    print("删除尾部元素:", last_element)  # 输出: 3
    print("删除尾部后的队列:", d)  # 输出: Deque([0, 1, 2])
    # 从头部删除元素
    first_element = d.popleft()
    print("删除头部元素:", first_element)  # 输出: 0
    print("删除头部后的队列:", d)  # 输出: Deque([1, 2])
    # 访问头部元素
    print("头部元素:", d.peekleft())  # 输出: 1
    # 访问尾部元素
    print("尾部元素:", d.peek())  # 输出: 2
```

## 例题

- [641. 设计循环双端队列](https://leetcode.cn/problems/design-circular-deque/)
- [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

