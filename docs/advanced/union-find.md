# Attack on Algorithm - 并查集 🐝 

## 定义

**并查集** **:**  一种树型的数据结构 , 用于处理一些不交集 (Disjoint Sets) 的合并及查询问题 , 不交集指的是一系列没有重复元素的集合 ; 并查集具体点就是用来解决不交集的**查找(Find)**和**合并(Union)**问题

不交集指没有交集的集合 , 比如集合 `{1}` 和集合 `{2,3}` 的交集是空 , 它们就是两个不交集

## 实现

并查集有两种实现思路 : 

- 使用快速查询思路 , 基于数组结构实现的并查集
- 使用快速合并思路 , 基于森林实现的并查集

### 快速查询

我们可以使用一个数组结构来表示集合中的元素 , 数组元素和集合元素是一一对应的 , 我们可以将数组的索引值作为每个元素的集合编号 , 称为 $id$ , 然后可以对数组进行以下操作来实现并查集 : 

- **初始化 :** 将数组下标索引值作为每个元素的集合编号 , 所有元素的 $id$ 都是唯一的 , 代表着每个元素单独属于一个集合
- **合并操作 :** 需要将其中一个集合中的所有元素 $id$ 更改为另一个集合中的 $id$ , 能够保证在合并后一个集合中所有元素的 $id$ 均相同
- **查找操作 :**  如果两个元素的 $id$ 一样 , 则说明它们属于同一个集合 ; 如果两个元素的 $id$ 不一样 , 则说明它们不属于同一个集合

如下图 , 我们使用数组来表示一系列集合元素 `{0},{1},{2},{3},{4},{5},{6},{7}` , 数组的每个下标索引值对应一个元素的集合编号 , 代表着每个元素单独属于一个集合

![union-find-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-01.png?raw=true)

进行合并操作后 , 比如合并后变为 `{0},{1,2,3},{4},{5,6},{7}` , 如下图

![union-find-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-02.png?raw=true)

在快速查询的实现思路中 , 单次查询操作的时间复杂度是 $O(1)$ , 而单次合并操作的时间复杂度为 $O(n)$ (每次合并操作需要遍历数组) , 两者的时间复杂度相差得比较大 , 完全牺牲了合并操作的性能 , 因此 , 这种并查集的实现思路并不常用

代码实现如下

```python
class UnionFind:
    def __init__(self, n):
        # 将每个元素的集合编号初始化为数组下标索引
        self.ids = [i for i in range(n)]

    def find(self, x):
        return self.ids[x]

    def union(self, x, y):
        x_id = self.find(x)
        y_id = self.find(y)
        # x 和 y 已经同属于一个集合
        if x_id == y_id:
            return False
          
        # 将两个集合的集合编号改为一致
        for i in range(len(self.ids)): 
            if self.ids[i] == y_id:
                self.ids[i] = x_id
        return True
		# 判断 x 和 y 是否同属于一个集合
    def connected(self, x, y):
        return self.find(x) == self.find(y)
```

### 快速合并

我们可以使用一个森林 (若干棵树) 来存储所有集合 , 每一棵树代表一个集合 , 树上的每个节点都是一个元素 , 树根节点为这个集合的代表元素 ; 我们仍然可以使用一个数组 $fa$ 来记录这个森林 , 我们用 $fa[x]$ 来保存 $x$ 的父节点的集合编号 , 代表着元素节点 $x$ 指向父节点 $fa[x]$

同样的我们可以对数组 $fa$ 进行以下操作来实现并查集 : 

- **初始化 :**  将数组 $fa$ 的下标索引作为每个元素的集合编号 , 所有元素的根节点的集合编号都不一样 , 代表着每个元素单独属于一个集合
- **合并操作 :** 需要将两个集合的树根节点相连接 , 即令其中一个集合的树根节点指向另一个集合的树根节点($fa[root1] = root2$) , 这样合并后当前集合中的所有元素的树根节点均为同一个
- **查找操作 : ** 分别从两个元素开始 , 通过数组 $fa$ 存储的值 , 不断递归访问元素的父节点 , 直到到达树根节点 , 如果两个元素的树根节点一样 , 则说明它们属于同一个集合 ; 如果两个元素的树根节点不一样 , 则说明它们不属于同一个集合

如下图 , 我们使用数组来表示一系列集合元素 `{0},{1},{2},{3},{4},{5},{6},{7}`

![union-find-03](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-03.png?raw=true)

进行合并操作后 , 比如 `union(4, 5), union(6, 7), union(4, 7)` 操作后变为 `{0},{1},{2},{3},{4,5,6,7}` , 结果如下图

