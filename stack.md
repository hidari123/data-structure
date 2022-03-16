# 栈
后进先出

## 栈的操作
1. `push()`添加一个元素到栈顶
2. `pop()`移除栈顶元素，同时返回被移除的元素
3. `peek()`返回栈顶的元素，不对栈做任何修改
4. `isEmpty()`如果栈里没有元素返回`true`，否则返回`false`
5. `size()`返回栈里元素的个数，与数组`length`属性类似
6. `toString()`将栈结构内容以字符形式返回

## 栈的封装
```js
// 用数组封装，栈底 index 为 0
class Stack {
    // 栈中的属性
    constructor (items) {
        this.items = []
    }
    // 栈的相关操作
    // push()
    push(element) {
        this.items.push(element)
    }
    // pop()
    pop() {
        return this.items.pop()
    }
    // peek()
    peek() {
        // 返回栈顶元素
        return this.items[this.items.length - 1]
    }
    // isEmpty()
    isEmpty() {
        return this.items.length === 0
    }
    // size()
    size() {
        return this.items.length
    }
    // toString()
    toString() {
        let resultString = ''
        for (let i = 0; i < this.items.length; i++) {
            resultString += this.items[i] + ' '
        }
        return resultString
    }
}
// 栈的使用
let stack = new Stack()
```

## 十进制转成二进制
```js
let decToBin = (decNumber) => {
    // 定义栈对象
    let stack = new Stack()
    // 循环操作
    while (decNumber > 0) {
        // 获取余数放入栈中
        stack.push(decNumber % 2)
        // 获取整除后的结果，作为下一次的数字
        decNumber = Math.floor(decNumber / 2) // 向下取整
    }
    // 从栈中取出 0 和 1
    let binaryString = ''
    while (!stack.isEmpty()) {
        binaryString += stack.pop()
    }
    return binaryString
}
// test
let test1 = decToBin(100)
console.log(test1)
```
