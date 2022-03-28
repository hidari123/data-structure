# 红黑树
![avatar](https://images0.cnblogs.com/i/497634/201403/251730074203156.jpg)
## 特性
1. **节点是红色或黑色**
2. **根节点是黑色**
3. 每个**叶子节点**（NIL节点）都是**黑色的空节点** [注意：这里叶子节点，是指为空(NIL或NULL)的叶子节点！]
4. **每个红色节点两个子节点都是黑色**（**从每个叶子到根的所有路径上不能有两个连续的红色节点**）
5. 从**任一节点**到**每个叶子的所有路径**都包含**相同数目**的黑色节点

## 红黑树的相对平衡
1. 从**根到叶子**的**最长可能路径**，不会超过**最短可能路径**的**两倍长**
2. 结果就是这个树基本是**平衡**的
3. 虽然没有做到绝对的平衡，但是可以保证在最坏情况下，依然是高效的
4. 为什么可以做到**最长路径不超过最短路径的两倍**？
    1. **性质4**决定了路径不能有两个相连的红色节点
    2. 最短的可能路径都是黑色节点
    3. 最长的可能路径是红色和黑色交替
    4. **性质5**所有路径都有相同数目的黑色节点
    5. 这就表明了没有路径能多于任何其他路径的两倍长

## 红黑树的变换
插入一个新节点时，有可能树不再平衡，可以通过三种方式变换，让树保持平衡
1. 变色
   1. 为了重新符合红黑树的规则，尝试把**红色节点**变成**黑色**，或者把**黑色节点**变成**红色**
   2. 插入的**新的节点**都是**红色**
      1. 插入红色 => 可能插入一次是**不违反红黑树任何规则**的
      2. 插入黑色 => 导致一条路径上**多了黑色节点**
      3. 红色节点可能导致出现**红红相连**的情况 => 可以通过**颜色调换**和**旋转**调整
2. 旋转
    1. 左旋转 => 逆时针旋转
        - ![avatar](https://images0.cnblogs.com/i/497634/201403/251733282013849.jpg)
        - ![avatar](https://images0.cnblogs.com/i/497634/201403/251734577643655.jpg)
        - 使得父节点被自己的右孩子取代，而成为自己的左孩子 
    2. 右旋转 => 顺时针旋转
        - ![avatar](https://images0.cnblogs.com/i/497634/201403/251735527958942.jpg)
        - ![avatar](https://images0.cnblogs.com/i/497634/201403/251737465769614.jpg)
        - 使得父节点被自己的左孩子取代，而成为自己的右孩子 
    3. 区分左旋和右旋
        - 仔细观察上面"左旋"和"右旋"的示意图。我们能清晰的发现，它们是对称的。无论是左旋还是右旋，被旋转的树，在旋转前是二叉查找树，并且旋转之后仍然是一颗二叉查找树。
        - ![avatar](https://images0.cnblogs.com/i/497634/201403/251739385617803.jpg)
    4. 左旋示例图(以x为节点进行左旋)：
        -  ```
                                             z
              x                             /                  
             / \      --(左旋)-->           x
            y   z                         /
                                         y
           ``` 
        - 对x进行左旋，意味着，将“x的右孩子”设为“x的父亲节点”；即，将 x变成了一个左节点(x成了为z的左孩子)！因此，左旋中的“**左**”，意味着“**被旋转的节点将变成一个左节点**”。
    5. 右旋示例图(以x为节点进行右旋)：
       -  ```
                                        y
            x                            \                 
           / \      --(右旋)-->            x
          y   z                            \
                                            z
          ```
       - 对x进行右旋，意味着，将“x的左孩子”设为“x的父亲节点”；即，将 x变成了一个右节点(x成了为y的右孩子)！因此，右旋中的“**右**”，意味着“**被旋转的节点将变成一个右节点**”。

## 操作
### 插入
1. 要插入的节点 => N，父节点 => P，祖父节点 => G，父亲的兄弟节点 =>U
2. 情况1：
   1. 新节点N位于根节点上，没有父节点
   2. 直接将红色变成黑色
3. 情况2：
   1. 新节点P是黑色
   2. 性质4没有失效（新节点是红色），性质5也没有问题
   3. 尽管新节点有两个黑色的叶子节点NIL，但是新节点N是红色的，所以通过他的路径中黑色节点个数依然相同，满足性质5
4. （下面三种情况(Case)处理问题的核心思路都是：**将红色的节点移到根节点；然后，将根节点设为黑色**。下面对它们详细进行介绍。 ）
   1. 情况3：
      - ![avatar](https://images0.cnblogs.com/i/497634/201403/251759273578917.jpg)
      - P为红色，U也是红色
      - 操作方案：
        - (01) 将“父节点”设为黑色。
        - (02) 将“叔叔节点”设为黑色。
        - (03) 将“祖父节点”设为“红色”。
        - (04) 将“祖父节点”设为“当前节点”(红色节点)；即，之后继续对“当前节点”进行操作。
          - 现在新节点N有了一个黑色的父节点P，所以每条路径上黑色节点数目没有改变
          - 从而在更高的路径上，必然都会经过G节点，所以那些路径的黑色节点数目也是不变的，符合性质5
        - 可能出现的问题：
          - N的祖父节点G也可能是红色，违反性质3，可以递归调整颜色
          - 递归调整颜色到了根节点，需要进行旋转
   2. 情况4：
      - ![avatar](https://images0.cnblogs.com/i/497634/201404/170945094945387.jpg)
      - N的叔叔U是黑色节点，且当前节点N是左孩子
      - 操作方案：
        - (01) 将“父节点”设为“黑色”。
        - (02) 将“祖父节点”设为“红色”。
        - (03) 以“祖父节点”为支点进行右旋。
          - 对祖父节点G进行依次右旋转
          - 在旋转查收的树中，以前的父节点P现在是新节点以及以前的祖父节点G的父节点
          - 交换以前的父节点P和祖父节点G的颜色（P => 黑色，G => 红色，G原来一定是黑色）
          - N的兄弟节点B向右平移，成为G节点的左子节点
   3. 情况5：
      - ![avatar](https://images0.cnblogs.com/i/497634/201403/251801031546918.jpg)
      - N的叔叔U是黑色节点，且当前节点N是右孩子
      - 操作方案：
        - 对P节点进行依次左旋转，形成情况4的结果
        - 对父节点G进行依次右旋转，并改变颜色

## 案例
![avatar](https://upload-images.jianshu.io/upload_images/1102036-d791cd8b5c6066d7.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
![avatar](https://upload-images.jianshu.io/upload_images/1102036-5163c2303984aca3.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
![avatar](https://upload-images.jianshu.io/upload_images/1102036-cf9e6b85213d5c52.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
![avatar](https://upload-images.jianshu.io/upload_images/1102036-d763141e7a188d48.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
![avatar](https://upload-images.jianshu.io/upload_images/1102036-c621cc745ec51e4d.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
![avatar](https://upload-images.jianshu.io/upload_images/1102036-cf78539acf806248.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
![avatar](https://upload-images.jianshu.io/upload_images/1102036-88877b186cc160bd.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
![avatar](https://upload-images.jianshu.io/upload_images/1102036-daec955db3e90471.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
