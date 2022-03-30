# 插入排序
![avatar](https://upload-images.jianshu.io/upload_images/1102036-bd898c9bfd2f1540?imageMogr2/auto-orient/strip|imageView2/2/w/383/format/webp)
## 思路
1. 局部有序:
 - 插入排序思想的核心是局部有序. 什么是局部有序呢? 
 - 比如在一个队列中的人, 我们选择其中一个作为标记的队员. 这个被标记的队员左边的所有队员已经是局部有序的. 
 - 这意味着, 有一部门人是按顺序排列好的. 有一部分还没有顺序. 
2. 思路:
 - 从第一个元素开始，该元素可以认为已经被排序 
 - 取出下一个元素，在已经排序的元素序列中从后向前扫描 
 - 如果该元素（已排序）大于新元素，将该元素移到下一位置 
 - 重复上一个步骤，直到找到已排序的元素小于或者等于新元素的位置 
 - 将新元素插入到该位置后, 重复上面的步骤

## 封装实现
```js
// 封装一个 arrayList
class ArrayList {
    constructor () {
        this.array = []
    }

    insert(item) {
        this.array.push(item)
    }

    toString() {
        return this.array.join(' ')
    }
}

class insertionSortList extends ArrayList {
    constructor () {
        super()
    }
    insertionSort() {
        // 外层循环: 外层循环是从1位置开始, 依次遍历到最后
        for (let i = 1; i < this.array.length; i++) {
            // 记录选出的元素, 放在变量temp中
            let temp = this.array[i]
            let j = i
            // 内层循环: 内层循环不确定循环的次数, 最好使用while循环
            while (this.array[j - 1] > temp && j > 0) {
                this.array[j] = this.array[j - 1]
                j--
            }
            // 将选出的j位置, 放入temp元素
            this.array[j] = temp
        }
    }
}

// 初始化数据项
let list = new insertionSortList()

list.insert(3)
list.insert(6)
list.insert(4)
list.insert(2)
list.insert(11)
list.insert(10)
list.insert(5)

console.log(list.toString())
list.insertionSort()
console.log(list.toString())
```
 - 解析 
    - 代码序号1: 获取数组的长度. 
    - 代码序号2: 外层循环, 从1位置开始, 因为0位置可以默认看成是有序的了. 
    - 代码序号3: 记录选出的i位置的元素, 保存在变量temp中. i默认等于j 
    - 代码序号4: 内层循环 
        - 内层循环的判断j - 1位置的元素和temp比较, 并且j > 0. 
        - 那么就将j-1位置的元素放在j位置. 
        - j位置向前移. 
    - 代码序号5: 将目前选出的j位置放置temp元素

## 效率
1. 插入排序的比较次数:
 - 第一趟时, 需要的最多次数是1, 第二趟最多次数是2, 依次类推, 最后一趟是N-1次
 - 因此是1 + 2 + 3 + ... + N - 1 = N * (N - 1) / 2
 - 然而每趟发现插入点之前, 平均只有全体数据项的一半需要进行比较
 - 我们可以除以2得到 N * (N - 1) / 4. 所以相对于选择排序, 其他比较次数是少了一半的
2. 插入排序的复制次数:
 - 第一趟时, 需要的最多复制次数是1, 第二趟最多次数是2, 依次类推, 最后一趟是N-1次
 - 因此是1 + 2 + 3 + ... + N - 1 = N * (N - 1) / 2
 - 对于基本有序的情况 
 - 对于已经有序或基本有序的数据来说, 插入排序要好很多
 - 当数据有序的时候, while循环的条件总是为假, 所以它变成了外层循环中的一个简单语句, 执行N-1次 
 - 在这种情况下, 算法运行至需要N(N)的时间, 效率相对来说会更高
 - 另外别忘了, 我们的比较次数是选择排序的一半, 所以这个算法的效率是高于选择排序的
