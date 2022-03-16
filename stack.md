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

## 判断算数表达式的括号是否一一对应
```js
/**
 * 例如 (5+3)*2-(4-3) 便一一对应，但是 (5+3)*2-(4-3 便不对应，
 * 栈可以判断这种问题，即从左到右遍历表达式，
 * 遇到左括号变将其压入栈中，遇到右括号则从栈中把左括号取出来抵消掉，
 * 最后如果栈栈中还有元素，则表达式不正确
 */
let matchBrackets = (str) => {
    let stack = new Stack()
    for(let i = 0; i < str.length; i++) {
        // charAt 返回指定索引处的字符
        if (str.charAt(i) === '(') {
            stack.push(str.charAt(i))
        } else if (str.charAt(i) === ')') {
            stack.pop()
        }
    }
    return stack.isEmpty()
}
let test2 = matchBrackets('(5+3)*2-(4-3)')
console.log(test2)
let test3 = matchBrackets('(5+3)*2-(4-3')
console.log(test3)
```

## 计算算数表达式的值
```js
/**
 * 从左到右把数字依次压入栈中，当遇到运算符时再从栈中取两个元素出来进行计算，得到结果后再压入栈中，循环最后得到结果
 */
let count = (formula) => {
    let stack = new Stack()
    let result = ''
    for (let i = 0; i < formula.length; i++) {
        // 如果没有遇到加减乘除
        if (['+', '-', '*', '/'].indexOf(formula[i] === -1)) {
            stack.push(formula[i])
        } else {
            // 取栈顶两个数字
            let num1 = stack.pop()
            let num2 = stack.pop()
            // eval() 函数计算或执行参数。
            // 如果参数是表达式，则 eval() 计算表达式。如果参数是一个或多个 JavaScript 语句，则 eval() 执行这些语句。
            result = eval(num1+formula[i]+num2)
            console.log(result)
            stack.push(result)
        }
    }
    return stack.peek()
}
let test4 = count([3,4,5,2,'-','+','*'])
console.log(test4)
```
