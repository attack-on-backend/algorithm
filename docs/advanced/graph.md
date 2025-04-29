# Attack on Algorithm - 图 🐝 

## 定义

**图 (Graph) :** 由顶点的非空有限集合 $V$ (由 $n>0$ 个顶点组成) 与边的集合 $E$ (顶点之间的关系) 构成的结构 , 其形式化定义为 $G=(V,E)$ 

- **顶点 (Vertex) :** 图中的数据元素通常称为顶点 , 在下面的示意图中我们使用圆圈来表示顶点
- **边 (Edge) : **图中两个数据元素之间的关联关系通常称为边 , 在下面的示意图中我们使用连接两个顶点之间的线段来表示边 , 边的形式化定义为 : $e=⟨u,v⟩$ , 表示从 $u$ 到 $v$ 的一条边 , 其中 $u$ 称为起始点 , $v$ 称为终止点

![graph-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/graph-01.png?raw=true)

## 分类

按照边是否有方向 , 我们可以将图分为无向图和有向图

- **无向图 (Undirected Graph) :** 如果图中的每条边都没有指向性 , 则称为无向图 , 例如朋友关系图、路线图都是无向图
- **有向图 (Directed Graph) :** 如果图中的每条边都具有指向性 , 则称为有向图 , 例如流程图是有向图

