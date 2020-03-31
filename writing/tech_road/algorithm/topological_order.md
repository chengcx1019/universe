> 工作需要，为了向同事表达清楚观点，提高行动力，整理下思路。

拓扑排序：在计算机科学中，有向图的拓扑排序是其顶点的线性排序，使得对于从顶点 `u` 到顶点 `v` 的每个有向边`uv` ，`u` 的排序都在 `v` 前。在实际应用过程中，图的顶点可以表示待执行的任务，边可以表示一个任务必须在另一个任务之前执行的约束，在此应用中，拓扑排序只是一个有效的任务顺序。

执行拓扑排序有一个限制，当且仅当图中没有定向环时，才有可能进行拓扑排序。

任何有向无环图至少有一个拓扑排序，可以在线性时间内，构建任何有向无环图的拓扑排序。



目前实现拓扑排序的常用的算法有卡恩算法和深度优先搜索。



## 卡恩算法

算法流程：

1. 从图中选择一个入度为0的顶点，输出该顶点。
2. 从图中删除该节点及其所有出边（即与之邻接的所有顶点入度-1）
3. 反复执行1和2，直至所有节点输出，如果最后图中还有入度不为0的顶点，那么说明图中有回路，不能进行拓扑排序



### 伪码描述

```text
L ← 包含已排序的元素的列表，目前为空
S ← 入度为零的节点的集合
当 S 非空时：
    将节点n从S移走
    将n加到L尾部
    选出任意起点为n的边e = (n,m)，移除e。如m没有其它入边，则将m加入S。
    重复上一步。
如图中有剩余的边则：
    return error   (图中至少有一个环)
否则： 
    return L   (L为图的拓扑排序)
```









## 深度优先搜索

算法流程：

1. 对图执行深度优先搜索。
2. 在执行深度优先搜索时，若某个顶点不能继续前进，即顶点的出度为0，则将此顶点入栈。
3. 最后得到栈中顺序的逆序即为拓扑排序顺序。



伪码描述：

```text
L ← 包含已排序的元素的列表，目前为空
当图中存在未永久标记的节点时：
    选出任何未永久标记的节点n
    visit(n)
    
function visit(节点 n)
    如n已有永久标记：
        return
    如n已有临时标记：
        stop   (不是定向无环图)
    将n临时标记
    选出以n为起点的边(n,m)，visit(m)
    重复上一步
    去掉n的临时标记
    将n永久标记
    将n加到L的起始
```



python 示例:

图定义：

```python
class State(Enum):
    unvisited = 0  # white，顶点还未访问
    visiting = 1  # gray，顶点已被访问，但还存在未访问的邻接顶点
    visited = 2  # black，顶点已被访问，且所有邻接顶点都被访问


class Node(object):

    def __init__(self, key):
        self.key = key
        self.visit_state = State.unvisited
        self.incoming_edges = 0
        self.adj_nodes = {}  # Key = key, val = Node
        self.adj_weights = {}  # Key = key, val = weight

    def add_neighbor(self, neighbor, weight=0):
        if neighbor is None or weight is None:
            raise TypeError('neighbor or weight cannot be None')
        self.incoming_edges += 1
        self.adj_weights[neighbor.key] = weight
        self.adj_nodes[neighbor.key] = neighbor

    def remove_neighbor(self, neighbor):
        if neighbor is None:
            raise TypeError('neighbor cannot be None')
        if neighbor.key not in self.adj_nodes:
            raise KeyError('neighbor not found')
        self.incoming_edges += 1
        del self.adj_nodes[neighbor.key]
        del self.adj_weights[neighbor.key]


class Graph(object):

    def __init__(self, ):
        self.nodes = {}

    def add_node(self, key):
        if key is None:
            raise TypeError('key cannot be None')
        if key not in self.nodes:
            self.nodes[key] = Node(key)
        return self.nodes[key]

    def add_edge(self, source_key, dest_key, weight=0):
        if source_key is None or dest_key is None:
            raise KeyError('Invalid key')
        if source_key not in self.nodes:
            self.add_node(source_key)
        if dest_key not in self.nodes:
            self.add_node(dest_key)
        self.nodes[source_key].add_neighbor(self.nodes[dest_key], weight)

    def add_undirected_edge(self, source_key, dest_key, weight=0):
        if source_key is None or dest_key is None:
            raise KeyError('Invalid key')
        self.add_edge(source_key, dest_key, weight)
        self.add_edge(dest_key, source_key, weight)
```



图深度优先遍历：

```python
class GraphDfs(Graph):

    def __init__(self, ):
        super(GraphDfs, self).__init__()
        self.pred = {}
        self.discovered = {}  # 第一次访问该顶点计数器的值
        self.finished = {}  # 完成该顶点的深度优先搜索后计数器的值
        self.count = 0  # 计数器

    def dfs(self, root, visit_func):
        if root is None:
            return
        for node in self.nodes.values():
            self.pred[node.key] = -1
            self.discovered[node.key] = -1
            self.finished[node.key] = -1

        visit_func(self.nodes[root])
        # 针对非连通图而言
        for node in self.nodes.values():
            if node.visit_state == State.unvisited:
                visit_func(node)

    # 对于每一节点，访问开始时标记为灰色，递归访问完所有的邻接节点后标记为黑色
    def dfs_visit(self, current_node):
        current_node.visit_state = State.visiting
        self.count += 1  # 开始访问顶点及顶点访问结束均需要累加计数器
        self.discovered[current_node.key] = self.count
        for node in current_node.adj_nodes.values():
            if node.visit_state == State.unvisited:  # 如果顶点是不是white，那么当前顶点就会知道该邻接顶点已被或正在被访问
                self.pred[node.key] = current_node.key
                self.dfs_visit(node)
        current_node.visit_state = State.visited
        self.count += 1  # 开始访问顶点及顶点访问结束均需要累加计数器
        self.finished[current_node.key] = self.count
```



有向无环图的拓扑排序：

```python
class DAGTopSort(GraphDfs):

	def __init__(self):
		super(DAGTopSort, self).__init__()
		self.top_sort = []

	def make_top_sort(self, current_node):
		current_node.visit_state = State.visiting
		self.count += 1  # 开始访问顶点及顶点访问结束均需要累加计数器
		self.discovered[current_node.key] = self.count
		for node in current_node.adj_nodes.values():
			if node.visit_state == State.unvisited:  # 如果顶点是不是white，那么当前顶点就会知道该邻接顶点已被或正在被访问
				self.pred[node.key] = current_node.key
				self.make_top_sort(node)
		current_node.visit_state = State.visited
		self.top_sort.append(current_node.key)
		self.count += 1  # 开始访问顶点及顶点访问结束均需要累加计数器
		self.finished[current_node.key] = self.count
```



> 完整代码已上传 [github](https://github.com/chengcx1019/architecture/blob/master/algorithm/basic/graph/topplogical_sort.py)