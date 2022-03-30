# 快速排序
 - 快速排序几乎可以说是目前所有排序算法中, 最快的一种排序算法.
 - 当然, 没有任何一种算法是在任意情况下都是最优的, 比如希尔排序确实在某些情况下可能好于快速排序. 但是大多数情况下, 快速排序还是比较好的选择.

## 思想
1. 快速排序其实是我们学习过的最慢的冒泡排序的升级版
  - 我们知道冒泡排序需要经过很多次交换，才能在一次循环中，将最大值放在正确的位置
  - 而快速排序可以在一次循环中(其实是递归调用)找出某个元素的正确位置，并且该元素之后不需要任何移动 
2. 快速排序最重要的思想是分而治之.
   1. 比如我们下面有这样一顿数字需要排序:
     - 第一步: 从其中选出了65 (其实可以是选出任意的数字, 我们以65举例)
     - 第二步: 我们通过算法：将所有小于65的数字放在65的左边, 将所有大于65的数字放在65的右边. 
     - 第三步: 递归的处理左边的数据 (比如你选择31来处理左侧)，递归的处理右边的数据 (比如选择75来处理右侧, 当然选择81可能更合适)
     - 最终: 排序完成
   2. 和冒泡排序不同的是什么呢?
     - 我们选择的65可以一次性将它放在最正确的位置, 之后不需要任何移动
     - 需要从开始位置两个两个比较, 如果第一个就是最大值, 它需要一直向后移动, 直到走到最后
     - 也就是即使已经找到了最大值, 也需要不断继续移动最大值. 而插入排序对数字的定位是一次性的
![avatar](https://upload-images.jianshu.io/upload_images/1102036-30f0c7dfac490247?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

## 枢纽
1. 在快速排序中有一个很重要的步骤就是选取枢纽(pivot也称为主元)
   - 如何选择才是最合适的枢纽呢?
      - 一种方案是直接选择第一个元素作为枢纽
      - 但第一个作为枢纽在某些情况下, 效率并不是特别高
      - ![avatar](https://upload-images.jianshu.io/upload_images/1102036-0d1429cb140fcd0a?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
   - 另一种方案是使用随机数：
      - 随机取 pivot？但是随即函数本身就是一个耗性能的操作
   - 另一种比较优秀的解决方案: 取头、中、尾的中位数
      - 例如 8、12、3的中位数就是8

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

class quickSortList extends ArrayList {
    constructor () {
        super()
    }
    quickSort() {
        this.quickSortRec(0, this.array.length - 1)
    }

    // 内部递归
    quickSortRec(left, right) {
        // 0.递归结束条件
        if (left >= right) return
        // 1.获取枢纽
        let pivot = this.median(left, right)

        // 2.开始进行交换
        // 2.1.记录左边开始位置和右边开始位置
        let leftPointer = left
        let rightPointer = right - 1
        // 2.2.循环查找位置
        while (true) {
            while (this.array[++leftPointer] < pivot) {}
            while (this.array[--rightPointer] > pivot) {}
            if (leftPointer < rightPointer) {
                // 2.3.交换两个数值
                [this.array[leftPointer], this.array[rightPointer]] = [this.array[rightPointer], this.array[leftPointer]]
            } else {
                // 2.4.当 leftPointer < rightPointer 的时候(一定不会=, 看下面解释中的序号3), 停止循环因为两边已经找到了相同的位置
                break
            }
        }
        // 3.将枢纽放在正确的位置
        [this.array[leftPointer], this.array[right - 1]] = [this.array[right - 1], this.array[leftPointer]]
        // 4.递归调用左边
        this.quickSortRec(left, leftPointer - 1)
        this.quickSortRec(leftPointer + 1, right)
    }

    // 选择枢纽
    median(left, right) {
        // 1.求出中间的位置
        let middle = Math.floor((left + right) / 2)
        // 2.判断并且进行交换
        if (this.array[left] > this.array[right]) {
            [this.array[left], this.array[right]] = [this.array[right], this.array[left]]
        }
        // 是独立的判断，每个都要经过，所以不能写 else if
        if (this.array[middle] > this.array[right]) {
            [this.array[middle], this.array[right]] = [this.array[right], this.array[middle]]
        }
        if (this.array[left] > this.array[middle]) {
            [this.array[middle], this.array[left]] = [this.array[left], this.array[middle]]
        }

        // 3.巧妙的操作: 将center移动到right - 1的位置
        [this.array[middle], this.array[right - 1]] = [this.array[right - 1], this.array[middle]]
        // 4.返回pivot
        return this.array[right - 1]
    }
}

// 初始化数据项
let list = new quickSortList()

list.insert(3)
list.insert(6)
list.insert(4)
list.insert(2)
list.insert(11)
list.insert(10)
list.insert(5)

console.log(list.toString())
list.quickSort()
console.log(list.toString())
```
代码解析:
1. 这里有两个函数: quickSort和quickSortRec. 
   1. 外部调用时, 会调用quickSort 
   2. 内部递归时, 会调用quickSortRec
2. 我们这里主要讲解一下quickSortRec方法. 
   1. 代码序号0: 是递归的结束条件. 可以回头再来看这个函数. 
   2. 代码序号1: 从三个数中获取枢纽值, 这个方法我们在上一节中已经讲过, 这里不再累述. 
   3. 代码序号2: 我们的重点代码 
   4. 代码序号2.1: 循环交换合适位置的数值. 
   5. 代码序号2.2: 使用两个while循环, 递归的查找合适的i(大于枢纽的值)和合适的j(小于枢纽的值). 
   6. 代码序号2.3: 交换i和j位置的值. 
   7. 代码序号2.4: 当i<j的时候, 两边查找到了同一个位置, 这个时候停止循环. 
   8. 代码序号3: 刚才我们查找到的i位置正是pivot应该所在的位置, 和pivot替换即可. 
      1. 这里你可能会有一个疑问, 为什么将i位置可以换到最后呢? 万一它比pivot小呢? 
      2. 这是因为我们在while (this.array[++i] < pivot)先使用的是i, 而不是j. 但是这意味着什么呢? 
      3. 意味着i找到的一个值, 现在停下来的, 必然是大于pivot. 而j会超过i的位置向后找了一个小于pivot. 
      4. 但是, 这个时候已经不需要继续进行交换了, 直接退出即可. 
      5. 而退出后, i位置的数值是大于pivot, 所以可以将其换到后面.
   9. 代码序号4: 递归调用该函数, 将left, i - 1传入就是左边排序, 将i + 1, right就是右边排序.

## 效率
1. 快速排序的最坏情况效率
   - 什么情况下会有最坏的效率呢? 就是每次选择的枢纽都是最左边或者最后边的. 
   - 那么效率等同于冒泡排序. 
   - 而我们的例子可能有最坏的情况吗? 是不可能的. 因为我们是选择三个值的中位值.
2. 快速排序的平均效率:
   - 快速排序的平均效率是O(N * logN). 
   - 虽然其他某些算法的效率也可以达到O(N * logN), 但是快速排序是最好的.
