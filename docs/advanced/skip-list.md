# Attack on Algorithm - 跳表 🐝 

## 定义

**跳表 :** 又叫跳跃表 , 是一种类似于链表的数据结构 , 跳表是对有序链表的改进

跳表是为了减少链表长度增加 , 查找链表节点时带来的额外比较次数 , 不借助额外空间的情况下 , 在链表中查找一个值 , 需要按照顺序一个个查找 , 时间复杂度为 $O(N)$ , 其中 $N$ 为链表长度

跳表在有序链表的基础上 , 引入了**分层**的概念 ; 首先 , 跳表的每一层都是一个有序链表 , 而最底层则是初始的有序链表 , 如下图

![skip-list-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/skip-list-01.png?raw=true)

跳表通过继续增加索引的层数 , 建立二级、三级......索引 , 可以使得链表能够实现二分查找

在跳表中查找 , 就是从最顶层开始 , 水平地逐个比较直至当前节点的下一个节点大于等于目标节点 , 然后移动至下一层 , 重复这个过程直至到达第一层且无法继续进行操作 , 此时 , 若下一个节点是目标节点 , 则成功查找 ; 反之 , 则元素不存在 , 这样一来 , 查找的过程中会跳过一些没有必要的比较 , 所以相比于有序链表的查询 , 跳表的查询更快 , 跳表查询的平均时间复杂度为 $O(log n)$

![skip-list-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/skip-list-02.png?raw=true)

## 实现

```python
import random

class Node:
    def __init__(self, value, level):
        self.value = value
        self.forward = [None] * level

class SkipList:
    def __init__(self, max_level, p):
        self.max_level = max_level
        # 新节点晋升到上一层的概率
        self.p = p
        # 投节点
        self.header = Node(float('-inf'), max_level)
        # 当前跳表的层数
        self.level = 1

    def random_level(self):
        lvl = 1
        # 根据概率生成新节点的层数
        while random.random() < self.p and lvl < self.max_level:
            lvl += 1
        return lvl

    def insert(self, value):
        # update数组用于记录每一层要操作的节点
        update = [None] * self.max_level
        current = self.header
        # 从最高层开始，逐层向下查找插入位置
        for i in range(self.level - 1, -1, -1):
            while current.forward[i] and current.forward[i].value < value:
                current = current.forward[i]
            update[i] = current
        current = current.forward[0]
        if current is None or current.value != value:
            # 随机晋升
            rlevel = self.random_level()
            if rlevel > self.level:
                for i in range(self.level, rlevel):
                    update[i] = self.header
                self.level = rlevel
            new_node = Node(value, rlevel)
            for i in range(rlevel):
                new_node.forward[i] = update[i].forward[i]
                update[i].forward[i] = new_node

    def search(self, value):
        current = self.header
        # 从最高层开始，逐层向下查找目标值
        for i in range(self.level - 1, -1, -1):
            while current.forward[i] and current.forward[i].value < value:
                current = current.forward[i]
        current = current.forward[0]
        return current if current and current.value == value else None

    def delete(self, value):
        # update数组用于记录每一层要操作的节点
        update = [None] * self.max_level
        current = self.header
        # 从最高层开始，逐层向下查找目标值
        for i in range(self.level - 1, -1, -1):
            while current.forward[i] and current.forward[i].value < value:
                current = current.forward[i]
            update[i] = current
        current = current.forward[0]
        if current and current.value == value:
            for i in range(self.level):
                if update[i].forward[i] != current:
                    break
                update[i].forward[i] = current.forward[i]
            # 更新跳表的层
            while self.level > 1 and self.header.forward[self.level - 1] is None:
                self.level -= 1

    def display(self):
        # 打印每一层的节点
        for lvl in range(self.level - 1, -1, -1):
            print(f"Level {lvl}: ", end="")
            node = self.header.forward[lvl]
            while node:
                print(node.value, end=" -> ")
                node = node.forward[lvl]
            print("None")

if __name__ == "__main__":
    skip_list = SkipList(16, 0.5)
    skip_list.insert(3)
    skip_list.insert(6)
    skip_list.insert(7)
    skip_list.insert(9)
    skip_list.insert(12)
    skip_list.insert(19)
    skip_list.insert(17)
    skip_list.insert(26)
    skip_list.insert(21)
    skip_list.insert(25)

    print("原始跳表:")
    skip_list.display()

    print("\n查找元素 19:")
    node = skip_list.search(19)
    if node:
        print(f"找到元素: {node.value}")
    else:
        print("未找到元素")

    print("\n删除元素 19:")
    skip_list.delete(19)
    skip_list.display()
```

## 例题

- [1206. 设计跳表](https://leetcode.cn/problems/design-skiplist/)

