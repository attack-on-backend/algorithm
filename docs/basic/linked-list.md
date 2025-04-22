# Attack on Algorithm - 链表 🐝 

## 定义

**链表 :** 一种线性表数据结构 , 它使用一组任意的存储单元 (可以是连续的 , 也可以是不连续的)  , 来存储一组具有相同类型的数据

链表是实现线性表链式存储结构的基础

### 单链表

单链表中的每个结点不仅包含值 , 还包含链接到下一个结点的`引用字段` , 通过这种方式 , 单链表将所有结点按顺序组织起来

下面是一个单链表的例子 , 蓝色箭头显示单个链接列表中的结点是如何组合在一起的

![linked-list-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/linked-list-01.png?raw=true)

### 双链表

双链表以类似的方式工作 , 但`还有一个引用字段` , 称为`prev`字段。有了这个额外的字段 , 你就能够知道当前结点的前一个结点

下面是一个双链表的例子 , 绿色箭头表示我们的`prev`字段是如何工作的

![linked-list-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/linked-list-02.png?raw=true)

## 实现

### 单链表

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.size = 0
        self.head = None
        self.last = None

    # 查找节点
    def find(self, index):
        if index < 0 or index >= self.size:
            raise Exception("超出链表节点范围!")
        p = self.head
        for i in range(index):
            p = p.next
        return p

    # 插入节点
    def insert(self, data, index):
        if index < 0 or index > self.size:
            raise Exception("超出链表节点范围！")
        node = Node(data)
        if self.size == 0:
            # 空链表
            self.head = node
            self.last = node
        elif index == 0:
            # 插入头部
            node.next = self.head
            self.head = node
        elif self.size == index:
            # 插入尾部
            self.last.next = node
            self.last = node
        else:
            # 插入中间
            prev_node = self.find(index - 1)
            node.next = prev_node.next
            prev_node.next = node
        self.size += 1

    # 删除节点
    def remove(self, index):
        if index < 0 or index > self.size:
            raise Exception("超出链表节点范围！")
        if index == 0:
            # 删除头节点
            removed_node = self.head
            self.head = self.head.next
        elif index == self.size - 1:
            # 删除尾结点
            prev_node = self.find(index - 1)
            removed_node = prev_node.next
            prev_node.next = None
            self.last = prev_node
        else:
            # 删除中间节点
            prev_node = self.find(index - 1)
            next_node = prev_node.next.next
            removed_node = prev_node.next
            prev_node.next = next_node
        self.size -= 1
        return removed_node
		# 遍历链表
    def list(self):
        p = self.head
        while p is not None:
            print(p.data)
            p = p.next
```

### 双链表

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None

class LinkedList:
    def __init__(self):
        self.size = 0
        self.head = None
        self.last = None

    # 查找节点
    def find(self, index):
        if index < 0 or index >= self.size:
            raise Exception("超出链表节点范围!")
        p = self.head
        for i in range(index):
            p = p.next
        return p

    # 插入节点
    def insert(self, data, index):
        if index < 0 or index > self.size:
            raise Exception("超出链表节点范围！")
        node = Node(data)
        if self.size == 0:
            # 空链表
            self.head = node
            self.last = node
        elif index == 0:
            # 插入头部
            node.next = self.head
            self.head.prev = node
            self.head = node
        elif self.size == index:
            # 插入尾部
            self.last.next = node
            node.prev = self.last
            self.last = node
        else:
            # 插入中间
            prev_node = self.find(index - 1)
            node.next = prev_node.next
            node.prev = prev_node
            prev_node.next = node
        self.size += 1
    # 删除节点
    def remove(self, index):
        if index < 0 or index > self.size:
            raise Exception("超出链表节点范围！")
        if index == 0:
            # 删除头节点
            removed_node = self.head
            self.head.prev = None
            self.head = self.head.next
        elif index == self.size - 1:
            # 删除尾结点
            prev_node = self.find(index - 1)
            removed_node = prev_node.next
            prev_node.next = None
            self.last = prev_node
        else:
            # 删除中间节点
            prev_node = self.find(index - 1)
            next_node = prev_node.next.next
            removed_node = prev_node.next
            next_node.prev = prev_node
            prev_node.next = next_node
        self.size -= 1
        return removed_node
		# 遍历链表
    def list(self):
        p = self.head
        while p is not None:
            print(p.data)
            p = p.next
```

## 例题

- [707. 设计链表](https://leetcode.cn/problems/design-linked-list/)
- [LCR 123. 图书整理 I](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)
- [237. 删除链表中的节点](https://leetcode.cn/problems/delete-node-in-a-linked-list/)



