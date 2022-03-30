# 希尔排序

## 思路
1. 希尔排序是插入排序的一种高效的改进版, 并且效率比插入排序要更快
2. 插入排序的问题:
   - 假设一个很小的数据项在很靠近右端的位置上, 这里本来应该是较大的数据项的位置
   - 把这个小数据项移动到左边的正确位置, 所有的中间数据项都必须向右移动一位
   - 如果每个步骤对数据项都进行N次复制, 平均下来是移动N/2, N个元素就是 N*N/2 = N²/2
   - 所以我们通常认为插入排序的效率是O(N²)
   - 如果有某种方式, 不需要一个个移动所有中间的数据项, 就能把较小的数据项移动到左边, 那么这个算法的执行效率就会有很大的改进.
3. 希尔排序的做法:
   - 比如下面的数字, 81, 94, 11, 96, 12, 35, 17, 95, 28, 58, 41, 75, 15
   - 我们先让间隔为5, 进行排序. (35, 81), (94, 17), (11, 95), (96, 28), (12, 58), (35, 41), (17, 75), (95, 15)
   - 排序后的新序列, 一定可以让数字离自己的正确位置更近一步
   - 我们再让间隔位3, 进行排序. (35, 28, 75, 58, 95), (17, 12, 15, 81), (11, 41, 96, 94)
   - 排序后的新序列, 一定可以让数字离自己的正确位置又近了一步
   - 最后, 我们让间隔为1, 也就是正确的插入排序. 这个时候数字都离自己的位置更近, 那么需要复制的次数一定会减少很多
![avatar](https://upload-images.jianshu.io/upload_images/1102036-18698fe457fe8d02?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
4. 选择合适的增量:
   - 在希尔排序的原稿中, 他建议的初始间距是N / 2, 简单的把每趟排序分成两半.
   - 也就是说, 对于N = 100的数组, 增量间隔序列为: 50, 25, 12, 6, 3, 1.
   - 这个方法的好处是不需要在开始排序前为找合适的增量而进行任何的计算.
   - 我们先按照这个增量来实现我们的代码.

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

class shellSortList extends ArrayList {
    constructor () {
        super()
    }
    shellSort() {
        // 1.获取数组的长度
        let length = this.array.length
        // 2.根据长度计算增量
        let gap = Math.floor(length / 2)
        // 3.增量不断变量小, 大于0就继续排序
        while (gap >= 1) {
            // 4.实现插入排序
            for (let i = gap; i < length; i++) {
                // 4.1.保存临时变量
                let temp = this.array[i]
                let j = i
                // 4.2.插入排序的内层循环
                while (this.array[j-gap] > temp && j > gap - 1) {
                    this.array[j] = this.array[j-gap]
                    j -= gap
                }
                // 4.3.将选出的j位置设置为temp
                this.array[j] = temp
            }
            // 5.重新计算新的间隔
            gap = Math.floor(gap / 2)
        }
    }
}

// 初始化数据项
let list = new shellSortList()

list.insert(3)
list.insert(6)
list.insert(4)
list.insert(2)
list.insert(11)
list.insert(10)
list.insert(5)

console.log(list.toString())
list.shellSort()
console.log(list.toString())
```
- 解析
  - 代码序号1: 获取数组的长度 
  - 代码序号2: 计算第一次的间隔, 我们按照希尔提出的间隔实现. 
  - 代码序号3: 增量不断变小, 大于0就继续改变增量 
  - 代码序号4: 实际上就是实现了插入排序
     - 代码序号4.1: 保存临时变量, j位置从i开始, 保存该位置的值到变量`temp`中 
     - 代码序号4.2: 内层循环, `j > gap - 1`并且`temp`大于`this.array[j - gap]`, 那么就进行复制. 
     - 代码序号4.3: 将j位置设置为变量`temp`
  - 代码序号5: 每次while循环后都重新计算新的间隔

## 效率
- 希尔排序的效率很增量是有关系的
- 但是, 它的效率证明非常困难, 甚至某些增量的效率到目前依然没有被证明出来.
- 但是经过统计, 希尔排序使用原始增量, 最坏的情况下时间复杂度为O(N²), 通常情况下都要好于O(N²)
