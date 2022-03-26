# 哈希表
1. 通常基于数组实现，相对数组有很多优势：
 - 快速插入-删除-查找操作
 - 无论多少数据，插入和删除需要更接近常量的时间，即`O(1)`的时间级 => 只需要几个机器指令
 - 哈希表的速度比树还要快，基本可以瞬间查到想要的元素
 - 哈希表相对于树来说编码容易很多
2. 相对数组的不足：
 - 哈希表中数据无序，不能以一种固定的方式（如从小到大）遍历元素
 - `key`不允许重复，不能放置相同的`key`，用于保存不同的元素
3. 它的结构是数组，神奇的地方在于对下标值的一种变换 => 哈希函数 => 获取到 `HashCode`
4. 概念：
    1. `哈希化`：大数字 => 数组范围内下标
    2. `哈希函数`：单词 => 大数字 + 大数字 => 哈希化
    3. `哈希表`：最终数据插入到的这个数组，对整个数组的封装
5. 哈希表冲突解决方案：
    1. 链地址法
    ![avatar](https://upload-images.jianshu.io/upload_images/1102036-0abbe32e9a9e5ec8?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
        1. 数组的每个数据单元中存储的是一个链条 => `数组 / 链表`
        2. 根据哈希化的`index`找出数组或链表时，通常使用线性查找，这个时候数组和链表效率差不多
        3. 某些情况下会将前端
    2. 开放地址法
    ![avatar](https://upload-images.jianshu.io/upload_images/1102036-a7b7cd5bb3a97185?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
         1. 寻找空白的位置来放置冲突的数据项有三种方法:线性探测 / 二次探测 / 再哈希法
         2. 线性探测：步长为`1`，线性的查找空白的单元
            - 插入:
              经过哈希化得到的`index`，但是在插入的时候，发现该位置已经有了其他元素，线性探测从`index`位置`+1`开始一点点查找空的位置
            - 查找：
                经过哈希化得到`index`，比较位置结果和查询的数值是否相同, 相同 => 直接返回
                不同呢 => 线性查找，从`index`位置`+1`开始查找
                注意: 如果元素之前没有插入，就是查询到空位置，就停止
              - 删除：
                  注意: 删除操作一个数据项时，不可以将这个位置下标的内容设置为`null`
                  因为将它设置为`null`可能会影响之后查询其他操作，所以通常删除一个位置的数据项时，可以将它进行特殊处理(比如设置为`-1`)
                  当之后看到`-1`位置的数据项时，就知道查询时要继续查询，但是插入时这个位置可以放置数据
            - 聚集问题：
                比如在没有任何数据的时候，插入的是一串连续元素，那么意味着下标值连续的位置都有元素，这种一连串填充单元就叫做聚集
                聚集会影响哈希表的性能，无论是`插入 / 查询 / 删除`都会影响.
                比如插入一个元素，会发现连续的单元都不允许我们放置数据，并且在这个过程中需要探索多次
                二次探测可以解决一部分这个问题
         3. 二次探测：
            - 二次探测在线性探测的基础上进行了优化:
              主要优化的是探测时的步长
              线性探测，我们可以看成是步长为1的探测，比如从下标值x开始，那么线性测试就是x+1，x+2，x+3依次探测
              二次探测，对步长做了优化，比如从下标值x开始，x+1²，x+2²，x+3²
              这样就可以一次性探测比较长的距离, 比避免那些聚集带来的影响
            - 问题:
              连续插入的是`32-112-82-2-192`，那它们依次累加的时候步长的相同的
              这种情况下会造成步长不一的一种聚集，还是会影响效率
         4. 再哈希法：
            - 二次探测的算法产生的探测序列步长是固定的：`1，4，9，16`，依次类推
              现在需要一种方法：产生一种依赖关键字的探测序列，而不是每个关键字都一样
              那么，不同的关键字即使映射到相同的数组下标，也可以使用不同的探测序列
              再哈希法的做法就是：把关键字用另外一个哈希函数，再做一次哈希化，用这次哈希化的结果作为步长
              对于指定的关键字，步长在整个探测中是不变的，不过不同的关键字使用不同的步长
            - 第二次哈希化需要具备如下特点：
              和第一个哈希函数不同 => (不要再使用上一次的哈希函数了，不然结果还是原来的位置)
              不能输出为0 => (否则，将没有步长，每次探测都是原地踏步，算法就进入了死循环)
            - 哈希函数:
              `stepSize = constant - (key % constant)`
              其中`constant`是质数，且小于数组的容量
              例如：`stepSize = 5 - (key % 5)`，满足需求，并且结果不可能为0
6. 哈希化的效率
   1. 哈希表中执行插入和搜索操作可以达到`O(1)`的时间级，如果没有发生冲突，只需要使用一次哈希函数和数组的引用，就可以插入一个新数据项或找到一个已经存在的数据项。 
   2. 如果发生冲突，存取时间就依赖后来的探测长度。一个单独的查找或插入时间与探测的长度成正比，这里还要加上哈希函数的常量时间。 
   3. 平均探测长度以及平均存取时间，取决于填装因子，随着填装因子变大，探测长度也越来越长。 
   4. 随着填装因子变大，效率下降的情况，在不同开放地址法方案中比链地址法更严重，所以我们来对比一下他们的效率，再决定我们选取的方案
7. 装填因子
   装填因子表示当前哈希表中已经包含的数据项和整个哈希表长度的比值
   `装填因子 = 总数据项 / 哈希表长度`
   开放地址法的装填因子最大是`1`，因为它必须寻找到空白的单元才能将元素放入
   链地址法的装填因子可以大于`1`，因为链地址法可以无限的延伸下去 (后面效率会变低)
8. 链地址法相对来说效率 > 开放地址法
   - 所以在真实开发中，使用链地址法的情况较多，因为它不会因为添加了某元素后性能急剧下降 => 比如在Java的`HashMap`中使用的就是链地址法
9. 好的哈希函数应该具备：
   1. 快速的计算 => 尽量少的有乘法和除法，多项式的优化：霍纳法则（秦九韶算法）
   2. 均匀的分布 => 需要在使用常量的地方，尽量使用质数
10. 哈希表扩容
    1. 为什么需要扩容？ 
     - 因为我们使用的是链地址法，`loadFactor`可以大于1，所以这个哈希表可以无限制的插入新数据
     - 但是，随着数据量的增多，每一个`index`对应的`bucket`会越来越长，也就造成效率的降低
     - 所以，在合适的情况对数组进行扩容
    2. 如何进行扩容?
     - 扩容可以简单的将容量增加大两倍
     - 但是这种情况下，所有的数据项一定要同时进行修改 => 重新哈希化，来获取到不同的位置
     - 这是一个耗时的过程，但是如果数组需要扩容，那么这个过程是必要的
    3. 什么情况下扩容呢?
     - 比较常见的情况是`loadFactor>0.75`的时候进行扩容
     - 比如Java的哈希表就是在装填因子大于`0.75`的时候，对哈希表进行扩容

## 封装
```js
class HashMap {
    constructor () {
        // 初始化用来存放数据的数组
        this.storage = []
        // 初始化数据数量
        this.count = 0
        // 初始化数组总长度
        this.limit = 7
    }

    // 链地址法
    // 哈希函数
    // string => hashCode => 压缩到数组范围(size)内
    hashFunc(str, size) {
        // 1. 初始化 hashCode
        let hashCode = 0
        // 2. 霍纳算法计算 hashCode 的值
        // charCodeAt() => str => Unicode 编码
        for (let i = 0; i < str.length; i++) {
            hashCode = 37 * hashCode + str.charCodeAt(i)
        }
        // 3. 取余操作
        let index = hashCode % size
        return index
    }

    // 插入和修改操作
    put(key, value) {
        // 1. 根据 key 获取相对应的 index
        let index = this.hashFunc(key, this.limit)
        // 2. 根据 index 取出对应的 bucket => 每个 index 的链条 => 数组
        // 3. 判断 bucket 是否为空
        let bucket = this.storage[index] || []
        this.storage[index] = bucket
        // 4. 是否是修改数据
        for (let i = 0; i < bucket.length; i++) {
            let tuple = bucket[i]
            if (tuple[0] === key) {
                tuple[1] = value
                return true
            }
        }
        // 添加操作
        bucket.push([key, value])
        this.count++
        // 判断是否需要扩容
        let newLimit = this.limit * 2
        let newPrime = this.getPrime(newLimit)
        if (this.count > this.limit * 0.75) this.resize(newPrime)
    }

    // 获取元素
    get(key) {
        // 1. 根据 key 获取相对应的 index
        let index = this.hashFunc(key, this.limit)
        // 2. 根据 index 取出对应的 bucket => 每个 index 的链条 => 数组
        let bucket = this.storage[index]
        // 3. 判断 bucket 是否为空
        if (bucket === undefined) return null
        // 4. 有 bucket 线性查找
        for (let i = 0; i < bucket.length; i++) {
            let tuple = bucket[i]
            if (tuple[0] === key) {
                return tuple[1]
            }
        }
        // 5. 依然没有找到 返回 null
        return null
    }

    // 删除元素
    remove(key) {
        // 1. 根据 key 获取相对应的 index
        let index = this.hashFunc(key, this.limit)
        // 2. 根据 index 取出对应的 bucket => 每个 index 的链条 => 数组
        let bucket = this.storage[index]
        // 3. 判断 bucket 是否为空
        if (bucket === undefined) return null
        // 4. 有 bucket 删除
        for (let i = 0; i < bucket.length; i++) {
            let tuple = bucket[i]
            if (tuple[0] === key) {
                bucket.splice(i, 1)
                this.count--

                // 缩小容量
                let newLimit = Math.floor(this.limit / 2)
                let newPrime = this.getPrime(newLimit)
                if (this.limit > 7 && this.count < this.limit * 0.25) {
                    this.resize(Math.floor(newPrime)) > 7 ? this.resize(Math.floor(newPrime)) : this.resize(Math.floor(7))
                }
                return tuple[1]
            }
        }
        // 5. 依然没有找到 返回 null
        return null
    }

    // 判断哈希表是否为 null
    isEmpty() {
        return this.count === 0
    }

    // 获取哈希表中元素的个数
    size() {
        return this.count
    }

    // 哈希表改变大小
    resize(newLimit) {
        // 1. 保存旧的数组内容
        let oldStorage = this.storage
        // 2. 重置所有的属性
        this.storage = []
        this.count = 0
        this.limit = newLimit
        // 3. 遍历 oldStorage 中所有的 bucket
        for (let i = 0; i < oldStorage.length; i++){
            // 3.1 取出对应的 bucket
            let bucket = oldStorage[i]
            // 3.2 判断 bucket 是否为 undefined
            if (bucket === undefined) continue
            // 3.3 bucket 有数据 取出数据 重新插入
            for (let i = 0; i < bucket.length; i++){
                let tuple = bucket[i]
                this.put(tuple[0], tuple[1])
            }
        }
    }

    // 判断某个数是否是质数
    // 一个数若可以进行因数分解，那么分解时得到的两个数一定是一个小于等于sqrt(n)，一个大于等于sqrt(n)
    isPrime(num) {
        // 获取 num 平方根
        let temp = parseInt(Math.sqrt(num))
        // 循环判断
        for (let i = 2; i <= temp; i++) {
            if (num % i === 0) return false
        }
        return true
    }

    // 获取质数的方法
    getPrime(num) {
        while (!this.isPrime(num)) {
            num++
        }
        return num
    }
}
let test = new HashMap()
test.put('age',18)
test.put('name', 'hidari')
test.put('sex', 'woman')
test.put('age', 20)
test.put('aaa', 'a')
test.put('bbb', 'b')
test.put('ccc', 'c')
test.put('ddd', 'd')
console.log(test)
console.log(test.get('123'))
test.remove('age')
test.remove('aaa')
test.remove('bbb')
test.remove('ccc')
test.remove('ddd')
console.log(test)
```
