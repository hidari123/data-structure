# 优先级队列
1. 插入元素时考虑数据优先级
2. 根据优先级放入正确的位置
3. 每个元素包含数据和优先级
4. 例如线程

## 优先级队列常见操作

## 优先级队列封装
1. 封装元素和优先级放在一起，可以封装一个新的构造函数
2. 添加元素时，将新插入元素的优先级和队列中已经存在的优先级进行比较，以获得正确的位置
```js
class PriorityQueue {
    constructor (items) {
        this.items = []
    }
    // enqueue
    enqueue() {
        // 创建 QueueElement 对象
        let queueElement = new QueueElement(element, priority)
        if ()
    }
}
// 内部类，作为类的静态属性
PriorityQueue.QueueElement = class {
    constructor (element, priority) {
        this.element = element
        this.priority = priority
    }
}
let priorityQueue = new PriorityQueue()
```