![union-find-04](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-04.png?raw=true)

`4` 的父节点指向`5` , `5` 的父节点指向 `7`  , `7` 为根节点

代码实现如下

```python
class UnionFind:
    def __init__(self, n):
        self.fa = [i for i in range(n)]

    def find(self, x):
        # 递归查找元素的父节点直到根节点
        while self.fa[x] != x:
            x = self.fa[x]
        # 返回元素根节点的集合编号
        return x                                    

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x == root_y:
            return False
        # x的根节点连接到y的根节点上
        self.fa[root_x] = root_y
        return True

    def connected(self, x, y):
        return self.find(x) == self.find(y)
```

## 路径压缩

在集合很大或者树很不平衡时 , 快速合并思路实现的并查集效率很差 , 最坏情况下树会退化成一条链 , 单次查询的时间复杂度为 $O(n)$ 

![union-find-05](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-05.png?raw=true)

为了避免出现最坏情况 , 常见的优化方式是路径压缩

**路径压缩 (Path Compression) :** 在从底向上查找根节点过程中 , 如果此时访问的节点不是根节点 , 则我们可以把这个节点尽量向上移动一下 , 从而减少树的层树 , 这个过程就叫做路径压缩

路径压缩有两种方式 : 隔代压缩和完全压缩

### 隔代压缩

**隔代压缩 :** 在查询时 , 两步一压缩 , 一直循环执行让当前节点指向它的父节点的父节点操作 , 从而减小树的深度

![union-find-06](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-06.png?raw=true)

```python
def find(self, x):
    while self.fa[x] != x:
        # 隔代压缩
        self.fa[x] = self.fa[self.fa[x]]
        x = self.fa[x]
    return x
```

### 完全压缩

**完全压缩 :** 在查询时 , 把被查询的节点到根节点的路径上的所有节点的父节点设置为根节点 , 从而减小树的深度

![union-find-07](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-07.png?raw=true)

```python
def find(self, x):
    if self.fa[x] != x:
        # 完全压缩
        self.fa[x] = self.find(self.fa[x])
    return self.fa[x]
```

## 按秩合并

因为路径压缩只在查询时进行 , 并且只压缩一棵树上的路径 , 所以并查集最终的结构仍然可能是比较复杂的 ; 还有合并应该是左向右合还是右向左合 , 为了避免这些情况 , 一个优化方式是按秩合并

**按秩合并 (Union By Rank) :** 指的是在每次合并操作时 , 都把秩较小的树根节点指向秩较大的树根节点

这里的秩有两种定义 , 一种定义是指节点的高度 , 另一种定义是指数的大小 (节点个数)

### 按深度合并

![union-find-08](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-08.png?raw=true)

```python
def union(self, x, y):
    root_x = self.find(x)
    root_y = self.find(y)
    if root_x == root_y:
        return False

    if self.rank[root_x] < self.rank[root_y]:
        # x根节点深度 < y根节点深度, x的根节点连接到y的根节点
        self.fa[root_x] = root_y 
    elif self.rank[root_y] > self.rank[root_y]:
        # x根节点深度 < y根节点深度, y的根节点连接到x的根节点
        self.fa[root_y] = root_x 
    else:
        # x根节点深度 = y根节点深度, 任意合并
        self.fa[root_x] = root_y
        # 因为层数相同，被合并的树必然层数会 +1
        self.rank[root_y] += 1  
    return True
```

### 按大小合并

![union-find-09](https://github.com/attack-on-backend/algorithm/blob/master/assert/union-find-09.png?raw=true)

```python
def union(self, x, y):
    root_x = self.find(x)
    root_y = self.find(y)
    if root_x == root_y:
        return False

    if self.size[root_x] < self.size[root_y]:
        # x节点数量 < y 节点数量, x 的根节点连接到 y 的根节点上
        self.fa[root_x] = root_y
        self.size[root_y] += self.size[root_x]
    elif self.size[root_x] > self.size[root_y]:
        # x节点数量 < y 节点数量, y 的根节点连接到 x 的根节点上
        self.fa[root_y] = root_x 
        self.size[root_x] += self.size[root_y]
    else:
        # x节点数量 = y 节点数量, 任意合并
        self.fa[root_x] = root_y
        self.size[root_y] += self.size[root_x]
    return True
```

## 例题

- [547. 朋友圈](https://leetcode-cn.com/problems/friend-circles/)
- [1319. 连通网络的操作次数](https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected/)
- [924. 尽量减少恶意软件的传播](https://leetcode-cn.com/problems/minimize-malware-spread/)

