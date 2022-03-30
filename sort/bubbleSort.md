# 冒泡排序
![avatar](https://upload-images.jianshu.io/upload_images/1102036-b8b1aca05ccf1b24?imageMogr2/auto-orient/strip|imageView2/2/w/404/format/webp)
## 思路
1. 对未排序的各元素从头到尾依次比较相邻的两个元素大小关系 
2. 如果左边的队员高, 则两队员交换位置 
3. 向右移动一个位置, 比较下面两个队员 
4. 当走到最右端时, 最高的队员一定被放在了最右边 
5. 按照这个思路, 从最左端重新开始, 这次走到倒数第二个位置的队员即可. 
6. 依次类推, 就可以将数据排序完成

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

class BubbleSortList extends ArrayList {
    constructor () {
        super()
    }

    // 从小到大
    bubbleSort() {
        // 反向循环, 因此次数越来越少
        for (let count = this.array.length; count >= 0; count--) {
            // 根据i的次数, 比较循环到i位置
            for (let i = 0; i < this.array.length; i++) {
                if (this.array[i] > this.array[i+1]) {
                    // 解构交换位置
                    [this.array[i], this.array[i+1]] = [this.array[i+1], this.array[i]]
                }
            }
        }
    }
}

// 初始化数据项
let list = new BubbleSortList()

list.insert(3)
list.insert(6)
list.insert(4)
list.insert(2)
list.insert(11)
list.insert(10)
list.insert(5)

console.log(list.toString())
list.bubbleSort()
console.log(list.toString())
```
![avatar](https://upload-images.jianshu.io/upload_images/1102036-143b34a96f876463?imageMogr2/auto-orient/strip|imageView2/2/w/1167/format/webp)

## 效率
1. 冒泡排序的比较次数:
 - 如果按照上面的例子来说, 一共有7个数字, 那么每次循环时进行了几次的比较呢? 
 - 第一次循环6次比较, 第二次5次比较, 第三次4次比较....直到最后一趟进行了一次比较. 
 - 对于7个数据项比较次数: 6 + 5 + 4 + 3 + 2 + 1 
 - 对于N个数据项呢? (N - 1) + (N - 2) + (N - 3) + ... + 1 = N * (N - 1) / 2

2. 冒泡排序的交换次数:
 - 冒泡排序的交换次数是多少呢? 
 - 如果有两次比较才需要交换一次 (不可能每次比较都交换一次) ，那么交换次数为N² / 4 
 - 由于常量不算在大O表示法中, 因此, 我们可以认为交换次数的大O表示也是O(N²)
