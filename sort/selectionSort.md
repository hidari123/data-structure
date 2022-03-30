# 选择排序
![avatar](https://upload-images.jianshu.io/upload_images/1102036-c570275e3deb7504?imageMogr2/auto-orient/strip|imageView2/2/w/464/format/webp)
## 思路:
1. 选定第一个索引位置，然后和后面元素依次比较 
2. 如果后面的队员, 小于第一个索引位置的队员, 则交换位置 
3. 经过一轮的比较后, 可以确定第一个位置是最小的 
4. 然后使用同样的方法把剩下的元素逐个比较即可
 - 可以看出选择排序，第一轮会选出最小值，第二轮会选出第二小的值，直到最后

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

class SelectionSort extends ArrayList {
    constructor () {
        super()
    }
    // 从大到小
    selectionSort() {
        // 外层循环: 从第一个位置开始取出数据, 直到倒数第二个
        for (let min = 0; min < this.array.length-1; min++) {
            // 内层循环: 从 min+1 位置开始, 和后面的内容比较
            for (let i = min+1; i < this.array.length; i++) {
                if (this.array[i] > this.array[min]) {
                    [this.array[i], this.array[min]] = [this.array[min], this.array[i]]
                }
            }
        }
    }
}

// 初始化数据项
let list = new SelectionSort()

list.insert(3)
list.insert(6)
list.insert(4)
list.insert(2)
list.insert(11)
list.insert(10)
list.insert(5)

console.log(list.toString())
list.selectionSort()
console.log(list.toString())
```
![avatar](https://upload-images.jianshu.io/upload_images/1102036-71ccc1ed70cb90c8?imageMogr2/auto-orient/strip|imageView2/2/w/938/format/webp)

## 效率
1. 选择排序的比较次数:
 - 选择排序和冒泡排序的比较次数都是`N*(N-1)/2`, 也就是`O(N²)`
2. 选择排序的交换次数:
 - 选择排序的交换次数只有`N-1`次, 用大O表示法就是`O(N)`
所以选择排序通常认为在执行效率上是高于冒泡排序的
