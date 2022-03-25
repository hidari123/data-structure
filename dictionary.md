# 字典

## 特点
1. key 和 value 一一对应，可以通过 key 取出 value（键值对）
2. key值不可重复，value可以重复，并且字典中的 key 是无序的
3. 字典和映射的关系： 
- 有些编程语言中称映射关系为字典，有些称映射关系为 Map
4. 字典和数组：
- 字典通过 key 获取 value，key 可以包含特殊含义
5. 字典和对象：
- JavaScript 中 字典和对象 类似

## 常见的操作
1. `set(key,value)`：向字典中添加新元素。 
2. `remove(key)`：通过使用键值来从字典中移除键值对应的数据值。 
3. `has(key)`：如果某个键值存在于这个字典中，则返回true，反之则返回false。 
4. `get(key)`：通过键值查找特定的数值并返回。 
5. `clear()`：将这个字典中的所有元素全部删除。 
6. `size()`：返回字典所包含元素的数量。与数组的length属性类似。 
7. `keys()`：将字典所包含的所有键名以数组形式返回。 
8. `values()`：将字典所包含的所有数值以数组形式返回。

## 封装
```js
// 创建字典的构造函数
class Dictionay {
    // 字典属性
    construtor() {
        this.items = {}
    }

    // 字典操作方法
    // 在字典中添加键值对
    set(key, value) {
        this.items[key] = value
    }

    // 判断字典中是否有某个key
    has(key) {
        return this.items.hasOwnProperty(key)
    }

    // 从字典中移除元素
    remove(key) {
        // 1.判断字典中是否有这个key
        if (!this.has(key)) return false

        // 2.从字典中删除key
        delete this.items[key]
        return true
    }

    // 根据key去获取value
    get(key) {
        return this.has(key) ? this.items[key] : undefined
    }

    // 获取所有的keys
    keys() {
        return Object.keys(this.items)
    }

    // 获取所有的value
    values() {
        return Object.values(this.items)
    }

    // size方法
    size() {
        return this.keys().length
    }

    // clear方法
    clear() {
        this.items = {}
    }
}

// 创建字典对象
var dict = new Dictionay()

// 在字典中添加元素
dict.set("age", 18)
dict.set("name", "Coderwhy")
dict.set("height", 1.88)
dict.set("address", "广州市")

// 获取字典的信息
alert(dict.keys()) // age,name,height,address
alert(dict.values()) // 18,Coderwhy,1.88,广州市
alert(dict.size()) // 4
alert(dict.get("name")) // Coderwhy

// 字典的删除方法
dict.remove("height")
alert(dict.keys())// age,name,address

// 清空字典
dict.clear()
```
