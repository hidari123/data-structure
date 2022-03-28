# 二叉查找树
1. 二叉查找树（BST，Binary Search Tree），也称二叉排序树或二叉搜索树
2. 二叉查找树是一颗二叉树, 可以为空；如果不为空，满足以下性质：
    1. 非空左子树的所有键值小于其根结点的键值。
    2. 非空右子树的所有键值大于其根结点的键值。
    3. 左、右子树本身也都是二叉搜索树。 
3. 二叉查找树的特点:
   1. 相对较小的值总是保存在左结点上，相对较大的值总是保存在右结点上
   那么利用这个特点，查找效率非常高，这也是二叉查找树中，搜索的来源
![avatar](https://img2018.cnblogs.com/blog/1590962/201908/1590962-20190804152452542-181454516.gif)

## 常见操作
1. `insert(key)`：向树中插入一个新的键。 
2. `search(key)`：在树中查找一个键，如果结点存在，则返回true；如果不存在，则返回false。 
3. `inOrderTraversal()`：通过中序遍历方式遍历所有结点。 
4. `preOrderTraversal()`：通过先序遍历方式遍历所有结点。 
5. `postOrderTraversal()`：通过后序遍历方式遍历所有结点。 
6. `min()`：返回树中最小的值/键。 
7. `max()`：返回树中最大的值/键。 
8. `remove(key)`：从树中移除某个键。

## 二叉查找树的缺陷
1. 二叉查找树的优势：
 - 可以**快速地找到**给定关键字的数据项，并且可以快速**地插入和删除数据项**
2. 二叉查找树的缺陷：
 - 查找效率和深度有关
 - 非平衡树：
     - 比较好的二叉查找树数据应该是**左右均匀分布**的
     - 但是插入连续数据后，分布的不均匀 => 非平衡树
     - 对于一棵**平衡二叉树**来说，插入 / 查找的效率是**O(logN)**
     - 对于一棵**非平衡二叉树**，相当于编写了一个链表，查找效率变成了**O(N)**
3. 树的平衡性：
  - 为了能以较快的时间O(logN)来操作一棵树，我们需要保证树总是平衡的
    - 树中每个节点左边的子孙节点个数，应该尽可能等于右边的子孙节点个数
    - 常见的平衡树
        - AVL树：最早的一种平衡树，每次插入 / 删除操作相对于红黑树效率不高，**整体效率不如红黑树**
        - 红黑树：平衡树的基本应用都是红黑树


## 封装
```js
class Node {
    constructor (key, data) {
        this.data = data
        this.key = key
        this.left = null
        this.right = null
        this.value = {}
        this.value[key] = data
    }
    search() {
        console.log(this.value)
    }
}
class BinarySearchTree {
    // 属性
    constructor () {
        this.root = null
    }

    // 插入
    insert(key, data) {
        // 根据 key 创建节点
        let node = new Node(key, data)
        // 判断根节点是否存在
        if (!this.root) {
            this.root = node
            return true
        }
        this.insertNode(this.root, node)
    }

    // 插入递归
    insertNode(node, newNode) {
        if (newNode.key < node.key) {
            // 向左查找
            if (!node.left) {
                node.left = newNode
                return true
            }
            this.insertNode(node.left, newNode)
        } else {
            // 向右查找
            if (!node.right) {
                node.right = newNode
                return true
            }
            this.insertNode(node.right, newNode)
        }
    }

    // 先序遍历
    preOrderTraversal() {
        let arr = this.preOrderTraversalNode(this.root)
        console.log(arr)
    }
    // 先序遍历递归
    preOrderTraversalNode(node, arr = []) {
        if (node) {
            // 处理经过的节点
            arr.push(node.value)
            // 处理经过节点的的左子节点
            this.preOrderTraversalNode(node.left, arr)
            // 处理经过节点的的右子节点
            this.preOrderTraversalNode(node.right, arr)
        }
        return arr
    }

    // 中序排列
    inOrderTraversal() {
        let arr = this.inOrderTraversalNode(this.root)
        console.log(arr)
    }
    // 中序遍历递归
    inOrderTraversalNode(node, arr = []) {
        if (node) {
            // 处理经过节点的的左子节点
            this.inOrderTraversalNode(node.left, arr)
            // 处理经过的节点
            arr.push(node.value)
            // 处理经过节点的的右子节点
            this.inOrderTraversalNode(node.right, arr)
        }
        return arr
    }

    // 后序排列
    postOrderTraversal() {
        let arr = this.postOrderTraversalNode(this.root)
        console.log(arr)
    }
    // 后序排列递归
    postOrderTraversalNode(node, arr = []) {
        if (node) {
            // 处理经过节点的的左子节点
            this.postOrderTraversalNode(node.left, arr)
            // 处理经过节点的的右子节点
            this.postOrderTraversalNode(node.right, arr)
            // 处理经过的节点
            arr.push(node.value)
        }
        return arr
    }
    
    // 最小值
    min() {
        if (this.root) {
            let node = this.root
            while (node.left) {
                node = node.left
            }
            return node.value
        }
    }

    // 最大值
    max() {
        if (this.root) {
            let node = this.root
            while (node.right) {
                node = node.right
            }
            return node.value
        }
    }

    // 搜索
    search(key) {
        return this.searchNode(this.root, key)

    }
    // 搜索递归
    searchNode(node, key) {
        // 如果传入的 node 为 null，退出递归
        // console.log(node)
        if (!node) return false
        // 判断 node 节点和传入的 key 值的大小
        if (node.key > key) { // 如果传入的 key 比 node 小
            // 向左查找
            return this.searchNode(node.left, key)
        } else if (node.key < key) {
            return this.searchNode(node.right, key)
        } else {
            return node.data
        }
    }

    // 删除节点
    remove(key) {
        // 1. 寻找要删除的节点
        // 1.1 定义变量，保存一些信息
        let current = this.root
        let parent = null
        // 1.2 开始寻找删除的节点
        while(current.key !== key) {
            parent = current
            if (current.key > key) {
                current = parent.left
            } else {
                current = parent.right
            }
            if (current === null) {
                return null
            }
        }
        // 2. 根据对应的情况删除节点
        // 找到了 current.key
        // 2.1 删除的是叶子节点
        if(current.left === null && current.right === null) {
            if(current === this.root) {
                this.root = null
                return current
            }
            current === parent.left ? parent.left = null : parent.right = null
            return current
        } 
        else if (current.left && !current.right || !current.left && current.right) {
            // 只有一个子节点
            if(current === this.root) {
                current.left ? this.root = current.left : this.root = current.right
                return current
            }
            if(current === parent.left) {
                current.left ? parent.left = current.left : parent.left = current.right
            } else {
                current.left ? parent.right = current.left : parent.right = current.right
            }
            return current
        } else {
            // 要删除的节点有两个子节点，要从下面的子节点中找到最接近 current 的子节点
            // 前驱 => 比 current 小一点点 => current 左子树最大值
            // 后继 => 比 current 大一点点 => current 右子树最小值
            // 1. 获取后继节点
            let successor = this.getSuccessor(current)
            // 2. current 是否是根节点
            if(this.root === current) {
                this.root = successor
                successor.left = current.left
                return current
            }
            current === parent.left ? parent.left = successor : parent.right = successor
            successor.left = current.left
            return current
        }
    }

    // 获取 前驱 / 后继
    getSuccessor(delNode) {
        // 后继
        // 定义变量 存储临时节点
        let successorParent = delNode
        let successor = delNode
        let current = delNode.right // 要从右子树开始找
        // 寻找节点
        while(current !== null) {
            successorParent = successor
            successor = current
            current = current.left
        }

        // 如果要删除的 delNode 的后继节点 successor 是否有左子树，有左子树需要把左子树上移到 successorParent 的左节点
        if(successor !== delNode.right) {
            successorParent.left = successor.right
            successor.right = delNode.right
        }
        return successor
    }
}
let bst = new BinarySearchTree()
bst.insert(11, 'a')
bst.insert(7, 'b')
bst.insert(15, 'c')
bst.insert(3, 'd')
bst.insert(9, 'e')
bst.insert(8, 'f')
bst.insert(10, 'g')
bst.insert(13, 'h')
bst.insert(12, 'i')
bst.insert(14, 'j')
bst.insert(20, 'k')
bst.insert(18, 'l')
bst.insert(25, 'm')
bst.insert(6, 'n')

bst.preOrderTraversal()
bst.inOrderTraversal()
bst.postOrderTraversal()
console.log(bst.min())
console.log(bst.max())
console.log(bst.remove(10))
bst.preOrderTraversal()
console.log(bst)
```