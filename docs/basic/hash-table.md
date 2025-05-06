# Attack on Algorithm - 哈希表 🐝 

## 定义

**哈希表 :** 也叫做散列表 , 是根据关键码值 (`key - value`)直接进行访问的数据结构 , 哈希表通过键 `key`和映射函数`Hash(key)`计算出对应的值 `value` , 把关键码值映射到表中一个位置来访问记录 , 以加快查找的速度 , 这个映射函数叫做哈希函数 (散列函数) , 存放记录的数组叫做哈希表 (散列表) 

哈希表的关键思想是使用哈希函数将键映射到存储桶 : 

- 当我们插入一个新的键时 , 哈希函数将决定该键应该分配到哪个桶中 , 并将该键存储在相应的桶中
- 当我们想要搜索一个键时 , 哈希表将使用相同的哈希函数来查找对应的桶 , 并只在特定的桶中进行搜索

![hash-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/hash-01.png?raw=true)

## 实现

以下是一个简单的实现

```python
class HashTable:
    def __init__(self, size=10):
        # 初始化哈希表的大小和内部存储结构
        self.size = size
        self.table = [[] for _ in range(self.size)]

    def _hash(self, key):
        # 计算键的哈希值，并将其映射到表中的一个索引
        return hash(key) % self.size

    def insert(self, key, value):
        # 计算键的哈希值，得到存储位置的索引
        index = self._hash(key)
        # 检查该位置是否已经存在相同的键，如果存在则更新值
        for pair in self.table[index]:
            if pair[0] == key:
                pair[1] = value
                return
        # 如果键不存在，则将新的键值对添加到该位置
        self.table[index].append([key, value])

    def find(self, key):
        # 计算键的哈希值，得到存储位置的索引
        index = self._hash(key)
        # 遍历该位置的链表，查找键对应的值
        for pair in self.table[index]:
            if pair[0] == key:
                return pair[1]
        return None  # 如果键不存在，返回 None

    def delete(self, key):
        # 计算键的哈希值，得到存储位置的索引
        index = self._hash(key)
        # 遍历该位置的链表，查找并删除键值对
        for i, pair in enumerate(self.table[index]):
            if pair[0] == key:
                del self.table[index][i]
                return True
        return False  # 如果键不存在，返回 False

    def __str__(self):
        # 返回哈希表的字符串表示，方便调试和查看
        return str(self.table)

if __name__ == '__main__':
    
    # 示例用法
    hash_table = HashTable()
    hash_table.insert("apple", 1)
    hash_table.insert("banana", 2)
    hash_table.insert("orange", 3)
    
    print(hash_table)  # 输出: [[], [], [], [], [], [], [['orange', 3]], [], [], [['banana', 2], ['apple', 1]]]
    print(hash_table.find("apple"))  # 输出: 1
    print(hash_table.find("grape"))  # 输出: None
    
    hash_table.delete("banana")
    print(hash_table)  # 输出: [[], [], [], [], [], [], [['orange', 3]], [], [], [['apple', 1]]]
```

## 哈希函数

哈希函数一般会满足以下条件 : 

- 哈希函数应该易于计算 , 并且尽量使计算出来的索引值均匀分布
- 哈希函数计算得到的哈希值是一个固定长度的输出值
- 如果 $Hash(key1)≠Hash(key2)$ , 那么 key1、key2 一定不相等
- 如果 $Hash(key1)==Hash(key2)$ , 那么 key1、key2 可能相等 , 也可能不相等 (会发生哈希碰撞)

常见的哈希算法 : 

- [MD5](https://en.wikipedia.org/wiki/MD5)
- [SHA-1](https://en.wikipedia.org/wiki/SHA-1)
- [SHA-2](https://en.wikipedia.org/wiki/SHA-2)
- [NTLM](https://it.wikipedia.org/wiki/NTLM)

## 哈希冲突

**哈希冲突 :** 不同的关键字通过同一个哈希函数可能得到同一哈希地址 , 即 $key1≠key2$ , 而 $Hash(key1)==Hash(key2)$ , 这种现象称为哈希冲突

解决哈希冲突有两种方法 : *开放地址法 (Open Addressing) , 链地址法 (Separate Chaining)*

### 开发地址法

当发生冲突时 , 开放地址法按照下面的方法求得后继哈希地址 : $H(i)=(Hash(key)+F(i)) mod m$ , $i=1,2,3,...,n(n≤m−1)$

- $H(i)$是在处理冲突中得到的地址序列 , 即在第 1 次冲突 ($i=1$) 时经过处理得到一个新地址 $H(1)$ , 如果在 $H(1)$ 处仍然发生冲突 ($i=2$) 时经过处理时得到另一个新地址 $H(2)$…… 如此下去 , 直到求得的 $H(n)$ 不再发生冲突
- $Hash(key)$是哈希函数 , $m$ 是哈希表表长 , 对哈希表长取余的目的是为了使得到的下一个地址一定落在哈希表中
- $F(i)$ 是冲突解决方法 , 取法可以有以下几种 : 
  - 线性探测法 : $F(i)=1,2,3,...,m−1$
  - 二次探测法 : $F(i)=1^2,−1^2,2^2,−2^2,...,±n^2(n≤m/2)$
  - 伪随机数序列 : $F(i)=伪随机数序列$

![hash-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/hash-02.png?raw=true)

### 链地址法

**链地址法 :** 将具有相同哈希地址的元素 (或记录) 存储在同一个线性链表中

链地址法是一种更加常用的哈希冲突解决方法 , 相比于开放地址法 , 链地址法更加简单

我们假设哈希函数产生的哈希地址区间为` [0, m−1]` , 哈希表的表长为 $m$ , 则可以将哈希表定义为一个有 $m$ 个头节点组成的链表指针数组 $T$

- 这样在插入关键字的时候 , 我们只需要通过哈希函数 $Hash(key)$ 计算出对应的哈希地址 $i$  , 然后将其以链表节点的形式插入到以 $T[i]$ 为头节点的单链表中 , 在链表中插入位置可以在表头或表尾 , 也可以在中间 , 如果每次插入位置为表头 , 则插入操作的时间复杂度为 $O(1)$
- 而在在查询关键字的时候 , 我们只需要通过哈希函数 $Hash(key)$ 计算出对应的哈希地址 $i$  , 然后将对应位置上的链表整个扫描一遍 , 比较链表中每个链节点的键值与查询的键值是否一致 , 查询操作的时间复杂度跟链表的长度 $k$ 成正比 , 也就是 $O(k)$ 对于哈希地址比较均匀的哈希函数来说 , 理论上讲 , $k=n//m$ , 其中 $n$ 为关键字的个数 , $m$ 为哈希表的表长

![hash-03](https://github.com/attack-on-backend/algorithm/blob/master/assert/hash-03.png?raw=true)

## 例题

- [1. 两数之和](https://leetcode.cn/problems/two-sum/)
- [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)
- [12. 整数转罗马数字](https://leetcode.cn/problems/integer-to-roman/)



