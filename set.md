# 集合

## 特性
1. 无序，不能重复
2. 不能通过下标值进行访问，相同对象在集合内只能存在一份

## 常见操作
1. `add(value)` 向集合添加一个新的项
2. `remove(value)` 从集合移除一个值
3. `has(value)` 如果值在集合中，返回 `true`，否则返回 `false`
4. `clear()` 移除集合中的所有项
5. `size()` 返回集合所包含元素的数量
6. `values()` 返回一个包含集合中所有值的数组

## 集合间操作
1. 并集：包含两个元素中所有元素的新集合
2. 交集：包含两个元素中共有元素的新集合
3. 差集：包含所有存在于第一个集合但不存在于第二个集合的元素的新集合
4. 子集：验证一个给定集合是否是另一集合的子集

## 封装
```js
class Set {
    constructor () {
        this.item = {}
    }
    add(value) {
        // 判断集合中是否包含该元素
        if(this.has(value)) return false
        this.item[value] = value
        return true
    }
    remove(value) {
        if(!this.has(value)) return false
        delete this.item[value]
        return true
    }
    clear() {
        this.item = []
    }
    size() {
        return Object.keys(this.item).length
    }
    values() {
        return Object.keys(this.item)
    }
    has(value) {
        return this.item.hasOwnProperty(value)
    }
    // 集合间操作
    // 并集
    union(otherSet) {
        let unionSet = new Set()
        let values = this.values()
        for (let i = 0; i < values.length; i++) {
            unionSet.add(values[i])
        }
        values = otherSet.values()
        for (let i = 0; i < values.length; i++) {
            if(!unionSet.has(values[i])) unionSet.add(values[i])
        }
        return unionSet
    }

    // 交集
    intersection(otherSet) {
        let intersectionSet = new Set()
        let values = this.values()
        for (let i = 0; i < values.length; i++) {
            let item = values[i]
            if (otherSet.has(item)) intersectionSet.add(item)
        }
        return intersectionSet
    }

    // 差集
    difference(otherSet) {
        let differenceSet = new Set()
        let values = this.values()
        for (let i = 0; i < values.length; i++) {
            let item = values[i]
            if (!otherSet.has(item)) differenceSet.add(item)
        }
        return differenceSet
    }

    // 子集
    subset(otherSet) {
        let values = this.values()
        for (let i = 0; i < values.length; i++) {
            let item = values[i]
            if (!otherSet.has(item)) return false
        }
        return true
    }
}

// test 1
let set = new Set()
set.add('abc')
set.add('def')
set.add('abc')
console.log(set.values())
console.log(set.has('abc'))
console.log(set.size())
set.remove('abc')
console.log(set.values())
set.clear()
console.log(set.values())

// test 2
let set = new Set()
set.add('abc')
set.add('def')
set.add('abc')

let otherSet = new Set()
otherSet.add('def')
otherSet.add('123')
set.union(otherSet)
console.log(otherSet.values())
console.log(set.union(otherSet).values())
console.log(set.intersection(otherSet).values())
console.log(set.difference(otherSet).values())

let sub = new Set()
sub.add('abc')
console.log(sub.subset(set))
```
