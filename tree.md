# 树
![avatar](https://img-blog.csdn.net/20170319193912710?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
## 树的优点
1. 数组:
   1. 优点:
   数组的主要优点是根据下标值访问效率会很高
   但是如果我们希望根据元素来查找对应的位置，比较好的方式是先对数组进行排序，再进行二分查找
   缺点:
   需要先对数组进行排序，生成有序数组，才能提高查找效率
   另外数组在插入和删除数据时，需要有大量的位移操作(插入到首位或者中间位置的时候)，效率很低
2. 链表:
   1. 优点:
   链表的插入和删除操作效率都很高
   2. 缺点:
   查找效率很低，需要从头开始依次访问链表中的每个数据项，直到找到
   而且即使插入和删除操作效率很高，但是如果要插入和删除中间位置的数据，还是需要重头先找到对应的数据
3. 哈希表:
   1. 优点:
   哈希表的插入 / 查询 / 删除效率都是非常高的
   2. 缺点:
   空间利用率不高，底层使用的是数组，并且某些单元是没有被利用的
   哈希表中的元素是无序的，不能按照固定的顺序来遍历哈希表中的元素
   不能快速的找出哈希表中的最大值或者最小值这些特殊的值
4. 树结构：非线性结构

## 树的术语
1. 树的定义:
   - 树（Tree）: n（n≥0）个结点构成的有限集合。
       - 当n=0时，称为空树；
       - 对于任一棵非空树（n> 0），它具备以下性质：
       - 树中有一个称为“根（Root）”的特殊结点，用 r 表示；
       - 其余结点可分为m(m>0)个互不相交的有限集T1，T2，... ，Tm，其中每个集合本身又是一棵树，称为原来树的“子树（SubTree）”
       - 注意:
           - 子树之间不可以相交
           - 除了根结点外，每个结点有且仅有一个父结点；
           - 一棵N个结点的树有N-1条边。
![avatar](https://img-blog.csdn.net/20170319193924570?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![avatar](https://img-blog.csdn.net/20170319193959828?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
2. 树的术语:
   1. 结点的度（Degree）：结点的子树个数.
   2. 树的度：树的所有结点中最大的度数. (树的度通常为结点的个数N-1)
   3. 叶结点（Leaf）：度为0的结点. (也称为叶子结点)
   4. 父结点（Parent）：有子树的结点是其子树的根结点的父结点
   5. 子结点（Child）：若A结点是B结点的父结点，则称B结点是A结点的子结点；子结点也称孩子结点。
   6. 兄弟结点（Sibling）：具有同一父结点的各结点彼此是兄弟结点。
   7. 路径和路径长度：从结点n1到nk的路径为一个结点序列n1 , n2,… , nk, ni是 ni+1的父结点。路径所包含边的个数为路径的长度。
   8. 结点的层次（Level）：规定根结点在1层，其它任一结点的层数是其父结点的层数加1。
   9. 树的深度（Depth）：树中所有结点中的最大层次是这棵树的深度。
![avatar](https://img-blog.csdn.net/20170319194014149?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![avatar](https://img-blog.csdn.net/20170319194027141?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![avatar](https://img-blog.csdn.net/20170319194037946?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
3. 树的表示
 - 孩子-兄弟表示法
![avatar](https://img-blog.csdn.net/20170319194256603?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![avatar](https://img-blog.csdn.net/20170319194304551?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
   - 这个表示法的最大好处是它把一棵复杂的树变成了一棵二叉树
 ![avatar](https://img-blog.csdn.net/20170319194315692?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc21pbGVfZnJvbV8yMDE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
4. 二叉树：
 - 树中每个节点最多只能有两个字节点
 - 定义：二叉树可以为空, 也就是没有结点.
        若不为空，则它是由根结点和称为其左子树TL和右子树TR的两个不相交的二叉树组成。
 - 二叉树的五种形态：
![avatar](https://img-blog.csdnimg.cn/20190925000345427.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p4YzQ3,size_16,color_FFFFFF,t_7)
 - 二叉树的性质：
![avatar](https://img-blog.csdnimg.cn/20190925003606938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p4YzQ3,size_16,color_FFFFFF,t_70)
 - 满二叉树（完美二叉树）：
![avatar](https://img-blog.csdnimg.cn/20190925001205153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p4YzQ3,size_16,color_FFFFFF,t_70)
 - 完全二叉树：
![avatar](https://img-blog.csdnimg.cn/20190925002153130.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p4YzQ3,size_16,color_FFFFFF,t_70)
   - 完全二叉树性质：
![avatar](https://img-blog.csdnimg.cn/20190925002240346.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p4YzQ3,size_16,color_FFFFFF,t_70)
 - 二叉树适用的存储结构：
   - 顺序存储结构：只适合平衡树的存储
![avatar](https://img-blog.csdnimg.cn/20190925005718156.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p4YzQ3,size_16,color_FFFFFF,t_70)
 - 链表存储：除了完全二叉树、平衡二叉树或者满二叉树，其他的一般都用链式存储结构。
![avatar](https://img-blog.csdnimg.cn/20190925013545464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p4YzQ3,size_16,color_FFFFFF,t_70)
