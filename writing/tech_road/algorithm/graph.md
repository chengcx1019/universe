# 图

理清图中边的分类，练通分量的相关概念，验证是否有向无环图

一个有向图是无环的，当且仅当对其进行深度的优先搜索不产生后向边

在对无向图进行深度优先时，每条边要么是树边，要么是后向边

subgraph isomorphism detection 同构检测

pseudographs 伪图：多条边（两条以上边），self-loop

A* 启发式搜索算法



如何用图思考[原文](https://medium.freecodecamp.org/i-dont-understand-graph-theory-1c96572a1401)

为了方便描述，有如下定义：

$G=(V,E)$ 定义图，其中 $V$ 代表顶点集合， $E$ 代表边集合

对图的操作可以分为以下三类：

- `create` : 生成
- `inspect` 查询或判定，判断图是否为有向图，给定顶点返回所有邻接边等
- `update` : 更新，添加顶点或边






## 图的表示

- 邻接数组-静态图

- 邻接表-动态图

- 邻接矩阵

  这里需要注意的是不同场景下的选择会不一样，稠密图可以考虑用邻接矩阵表示法，需要快速判断任意两个顶点间是否有边连接，可能也需要使用邻接矩阵表示法



## 图的遍历

> 相比广度优先使用队列存储未访问的邻接节点，深度遍历的循环实现方式使用栈存储当前节点的未访问邻接节点



### 广度优先搜索

广度优先搜索从边上来说，都是从源点到图中任意顶点的最短路径



### 深度优先搜索



顶点访问顺序会改变计数器的值，所以需要注意临接节点的顺序，深度优先搜索计算出来的结果非常有用，包括**拓扑排序**，寻找**强连通部**，寻找网络中潜在的弱点。

深度优先搜索结束后，可以使用每个顶点存储的前序节点值找到一条从**任意顶点到原点s的路径**，当然，这条路径也许不是最短路径。

深度优先搜索仅仅依靠当前信息，是一种盲目的搜索，它没有一个明智的计划来快速达到目标顶点t。



算法分析：

- 对于有向图，点和边都会被访问一次

- 对于无向图，对每个顶点都需要回去检查一次，顶点和边都被访问了两次

  综上，算法复杂度为 `O(V+E)` 



### 边的分类

在树、有向图或者无向图上执行深度优先搜索后边的类型

- 树边(tree edge)
- 反向边(back edge)
- 正向边(forward edge)
- 交叉边(cross edge)



### 连通图

没有不可达顶点的图，即在每一对顶点之间必须有一条路径。





### 验证图是否是有向无环图

有几种不同的方式：

1. 根据深度优先搜索后边的类型进行判读，图中的边只能是树边或反向边，一旦发现反向边，则表明存在环





## 求解最短路径





### 单源最短路径



#### Dijkstra :非负代价边

Dijkstra 算法借助优先队列来存储每个顶点到源顶点的最短路径（记为优先级），同时算法会记录每个顶点的前置顶点，算法结束后，输入终止顶点，可以得到源顶点到终止顶点的最短路径。



开始 Dijkstra 算法之前，先用数组实现一个最简单的优先队列：

```python
import sys


class PriorityQueueNode(object):
    def __init__(self, obj, key):
        self.obj = obj
        self.key = key  # 优先级

    def __repr__(self):
        return str(self.obj) + ':' + str(self.key)


class PriorityQueue(object):
    def __init__(self):
        self.array = []

    def __len__(self):
        return len(self.array)

    def insert(self, node):
        self.array.append(node)
        return self.array[-1]

    def extra_min(self):
        if len(self.array) == 0:
            return None
        minimum = sys.maxsize
        mini_index = 0
        for index, node in enumerate(self.array):
            if node.key < minimum
                minimum = node.key
                mini_index = index
        return self.array.pop(mini_index)

    def decrease_key(self, obj, new_key):
        for node in self.array:
            if node.obj is obj:
                node.key = new_key
                return node
        return None
```





```python
import sys


class Dijkstra(object):
    def __init__(self, graph):
        sele.graph = graph
        self.pred = {}  # 记录最短路径下每个顶点的前置顶点
        self.dist = {}  # 记录每个顶点到源顶点的最短距离
        self.priority_queue = PriorityQueue()

        for node in self.graph.nodes.values():
            self.pred[node.key] = -1
            self.dist[node.key] = sys.maxsize
            self.priority_queue.insert(PriorityQueueNode(node.key, self.dist[node.key]))

    def dijkstra(self, start_node_key, end_node_key=None):
        if start_node_key is None:
            raise TypeError('Input node keys cannot be None')
        if start_node_key not in self.graph.nodes:
            raise ValueError('Invalid start or end node key')

        self.dist[start_node_key] = 0
        self.priority_queue.decrease_key(start_node_key, self.dist[start_node_key])

        while self.priority_queue:
            current_min = self.priority_queue.extra_min().obj  # 队列不为空，必有值
            current_node = self.graph.nodes[current_min]

            for node in current_node.adj_nodes.values():
                weight = current_node.adj_weights[node.key]
                new_weight = self.dist[current_min] + weight
                if new_weight < self.dist[node.key]:
                    self.dist[node.key] = new_weight
                    self.priority_queue.decrease_key(node.key, new_weight)
                    self.pred[node.key] = current_min

        if end_node_key:
            return self.get_full_shortest_path(end_node_key)

    def get_full_shortest_path(self, end_node_key):
        result = []
        current_node_key = end_node_key
        while current_node_key != -1:
            result.append(current_node_key)
            current_node_key = self.pred[current_node_key]
        return result[::-1]
        
```

> 完整代码参考 [github](https://github.com/chengcx1019/architecture/blob/master/algorithm/basic/graph/dijkstra.py)



#### Bellman-Ford : 任意代价边

注：不可包含总权值为负值的环



### 任意两个顶点间最短路径



#### 动态规划Floyd-Warshall





## 其他典型问题描述



### 有向无环图中的拓扑排序

参考[这篇文章](https://changxin10m.com/postDetail/%2Fblog%2Fapi%2Ftuo-bu-pai-xu,61%2F) 



### 最小生成树

给定无向连通图$G=(V,E)$ (无向带权重)，最小生成树：（1）包含图中的所有顶点，（2）所有边的权重之和是最小的，典型算法 `Prime `, 实现 `Prime` 算法的过程中需要用的优先队列，选择最小权重边





## reference

https://github.com/kamyu104/LeetCode.git

https://github.com/hickford/codejam.git





## todo

深度优先搜索的应用

有向图中计算强连通分量

## 关于图的其他

- 欧拉回路(不重复经过所有边)

  有限无向连通图中，如果一个顶点的度数为奇数，不可能进入该顶点又离开，一个图是欧拉回路的条件是度为奇数的顶点只有两个，或者所有顶点的度均为偶数。

- 曼哈顿回路(不重复经过所有点)