![graph-02](https://github.com/attack-on-backend/algorithm/blob/master/assert/graph-02.png?raw=true)

而根据图中是否有环 , 我们可以将图分为环形图和无环图

- **环形图 (Circular Graph) :** 如果图中存在至少一条环路 , 则该图称为环形图
- **无环图 (Acyclic Graph) :** 如果图中不存在环路 , 则该图称为无环图

![graph-03](https://github.com/attack-on-backend/algorithm/blob/master/assert/graph-03.png?raw=true)

## 实现

图的实现主要有以下几种方法 : 

- 邻接矩阵 : 使用二维数组表示节点之间的连接关系
- 邻接表 : 使用列表或哈希表存储每个节点及其相邻节点
- 边集数组 : 用一个数组存储所有边 , 每条边用两个节点 (或三个节点加权重) 表示
- 链式前向星 : 静态邻接表 , 实质上就是使用静态链表实现的邻接表 , 链式前向星将边集数组和邻接表相结合，可以快速访问一个节点所有的邻接点，并且使用很少的额外空间
- 十字链表 : 一种同时支持有向图和无向图的存储方式，结合了邻接表和逆邻接表

| 方法       | 空间复杂度 | 查询效率       | 修改效率 | 使用场景                 |
| ---------- | ---------- | -------------- | -------- | ------------------------ |
| 邻接矩阵   | $O(V^2)$   | $O(1)$         | 较低     | 稠密图、频繁查询连接关系 |
| 邻接表     | $O(V + E)$ | $O(degree(V))$ | 较高     | 稀疏图、遍历邻居节点     |
| 边集数组   | $O(E)$     | 较低           | 较高     | 排序边、最小生成树问题   |
| 链式前向星 | $O(V + E)$ | 较高           | 较低     | 静态图、高效遍历         |
| 十字链表   | $O(V + E)$ | 较高           | 较高     | 同时处理出边和入边       |

以下主要介绍邻接矩阵、邻接表、边集数组这三种实现方式

### 邻接矩阵

**邻接矩阵 (Adjacency Matrix) :** 使用一个二维数组 `adj_matrix` 来存储顶点之间的邻接关系

- 对于无权图来说 , 如果 `adj_matrix[i][j]` 为 `1` , 则说明顶点 $vi$ 到 $vj$ 存在边 , 如果 `adj_matrix[i][j]` 为 `0`  , 则说明顶点 $vi$ 到 $vj$ 不存在边
- 对于带权图来说 , 如果 `adj_matrix[i][j]` 为 $w$ , 并且 $w≠∞$ (即 `w != float('inf')`) , 则说明顶点 $vi$ 到 $vj$ 的权值为 $w$ , 如果 `adj_matrix[i][j]` 为 $∞$ (即 `float('inf')`) ,  则说明顶点 $vi$ 到 $vj$ 不存在边

![graph-04](https://github.com/attack-on-backend/algorithm/blob/master/assert/graph-04.png?raw=true)

代码实现

```python
class Graph:
    def __init__(self, ver_count):
        # 顶点个数
        self.ver_count = ver_count
        # 邻接矩阵
        self.adj_matrix = [[None for _ in range(ver_count)] for _ in range(ver_count)]

    # 判断顶点 v 是否有效
    def _valid(self, v):
        return 0 <= v <= self.ver_count

    # 创建图
    def create(self, edges=None):
        if edges is None:
            edges = []
        for vi, vj, val in edges:
            self.add_edge(vi, vj, val)

    # 添加边vi - vj, 权值为 val
    def add_edge(self, vi, vj, val):
        if not self._valid(vi) or not self._valid(vj):
            raise ValueError(str(vi) + ' or ' + str(vj) + " is not a valid vertex.")

        self.adj_matrix[vi][vj] = val

    # 获取 vi - vj 边的权值
    def get_edge(self, vi, vj):
        if not self._valid(vi) or not self._valid(vj):
            raise ValueError(str(vi) + ' or ' + str(vj) + " is not a valid vertex.")

        return self.adj_matrix[vi][vj]

    def print_graph(self):
        for vi in range(self.ver_count):
            for vj in range(self.ver_count):
                val = self.get_edge(vi, vj)
                if val:
                    print(str(vi) + ' - ' + str(vj) + ' : ' + str(val))

if __name__ == '__main__':
    graph = Graph(5)
    edges = [[1, 2, 5], [2, 1, 5], [1, 3, 30], [3, 1, 30], [2, 3, 14], [3, 2, 14], [2, 4, 26], [4, 2, 26]]
    graph.create(edges)
    print(graph.get_edge(3, 4))
    graph.print_graph()
```

邻接矩阵的特点 : 

- 优点 : 实现简单 , 并且可以直接查询顶点 $vi$ 与 $vj$ 之间是否有边存在 , 还可以直接查询边的权值
- 缺点 : 初始化效率和遍历效率较低 , 空间开销大 , 空间利用率低 , 并且不能存储重复边 , 也不便于增删节点 , 如果当顶点数目过大 (比如当 $n>10^5$) 时 , 使用邻接矩阵建立一个 $n×n$ 的二维数组不太现实

### 邻接表

**邻接表 (Adjacency List) :** 使用顺序存储和链式存储相结合的存储结构来存储图的顶点和边 , 其数据结构包括两个部分 , 其中一个部分是数组 , 主要用来存放顶点的数据信息 , 另一个部分是链表 , 用来存放边信息

![graph-06](https://github.com/attack-on-backend/algorithm/blob/master/assert/graph-06.png?raw=true)

两种实现方式 , 数组和哈希表 , 其表现形式如下 : 

```python
# 数组
adj_list = [
    [1, 3],  # 节点 0 的邻居
    [0, 2],  # 节点 1 的邻居
    [1, 3],  # 节点 2 的邻居
    [0, 2]   # 节点 3 的邻居
]

# 哈希表
adj_set = {
    0: {1, 3},
    1: {0, 2},
    2: {1, 3},
    3: {0, 2}
}
```

#### 数组实现

```python
class EdgeNode:
    def __init__(self, vj, val):
        # 边的终点
        self.vj = vj
        # 边的权值
        self.val = val
        # 下一条边
        self.next = None

class VertexNode:  # 顶点信息类
    def __init__(self, vi):
        # 边的起点
        self.vi = vi
        # 下一个邻接点
        self.head = None

class Graph:
    def __init__(self, ver_count):
        self.ver_count = ver_count
        self.vertices = []
        for vi in range(ver_count):
            vertex = VertexNode(vi)
            self.vertices.append(vertex)

    # 判断顶点 v 是否有效
    def _valid(self, v):
        return 0 <= v <= self.ver_count

    def create(self, edges=None):
        if edges is None:
            edges = []
        for vi, vj, val in edges:
            self.add_edge(vi, vj, val)

    # 添加边vi - vj, 权值为 val
    def add_edge(self, vi, vj, val):
        if not self._valid(vi) or not self._valid(vj):
            raise ValueError(str(vi) + ' or ' + str(vj) + " is not a valid vertex.")

        vertex = self.vertices[vi]
        edge = EdgeNode(vj, val)
        edge.next = vertex.head
        vertex.head = edge

    # 获取 vi - vj 边的权值
    def get_edge(self, vi, vj):
        if not self._valid(vi) or not self._valid(vj):
            raise ValueError(str(vi) + ' or ' + str(vj) + " is not a valid vertex.")

        vertex = self.vertices[vi]
        cur_edge = vertex.head
        while cur_edge:
            if cur_edge.vj == vj:
                return cur_edge.val
            cur_edge = cur_edge.next
        return None

    # 根据邻接表打印图的边
    def print_graph(self):
        for vertex in self.vertices:
            cur_edge = vertex.head
            while cur_edge:
                print(str(vertex.vi) + ' - ' + str(cur_edge.vj) + ' : ' + str(cur_edge.val))
                cur_edge = cur_edge.next

if __name__ == '__main__':
    graph = Graph(7)
    edges = [[1, 2, 5], [1, 5, 6], [2, 4, 7], [4, 3, 9], [3, 1, 2], [5, 6, 8], [6, 4, 3]]
    graph.create(edges)
    print(graph.get_edge(3, 4))
    graph.print_graph()
```

#### 哈希表实现

```python
class VertexNode:
    def __init__(self, vi):
        # 顶点
        self.vi = vi
        # 顶点的邻接边
        self.adj_edges = dict()

class Graph:
    def __init__(self):
        # 顶点
        self.vertices = dict()

    def create(self, edges=None):
        if edges is None:
            edges = []
        for vi, vj, val in edges:
            self.add_edge(vi, vj, val)

    # 向图中添加节点
    def add_vertex(self, vi):
        vertex = VertexNode(vi)
        self.vertices[vi] = vertex

    # 添加边vi - vj, 权值为 val
    def add_edge(self, vi, vj, val):
        if vi not in self.vertices:
            self.add_vertex(vi)
        if vj not in self.vertices:
            self.add_vertex(vj)

        self.vertices[vi].adj_edges[vj] = val

    # 获取 vi - vj 边的权值
    def get_edge(self, vi, vj):
        if vi in self.vertices and vj in self.vertices[vi].adj_edges:
            return self.vertices[vi].adj_edges[vj]
        return None

    # 根据邻接表打印图的边
    def print_graph(self):
        for vi in self.vertices:
            for vj in self.vertices[vi].adj_edges:
                print(str(vi) + ' - ' + str(vj) + ' : ' + str(self.vertices[vi].adj_edges[vj]))

if __name__ == '__main__':
    graph = Graph()
    edges = [[1, 2, 5], [1, 5, 6], [2, 4, 7], [4, 3, 9], [3, 1, 2], [5, 6, 8], [6, 4, 3]]
    graph.create(edges)
    print(graph.get_edge(3, 4))
    graph.print_graph()
```

### 边集数组

**边集数组 (Edgeset Array) :** 使用一个数组来存储存储顶点之间的邻接关系 , 数组中每个元素都包含一条边的起点 $vi$、终点 $vj$ 和边的权值 $val$ (如果是带权图)

![graph-05](https://github.com/attack-on-backend/algorithm/blob/master/assert/graph-05.png?raw=true)

代码实现

```python
class EdgeNode:
    def __init__(self, vi, vj, val):
        # 边的起点
        self.vi = vi
        # 边的终点
        self.vj = vj
        # 边的权值
        self.val = val

class Graph:
    def __init__(self):
        self.edges = []  # 边数组

    # 图的创建操作，edges 为边信息
    def create(self, edges=None):
        if edges is None:
            edges = []
        for vi, vj, val in edges:
            self.add_edge(vi, vj, val)

    # 添加边vi - vj, 权值为 val
    def add_edge(self, vi, vj, val):
        edge = EdgeNode(vi, vj, val)
        self.edges.append(edge)

    # 获取 vi - vj 边的权值
    def get_edge(self, vi, vj):
        for edge in self.edges:
            if vi == edge.vi and vj == edge.vj:
                val = edge.val
                return val
        return None

    # 根据边数组打印图
    def print_graph(self):
        for edge in self.edges:
            print(str(edge.vi) + ' - ' + str(edge.vj) + ' : ' + str(edge.val))

if __name__ == '__main__':
    graph = Graph()
    edges = [[1, 2, 5], [1, 5, 6], [2, 4, 7], [4, 3, 9], [3, 1, 2], [5, 6, 8], [6, 4, 3]]
    graph.create(edges)
    print(graph.get_edge(3, 4))
    graph.print_graph()
```

## 例题

- [LCP 07. 传递信息](https://leetcode.cn/problems/chuan-di-xin-xi/)
- [1791. 找出星型图的中心节点](https://leetcode.cn/problems/find-center-of-star-graph/)
- [133. 克隆图](https://leetcode.cn/problems/clone-graph/)

