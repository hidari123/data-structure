# 队列
1. 先进先出(First in First out)
2. 前端删除，后端插入

## 常见操作
1. `enqueue(element)`队列尾部添加一个或多个新的项
2. `dequeue()`移除队列的第一项，并返回被移除的元素
3. `front()`返回队列中的第一个元素————最先被添加，也最先被移除的元素，队列不做任何改动，类似Stack类中的peek方法
4. `isEmpty()`如果队列中不包含任何元素，返回`true`，否则返回`false`
5. `size()`返回队列包含的元素个数
6. `toString()`将队列中的内容转成字符串格式

## 队列封装
```js
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
let queue = new Queue()
```

## 击鼓传花
游戏规则：几个朋友一起玩一个游戏，围成一圈开始数数，数到某个数字的人自动淘汰，最后剩下的人获得胜利，请问最后剩下的是原来在哪个位置上的人？
```js
const passGame = (namelist, num) => {
    // 创建队列结构
    let queue = new Queue()
    // 将所有人依次加入到队列中
    for (let i = 0; i < namelist.length; i++) {
        queue.enqueue(namelist[i])
    }
    // 开始数数
    while (queue.size() > 1) {
        // 不是 num 重新加入队列
        // 是 num dequeue移除队列
        for (let i = 0; i < num - 1; i++) {
            queue.enqueue(queue.dequeue())
        }
        // 删除 num
        queue.dequeue()
    }
    console.log(queue.size())
    // 获取剩下的人
    const winner = queue.front()
    console.log('the winner is ' + winner)
    return namelist.indexOf(winner)
}
// test
const namelist = ['hidari', 'migi', 'april', 'shmily', 'sherlock']
const num = 3
passGame(namelist, num)
```

