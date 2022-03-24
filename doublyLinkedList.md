# 双向链表

## 常见操作
1. `append(element)`：向链表尾部添加一个新的项
2. `insert(position, element)`：链表指定位置插入一个新的项
3. `get(position)`：获取对应位置的元素
4. `indexOf(element)`：返回元素在链表中的索引，如果链表中没有该元素则返回`-1`
5. `update(position, element)`：修改某个位置的元素
6. `removeAt(position)`：从列表的特定位置移除一项
7. `remove(element)`：从列表中移除一项
8. `isEmpty()`：如果链表中不包含任何元素，返回`true`，如果链表长度大于`0`则返回`false`
9. `size()`：返回链表包含的元素个数，与数组`length`属性类似
10. `toString()`：由于列表使用了`Node`类，需要重写继承自JavaScript对象的默认`toString`方法，让其只输出元素的值
11. `forwardString()`: 返回正向遍历的节点字符串形式
12. `backwardString()`: 返回反向遍历的节点字符串形式
## 封装
