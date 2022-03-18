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
class PriorityQueue extends Queue{
    enqueue(element, priority = 0) {
        let insertIndex = this.items.length
        for (let i = 0; i < insertIndex; i++) {
            if (priority <= this.items[i][1]) {
                insertIndex = i
                break
            }
        }
        this.items.splice(insertIndex, 0, [element, priority])
    }
    dequeue() {
        let item = this.items.shift()
        return item ? item[0] : undefined
    }
    front() {
        let item = this.items[0]
        return item ? item[0] : undefined
    }
    isEmpty() {
        return this.items.length === 0
    }
    size() {
        return this.items.length
    }
    toString() {
        let resArr = []
        this.items.forEach((item) => {
            res.push(item[0])
        })
        return resArr.join(' ')
    }
}
let priorityQueue = new PriorityQueue()
```
