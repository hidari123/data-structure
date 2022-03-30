# 图
![avatar](https://upload-images.jianshu.io/upload_images/1102036-7e22c0e47e42f69a?imageMogr2/auto-orient/strip|imageView2/2/w/1006/format/webp)
## 特点
1. 一组顶点：通常用 V (Vertex) 表示顶点的集合 
2. 一组边：通常用 E (Edge) 表示边的集合 
   - 边是顶点和顶点之间的连线 
   - 边可以是有向的, 也可以是无向的.(比如A --- B, 通常表示无向. A --> B, 通常表示有向)
3. 相邻顶点：由一条边连接在一起的顶点称为相邻顶点
4. 度：一个顶点的度是相邻顶点的数量 => 比如0顶点和其他两个顶点相连，0顶点的度是2
5. 路径：路径是顶点v1, v2..., vn的一个连续序列，比如上图中0 1 5 9就是一条路径.
   - 简单路径: 简单路径要求不包含重复的顶点. 比如 0 1 5 9是一条简单路径. 
   - 回路: 第一个顶点和最后一个顶点相同的路径称为回路. 比如 0 1 5 6 3 0
6. 无向图：上面的图就是一张无向图, 因为所有的边都没有方向，比如 0 - 1之间有边，那么说明这条边可以保证 0 -> 1，也可以保证 1 -> 0. 
7. 有向图： 有向图表示的图中的边是有方向的 => 比如 0 -> 1，不能保证一定可以 1 -> 0，要根据方向来定
8. 无权图和带权图 
   1. 无权图：我们上面的图就是一张无权图(边没有携带权重)
      - 我们上面的图中的边是没有任何意义的，不能收 0 - 1的边，比4 - 9的边更远或者用的时间更长
   2. 带权图：带权图表示边有一定的权重
      - 这里的权重可以是任意你希望表示的数据：比如距离或者花费的时间或者票价
![avatar](https://upload-images.jianshu.io/upload_images/1102036-6511eda63dc574f6?imageMogr2/auto-orient/strip|imageView2/2/w/980/format/webp)

## 现实建模
图可用于对现实中很多系统建模
1. 对交通流量建模
   - 顶点可以表示街道的十字路口，边可以表示街道
   - 加权的边可以表示限速或者车道的数量或者街道的距离
   - 建模人员可以用这个系统来判定最佳路线以及最可能堵车的街道
2. 对飞机航线建模
   - 航空公司可以用图来为其飞行系统建模
   - 将每个机场看成顶点，将经过两个顶点的每条航线看作一条边
   - 加权的边可以表示从一个机场到另一个机场的航班成本，或两个机场间的距离
   - 建模人员可以利用这个系统有效的判断从一个城市到另一个城市的最小航行成本

## 图的表示
1. 邻接矩阵
   - 邻接矩阵让每个节点和一个整数向关联，该整数作为数组的下标值
   - 用一个二维数组来表示顶点之间的连接
   - ![avatar](https://upload-images.jianshu.io/upload_images/1102036-9a6cd682c1cf8d29?imageMogr2/auto-orient/strip|imageView2/2/w/1024/format/webp)
   - 图片解析:
      - 在二维数组中，0表示没有连线，1表示有连线
      - 通过二维数组，我们可以很快的找到一个顶点和哪些顶点有连线 (比如A顶点，只需要遍历第一行即可)
      - 另外，A - A，B - B(也就是顶点到自己的连线)，通常使用0表示
   - 邻接矩阵的问题：
      - 如果是一个无向图，邻接矩阵展示出来的二维数组，其实是一个对称图
      - 也就是A -> D是1的时候，对称的位置 D -> A 一定也是1
      - 那么这种情况下会造成空间的浪费
   - 邻接矩阵还有一个比较严重的问题就是如果图是一个稀疏图
      - 那么矩阵中将存在大量的0，这意味着我们浪费了计算机存储空间来表示根本不存在的边
      - 而且即使只有一个边，我们也必须遍历一行来找出这个边，也浪费很多时间
2. 邻接表
   - 邻接表由图中每个顶点以及和顶点相邻的顶点列表组成
   - 这个列表有很多中方式来存储：数组 / 链表 / 字典(哈希表)都可以
   - ![avatar](https://upload-images.jianshu.io/upload_images/1102036-be0b2812d32fe1e8?imageMogr2/auto-orient/strip|imageView2/2/w/1024/format/webp)
   - 图片解析：
      - 要表示和A顶点有关联的顶点(边)，A和B/C/D有边，那么我们可以通过A找到对应的数组/链表/字典，再取出其中的内容就可以
   - 邻接表的问题： 
      - 邻接表计算"出度"是比较简单的 (出度：指向别人的数量，入度：指向自己的数量)
      - 邻接表如果需要计算有向图的"入度"，那么是一件非常麻烦的事情
      - 它必须构造一个"逆邻接表"，才能有效的计算"入度"，而邻接矩阵会非常简单


## 图的遍历
1. 图的遍历思想
   - 图的遍历算法的思想在于必须访问每个第一次访问的节点，并且追踪有哪些顶点还没有被访问到
2. 有两种算法可以对图进行遍历
   1. 广度优先搜索(Breadth-First Search, 简称BFS)
      - ![avatar](https://upload-images.jianshu.io/upload_images/1102036-97199fd04e5c6515?imageMogr2/auto-orient/strip|imageView2/2/w/1024/format/webp)
   2. 深度优先搜索(Depth-First Search, 简称DFS)
      - ![avatar](https://upload-images.jianshu.io/upload_images/1102036-1c873ab8dad0805e?imageMogr2/auto-orient/strip|imageView2/2/w/1024/format/webp)
      - 两种遍历算法, 都需要明确指定第一个被访问的顶点
3. 遍历的注意点：
   - 完全探索一个顶点要求我们便查看该顶点的每一条边
   - 对于每一条所连接的没有被访问过的顶点，将其标注为被发现的，并将其加进待访问顶点列表中
   - 为了保证算法的效率：每个顶点至多访问两次
4. 两种算法的思想：
   1. BFS: 基于队列，入队列的顶点先被探索
   2. DFS: 基于栈，通过将顶点存入栈中，顶点是沿着路径被探索的，存在新的相邻顶点就去访问
5. 为了记录顶点是否被访问过，我们使用三种颜色来反应它们的状态：(或者两种颜色也可以)
   1. 白色: 表示该顶点还没有被访问
   2. 灰色: 表示该顶点被访问过，但并未被探索过
   3. 黑色: 表示该顶点被访问过且被完全探索过

## 封装
- 添加测试代码效果图
- ![avatar](https://upload-images.jianshu.io/upload_images/1102036-38c900671fd33dc7?imageMogr2/auto-orient/strip|imageView2/2/w/1024/format/webp)
```js
// 封装图需要用到 字典 和 队列
class Dictionay {
    // 字典属性
    constructor() {
        this.items = {}
    }

    // 字典操作方法
    // 在字典中添加键值对
    set(key, value) {
        this.items[key] = value
    }

    // 判断字典中是否有某个key
    has(key) {
        return this.items.hasOwnProperty(key)
    }

    // 从字典中移除元素
    remove(key) {
        // 1.判断字典中是否有这个key
        if (!this.has(key)) return false

        // 2.从字典中删除key
        delete this.items[key]
        return true
    }

    // 根据key去获取value
    get(key) {
        return this.has(key) ? this.items[key] : undefined
    }

    // 获取所有的keys
    keys() {
        return Object.keys(this.items)
    }

    // 获取所有的value
    values() {
        return Object.values(this.items)
    }

    // size方法
    size() {
        return this.keys().length
    }

    // clear方法
    clear() {
        this.items = {}
    }
}

// 队列
class Queue {
    // 属性
    constructor (items) {
        this.items = []
    }
    // 方法
    // enqueue
    enqueue(element) {
        this.items.push(element)
    }
    // dequeue
    dequeue() {
        return this.items.shift()
    }
    // front
    front() {
        return this.items[0]
    }
    // isEmpty
    isEmpty() {
        return this.items.length === 0
    }
    // size
    size() {
        return this.items.length
    }
    // toString
    toString() {
        let resultString = ''
        for (let i = 0; i < this.items.length; i++) {
            resultString += this.items[i] + ' '
        }
        return resultString
    }
}

class Graph {
    constructor() {
        // 属性：顶点（数组），边（字典）
        this.vertexes = []
        this.edges = new Dictionay()
    }
    // 方法
    // 添加
    // 添加顶点
    addVertex(v) {
        this.vertexes.push(v)
        this.edges.set(v, [])
    }
    // 添加边
    addEdge(v1, v2) {
        this.edges.get(v1).push(v2)
        this.edges.get(v2).push(v1)
    }

    // toString
    toString() {
        let resultStr = ''
        // 遍历所有顶点以及顶点对应的边
        for (let i = 0; i < this.vertexes.length; i++) {
            resultStr += this.vertexes[i] + ' -> '
            let vEdges = this.edges.get(this.vertexes[i])
            for (let i = 0; i < vEdges.length; i++) {
                resultStr += vEdges[i] + ' '
            }
            resultStr += '\n'
        }
        return resultStr
    }

    // 遍历
    // 初始化状态颜色
    initializeColor() {
        let colors = []
        for (let i = 0; i < this.vertexes.length; i++) {
            // 给对象赋值 'white' 属性
            colors[this.vertexes[i]] = 'white'
        }
        return colors
    }

    // 广度优先搜索
    bfs(initV) {
        let resultStr = ''
        // 初始化颜色
        let colors = this.initializeColor()
        // 创建队列
        let queue = new Queue()
        // 将顶点加入到队列中
        queue.enqueue(initV)
        // 循环从队列中取元素
        while (!queue.isEmpty()) {
            // 从队列中取出一个顶点
            let v = queue.dequeue()
            // 获取和顶点相连的另一个顶点
            let vList = this.edges.get(v)
            // 将 v 的颜色设为灰色
            colors[v] = 'gray'
            // 遍历左右的顶点，并且加入到队列中
            for (let i = 0; i < vList.length; i++) {
                let a = vList[i]
                if (colors[a] === 'white') {
                    colors[a] = 'gray'
                    queue.enqueue(a)
                }
            }
            resultStr += v + ' '
            colors[v] = 'black'
        }
        return resultStr
    }

    // 深度优先搜索
    dfs(initV) {
        let arr = []
        let resultStr = ''
        // 初始化颜色
        let colors = this.initializeColor()
        // 遍历所有的顶点，开始访问
        for (let i = 0; i < this.vertexes.length; i++) {
            if (colors[this.vertexes[i]] === 'white') {
                arr = this.dfsVisit(initV, colors)
            }
        }
        for (let i = 0; i < arr.length; i++) {
            resultStr += arr[i] + ' '
        }
        return resultStr
    }

    // 深度优先搜索遍历
    dfsVisit(v, colors, resultStr = []) {
        // 1.将v的颜色设置为灰色
        colors[v] = "gray"
        // 2.处理v顶点
        // console.log(resultStr)

        // console.log(resultStr)
        // 3.v的所有邻接顶点的访问
        let vList = this.edges.get(v)
        for (let i = 0; i < vList.length; i++) {
            let a = vList[i]
            if (colors[a] === 'white') {
                this.dfsVisit(a, colors, resultStr)
            }
        }
        // 4.将v设置为黑色
        colors[v] = "black"
        resultStr.push(v)
        return resultStr
    }
}

// 测试代码
let graph = new Graph();

// 添加顶点
let myVertexes = ["A", "B", "C", "D", "E", "F", "G", "H", "I"];
for (let i = 0; i < myVertexes.length; i++) {
    graph.addVertex(myVertexes[i])
}

// 添加边
graph.addEdge('A', 'B');
graph.addEdge('A', 'C');
graph.addEdge('A', 'D');
graph.addEdge('C', 'D');
graph.addEdge('C', 'G');
graph.addEdge('D', 'G');
graph.addEdge('D', 'H');
graph.addEdge('B', 'E');
graph.addEdge('B', 'F');
graph.addEdge('E', 'I');
console.log(graph.toString())
console.log(graph.bfs(graph.vertexes[0]))
console.log(graph.dfs(graph.vertexes[0]))
```
