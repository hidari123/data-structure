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
1. `append(element)`：向列表尾部添加一个新的项
2. `insert(position, element)`：列表指定位置插入一个新的项
3. `get(position)`：获取对应位置的元素
4. `indexOf(element)`：返回元素在列表中的索引，如果列表中没有该元素则返回`-1`
5. `update(position)`：修改某个位置的元素
6. `removeAt(position)`：从列表的特定位置移除一项
7. `remove(element)`：从列表中移除一项
8. `isEmpty()`：如果链表中不包含任何元素，返回`true`，如果链表长度大于`0`则返回`false`
9. `size()`：返回链表包含的元素个数，与数组`length`属性类似
10. `toString()`：由于列表使用了`Node`类，需要重写继承自JavaScript对象的默认`toString`方法，让其只输出元素的值

## 封装
```js
// 创建Node节点类
class Node {
    constructor(element) {
        this.data = element
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
}
```
