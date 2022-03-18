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
    // clear() 清空栈
    clear() {
        this.items = []
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
 * 例如 (5+3)*2-(4-3) 一一对应，但是 (5+3)*2-(4-3 便不对应，
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
 * 判断四则运算算式中哪些是数那些是操作符，分别压入两个栈，当遇到运算符时再从数字栈中取两个元素出来进行计算，得到结果后再压入数字栈中，循环最后得到结果
 */
let calculate = (numString) => {
    // 数字栈
    let numStack = new Stack()
    // 操作符栈
    let operationStack = new Stack()
    // 将字符串分割成数组
    let tokens = insertBlank(numString).split(' ')
    console.log('token is ' + tokens)
    // for in 遍历数组
    for (let i in tokens) {
        // 如果输入数字操作符时有空格，消除空格
        if (tokens[i] === '') {
            continue
        }
        // 加减操作
        else if (tokens[i].charAt(0) === '+' || tokens[i].charAt(0) === '-') {
            // 处理所有的操作符
            while (!operationStack.isEmpty() && (
                operationStack.peek() === "+" || operationStack.peek() === "-" || operationStack.peek() === "*" || operationStack.peek() === "/"
            )) {
                // 操作符栈中有东西先处理
                processAnOperator(numStack, operationStack)
            }
            // 将当前操作符 push 进栈
            operationStack.push(tokens[i].charAt(0))
        }
        // 乘除操作
        else if(tokens[i].charAt(0) === '*' || tokens[i].charAt(0) === '/') {
            while(!operationStack.isEmpty() && (operationStack.peek() === '*' || operationStack.peek() === '/')) {
                //如果遇到乘除操作符进行处理
                processAnOperator(numStack,operationStack)
            }
            operationStack.push(tokens[i].charAt(0))
        }
        // 左括号入栈
        else if (tokens[i].trim().charAt(0) === '(') {
            operationStack.push('(')
        }
        // 遇到右括号移出左括号
        else if (tokens[i].trim().charAt(0) === ')') {
            while (operationStack.peek() !== '(') {
                // 处理操作符直到遇到 (
                processAnOperator(numStack, operationStack)
            }
            // 将 ( 移出栈
            operationStack.pop()
        }
        // 遇到的是数字，移入数字栈
        else {
            // 将需要计算的数放到数字栈中
            numStack.push(parseFloat(tokens[i]))
        }
    }
    // 如果操作符栈中还有操作符，继续计算
    while(!operationStack.isEmpty()) {
        processAnOperator(numStack, operationStack)
    }
    return numStack.pop()
}

// 处理操作符
let processAnOperator = (numStack, operationStack) => {
    let op = operationStack.pop()
    let num1 = numStack.pop()
    let num2 = numStack.pop()
    if (op === '-') {
        numStack.push(num2 - num1);
    }
    else if (op === '+') {
        numStack.push(num2 + num1);
    }
    else if (op === '*') {
        numStack.push(num2 * num1)
    }
    else {
        numStack.push(num2 / num1);
    }
}

// 分割字符串，以便后续分割字符串为数组

let insertBlank = (numString) => {
    let blankString = ''
    for (let i = 0; i < numString.length; i++) {
        let indexOfI = numString[i]
        if (indexOfI === '+' || indexOfI === '-' || indexOfI === '*' || indexOfI === '/' || indexOfI ==='(' || indexOfI === ')') {
            blankString += ' ' + numString.charAt(i) + ' '
        } else {
            blankString += numString.charAt(i)
        }
    }
    return blankString
}

let test4 = '2.3+23/12+(3.1459*0.24)'
let resr = calculate(test4)
console.log("result is " + resr)
```