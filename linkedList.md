# 链表

## 优势
1. 内存中`不必是连续空间`
2. 每个元素由`储存元素本身的节点`和`指向下一个元素的引用（指针）`组成
3. 相对于数组优点：
    1. 内存动态管理
    2. 不必创建时确定大小，可以无限延伸
    3. 插入和删除时，时间复杂度可以达到O(1)，相对数组效率高很多
4. 相对于数组缺点：
    1. 访问任意位置元素都需要从头开始访问
    2. 无法通过下标直接访问元素

## 链表常见操作
1. `append(element)`：向链表尾部添加一个新的项
2. `insert(position, element)`：链表指定位置插入一个新的项
3. `get(position)`：获取对应位置的元素
4. `indexOf(element)`：返回元素在链表中的索引，如果链表中没有该元素则返回`-1`
5. `update(position, element)`：修改某个位置的元素
6. `removeAt(position)`：从链表的特定位置移除一项
7. `remove(element)`：从链表中移除一项
8. `isEmpty()`：如果链表中不包含任何元素，返回`true`，如果链表长度大于`0`则返回`false`
9. `size()`：返回链表包含的元素个数，与数组`length`属性类似
10. `toString()`：由于链表使用了`Node`类，需要重写继承自JavaScript对象的默认`toString`方法，让其只输出元素的值

## 封装
```js
// 创建Node节点类
class Node {
   constructor(element) {
      this.element = element
      this.next = null
   }
}
// 封装链表类
class LinkedList {
   // 属性
   constructor() {
      this.head = null // 默认指向空
      this.length = 0 // 初始长度为0
   }

   append(element) {
      // 创建新的节点
      let node = new Node(element)
      // 判断是否添加的是第一个元素
      if(this.length === 0) {
         this.head = node
         node.next = null // 可以不写
      } else {
         // 找到最后一个节点
         let current = this.head // current 指向 head 的 next指针 指向的第一个节点
         while (current.next) { // next指针 不为空的时候
            current = current.next // 让 current的 next指针 指向 下一个节点，直到 current 为空
            node.next = null //也可以不写
         }
         // 最后节点的 next 指向新节点 node
         current.next = node
      }
      // 链表长度 +1
      this.length++
   }

   toString() {
      // 定义变量
      let current = this.head
      let listString = ''
      // 循环获取节点
      while (current) {
         listString += current.element + ' '
         current = current.next
      }
      return listString
   }

   insert(position, element) {
      // 对 position 越界判断
      if (position < 0 || position > this.length) return false
      // 创建节点
      let node = new Node(element)
      // 插入 position = 0 的位置
      if (position === 0) {
         node.next = this.head // 此时 this.head 指向的是原先的第一个节点 让插入节点的 next指针 指向原先的第一个节点
         this.head = node // 将 head 的 next指针 指向 node
      } else {
         let index = 0
         let current = this.head // current 是第一个节点
         let previous = null // current 是第一个节点的时候，前一个是 null
         while (index++ < position) { // 先判断 index 是否小于 position 再++
            previous = current // 此时 current 指向的是 position 把此时的 position 赋值给前一个节点 previous
            current = current.next // 找到需要插入的位置的当前节点，current 指向的是 position 的后一个位置
         }
         node.next = current // 插入的节点的 next指针 指向这个位置原先的节点
         previous.next = node // 前一个节点 next指针 指向插入的节点
      }
   }

   get(position) {
      // 越界判断
      if (position < 0 || position >= this.length) return false
      // 获取对应的 element
      let current = this.head
      let index = 0
      while (index++ < position) {
         current = current.next
      }
      return current.element
   }

   indexOf(element) {
      let current = this.head
      let index = 0
      while (current) {
         if (current.element === element) return index
         current = current.next
         index++
      }
      // 没有找到
      return -1
   }

   update(position, element) {
      // 越界判断
      if (position < 0 || position >= this.length) return false
      let current = this.head
      let index = 0
      while (index++ < position) {
         current = current.next
      }
      current.element = element
      return true
   }

   removeAt(position) {
      if (position < 0 || position >= this.length) return false
      let current = this.head
      // 判断删除的是否是第一个节点
      if (position === 0) {
         // 原来的 this.head 指向要被删除的节点 现在指向被删除的下一个节点
         this.head = this.head.next // 被删除节点也指向 this.head.next 不用管 没有指针指向被删除节点 浏览器自动清除没有被引用的数据
      } else {
         let index = 0
         let previous = null
         if (index++ < position) {
            previous = current
            current = current.next
         }
         previous.next = current.next
      }
      // 链表长度 -1
      this.length--
      return true
   }

   remove(element) {
      // 获取 element 在链表中的位置
      let position = this.indexOf(element)
      // 根据位置信息删除节点
      if (position < 0) return false
      return this.removeAt(position)
   }

   isEmpty() {
      return this.length === 0
   }

   size() {
      return this.length
   }
}

// test
let list = new LinkedList()
list.append('123')
list.append('456')
list.append('789')
list.removeAt(1)
list.append('abc')
list.remove('123')
list.update(0,'2')
console.log(list.indexOf('2'))
console.log(list.isEmpty())
console.log(list.toString())
console.log(list.size())
console.log(list)
console.log(list.get(1))
```
