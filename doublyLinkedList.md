# 双向链表
![avatar](https://upload-images.jianshu.io/upload_images/24753899-87479a4b3bbbb96c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1051/format/webp)
## 常见操作
1. `append(element)`：向链表尾部添加一个新的项
2. `insert(position, element)`：链表指定位置插入一个新的项
3. `get(position)`：获取对应位置的元素
4. `indexOf(element)`：返回元素在链表中的索引，如果链表中没有该元素则返回`-1`
5. `update(position, element)`：修改某个位置的元素
6. `removeAt(position)`：从列表的特定位置移除一项
7. `remove(element)`：从列表中移除一项
8. `isEmpty()`：如果链表中不包含任何元素，返回`true`，如果链表长度大于`0`则返回`false`
9. `size()`：返回链表包含的元素个数，与数组`length`属性类似
10. `toString()`：由于链表项使用了`Node`类，需要重写继承自JavaScript对象的默认`toString`方法，让其只输出元素的值
11. `forwardString()`: 返回正向遍历的节点字符串形式
12. `backwardString()`: 返回反向遍历的节点字符串形式

## 封装
```js
class Node {
    constructor (ele) {
        this.prev = null
        this.next = null
        this.ele = ele
    }
}
class DoublyLinkedList {
    // 属性
    constructor () {
        this.head = null // 头指针
        this.tail = null // 尾指针
        this.length = 0
    }
    append(ele) {
        let node = new Node(ele)
        // 判断链表是否为空
        if (this.head === null) {
            this.head = node
            this.tail = node
        } else {
            this.tail.next = node
            node.prev = this.tail
            this.tail = node
        }
        this.length++
    }

    insert(position, ele) {
        if (position < 0 || position > this.length) return false
        let node = new Node(ele)
        if (this.length === 0) {
            this.head = node
            this.tail = node
        } else {
            if (position === 0) {
                this.head.prev = node
                node.next = this.head
                this.head = node
            } else if(position === this.length) {
                this.tail.next = node
                node.prev = this.tail
                this.tail = node
            } else {
                let current = this.search(position)
                node.next = current
                node.prev = current.prev
                current.prev.next = node
                current.prev = node
            }
        }
        this.length++
        return true
    }

    get(position) {
        if (position < 0 || position >= this.length) return false
        let current = this.search(position)
        return current.ele
    }

    indexOf(ele) {
        let current = this.head
        let index = 0
        while (current) {
            if (current.ele === ele) return index
            current = current.next
            index++
        }
        return -1
    }

    update(position, ele) {
        if (position < 0 || position >= this.length) return false
        let current = this.search(position)
        current.ele = ele
        return true
    }

    removeAt(position) {
        if (position < 0 || position >= this.length) return null
        let current = this.head
        // 判断是否只有一个节点
        if (this.length === 1) {
            this.head = null
            this.tail = null
        } else {
            if (position === 0) {
                this.head.next.prev = null
                this.head = this.head.next
            } else if (position === this.length - 1) {
                current = this.tail
                this.tail.prev.next = null
                this.tail = this.tail.prev
            } else {
                current = this.search(position)
                current.prev.next = current.next
                current.next.prev = current.prev
            }
        }
        this.length--
        // 返回删除的元素
        return current.ele
    }

    remove(ele) {
        let index = this.indexOf(ele)
        return this.removeAt(index)
    }

    isEmpty() {
        return this.length === 0
    }

    size() {
        return this.length
    }

    getFirst() {
        return this.head.ele
    }

    getTail() {
        return this.tail.ele
    }

    toString() {
        return this.forwardString()
    }

    forwardString() {
        // 定义变量
        let current = this.head
        let resultString = ''

        // 依次向后遍历，获取每一个节点
        while (current) {
            resultString += current.ele + ' '
            current = current.next
        }
        return resultString
    }

    backwardString() {
        let current = this.tail
        let resultString = ''
        while (current) {
            resultString += current.ele + ' '
            current = current.prev
        }
        return resultString
    }

    search(position) {
        let current = null
        if (this.length / 2 > position) {
            current = this.head
            let index = 0
            while (index++ < position) {
                current = current.next
            }
        } else {
            current = this.tail
            let index = this.length - 1
            while (index-- > position) {
                current = current.prev
            }
        }
        return current
    }
}

let list = new DoublyLinkedList()
list.append('123')
list.append('654')
list.append('789')
list.insert(3,'abc')
console.log(list.forwardString())
console.log(list.backwardString())
console.log(list.toString())
console.log(list.get(1))
console.log(list.indexOf('abc'))
list.update(3,'def')
console.log(list.toString())
let a = list.removeAt(0)
console.log(list.toString())
console.log(a)
list.remove('def')
console.log(list.toString())
console.log(list.isEmpty())
console.log(list.size())
console.log(list.getTail())
console.log(list.getFirst())
```
