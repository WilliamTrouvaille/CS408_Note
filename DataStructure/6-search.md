# 第七章 查找

## 导读

### 【考纲内容】

1. 查找的基本概念
2. 顺序查找法
3. 分块查找法
4. 折半查找法
5. 树型查找
    + 二叉搜索树
    + 平衡二叉树
    + 红黑树
6. $B$树及其基本操作、$B+$树的基本概念
7. 散列($Hash$)表
8. 查找算法的分析及应用

### 【知识导图】

![23July20-203847-1689856727-fe8c4525-932c-409e-b126-e6300a037bed](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202038250.png)

### 【复习提示】

+ ==本章是考研命题的重点==
+ 对于折半查找，应掌握折半查找的过程、构造判定树、分析平均查找长度等
+ 对于二叉排序树、二叉平衡树和红黑树，要了解它们的概念、性质和相关操作等
+ $B$树和$B+$树是本章的难点
    + 对于$B$树，考研大纲要求掌握插入、删除和查找的操作过程
    + 对于$B+$树，仅要求了解其基本概念和性质
+ 对于散列查找，应掌握散列表的构造、冲突处理方法（各种方法的处理过程)、查找成功和查找失败的平均查找长度、散列查找的特征和性能分析。

## 基本概念

![image-20230723104749498](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231047769.png)

+ 查找：在数据集合中寻找满足某种条件的数据元素的过程。

+ 查找表（查找结构）：用于查找的数据集合，由同一类型的数据元素或记录组成。
    + 查找表的常见操作
        1.   查找符合条件的数据元素
        2.   插入、删除某个数据元素

+ 关键字：数据元素中唯一标识该元素的某个数据项的值，使用基于关键字的查找，查找结果应该唯一。

+ 静态查找表：只需要进行操作`1.`
    + 顺序查找、折半查找、散列查找

+ 动态查找表：除了进行操作`1.`还要进行操作`2.`，同时还要考虑插入删除操作是否方便
    + 二叉查找树查找、散列查找

+ 查找长度：查找运算中，需要对比关键字的次数。

+ 平均查找长度$ASL(Average \; Search \;Length)$：所有查找过程中进行关键字比较次数的平均值
    $$
    ASL=\sum_{i=1}^nP_iC_i
    $$
    
    + 其中$P_i$表示查找第$i$个元素的概率，$C_i$表示查找第$i$个元素的查找长度。
    
    + 通常认为查找任何一个元素的概率都相同，即
        $$
        P_i=\frac{1}{n}
        $$
        

## 线性表查找

![image-20230723104833435](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231048285.png)

### 顺序查找

又称为线性查找，常用于线性表，从头到尾逐个查找。

#### 一般查找

在算法实现时一般将线性表的$0$号索引的元素值设为查找的值，从表最后面开始向前查找，当没有找到时就直接从$0$返回。这个数据称为哨兵，可以避免不必要的判断线性表长是否越界语句从而提高程序效率。

$ASL_{成功}$为$\dfrac{1+2+3+\cdots+n}{n}=\dfrac{n+1}{2}$

$ASL_{失败}$为$n+1$

时间复杂度为$O(n)$。

![23July20-200034-1689854434-cafb4f95-3223-48b7-81b7-23f7c0659429](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202000659.png)

#### 优化-有序数据

+   若在查找之前就已经知道表是关键字有序的，则查找失败时可以不用再比较到表的另一端就能返回查找失败的信息，从而降低顺序查找失败的平均查找长度。 
+   这样顺序结构从逻辑上就变成了一个二叉树结构（一般是顺序的）
    +   其中，左子树(图中矩形结点)代表小于
        +   即==失败结点==
        +   失败结点是虚构的空结点，实际上是不存在的
        +   所以到达失败结点时所查找的长度等于它上面的一个圆形结点的所在层数

    +   右子树代表大于
        +   需要继续向右查找

    +   所有数据结点挂在右子树上


![image-20230723110117127](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231101221.png)

+   ==对于顺序查找，无论顺序表还是乱序表，查找成功的时间和$ASL$都是相同的==
    +   一个成功结点的查找长度=自身所在层数
    +   失败结点的查找长度=其父结点所在层数
+   $ASL$查找失败为$\dfrac{1+2+3+\cdots+n+n}{n+1}=\dfrac{n}{2}+\dfrac{n}{n+1}$。成功结点的查找长度等于自身所在层数，失败结点的查找长度等于其父结点所在层数。
    +   若有$n$个结点，则有$n+1$个查找失败结点。
    +   但是失败结点都是不存在的，如果结点失败只会在一个不存在的区间上，所以查找平均概率是$\dfrac{1}{n+1}$而不是$\dfrac{1}{2n+1}$
        +   同理，查找成功查找失败都会在那个区间停下，一旦判断所在区域不存在就会退出
        +   所以查找失败的次数跟最坏查找成功次数一样都是$n$查完所有。

    +   查找情况还要比普通乱序查找加上一个大于最大值的情况。


>   注意，有序线性表的顺序查找和后面的折半查找的思想是不一样的（有序顺序查找是从两端开始向右比较，折半查找从中间开始不断取中间值比较）。且有序线性表的顺序查找中的线性表可以是链式存储结构。

#### 优化-不等概率

当数据元素查找概率不等时，可以将查找概率更大的元素放在靠前的位置，以减少大概率元素被遍历的时间。

$ASL$查找成功为$\sum\limits_{i=1}^nP_ii$，$P_i$为第$i$个元素出现概率。

但是此时数据是乱序的，所以查找失败的时间复杂度与没有优化的是一样的为$O(n)$。

### 折半查找

也称为二分查找，==只适用于有序的顺序表==

==**链表无法适用**，因为链表不具备随机存储特性==

#### 折半查找的结构

+   定义三个指针
    +   $low$指向查找范围的最小值
    +   $high$指向查找范围的最大值
    +   $mid$指向查找范围的中间值
        +   $mid=\lfloor(low+high)\div2\rfloor$
        +   也可以向上取整，过程会有所不同
        +   向下取整可以直接利用左移一位`mid= (low+high) <<1;`

#### 折半查找的过程

![23July20-202218-1689855738-da92d858-76a7-4d6b-a6b3-52f07efaae5f](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202022708.png)

1. 初始化
    1. 定义左边界$low$，默认为$0$
    2. 右边界$high$，默认为$length-1$

2. 循环执行折半查找(以下两步)
    1. 计算出$mid=\lfloor\dfrac{(low+high)}{2}\rfloor$。
    2. 判断中间索引值$data\lbrack mid\rbrack$是否与搜索值$target$相等。
        +   若$data\lbrack mid\rbrack=target$
            +   查找成功
            +   返回中间索引。
        +   若$data\lbrack mid\rbrack<target$
            +   说明查找数据在右半部分
            +   则将$low=mid+1$
        +   若$data\lbrack mid\rbrack>target$
            +   说明查找数据在左半部分
            +   则将$high=mid-1$

3. 当查找最后$low>high$则查找失败。

#### 折半公式优化

在对$mid$进行取值时，如果数据量太大，查找到右侧时计算$mid$进行两数相加$low+high$可能会数值溢出。那么如何解决？

+ 变幻公式：$(low+high)\div2\rightarrow low\div2+high\div2$或$\rightarrow low-(low\div2-high\div2)\rightarrow low+(high-low)\div2$。
+ 无符号右移运算：$mid=(low+hight) >> 1$。直接将除以$2$变为右移运算，速度更快，且舍去了小数位不需要进行取整运算。

>   使用`std::vector.at(index)`就行,`at(index)`可以带有边界检查地访问` vector `中的元素

#### 折半查找判定树

==折半查找判定树不是完全二叉树!==

折半查找的过程可用二叉树来描述，称为判定树。树中每个圆形结点表示一个记录，结点中的值为该记录的关键字值；树中最下面的叶结点都是方形的，它表示查找不成功的情况。从判定树可以看出，查找成功时的查找长度为从根结点到目的结点的路径上的结点数，而查找不成功时的查找长度为从根结点到对应失败结点的父结点的路径上的结点数。

![23July23-122655-1690086415-2d035f74-f549-49aa-9617-24220912f27e](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231227722.png)

构建判定树方式：

+ 若当前$low$和$high$之间有奇数个元素，则$mid$分割后左右两部分元素个数相等。
+ 若当前$low$和$high$之间有偶数个元素，则$mid$分割后左部分元素个数小于右部分一个。
    + 右子树结点数-左子树结点数=0或1
    + 即==右子树结点数最多比左子树结点数多一个==

+ 结点索引从$0$或$1$开始，相加除以二后向下取整，并且两端要移动一位，否则会有问题。
+ 数值结点为圆形，末端结点后再加一层方形的叶子结点，表示查找失败，失败的叶子结点是虚拟的，所在层数跟上层圆形结点是一样的，所以在计算查找长度时该点层数为其父结点成功结点的层数。

判定树性质：

+ 由$mid=\lfloor\dfrac{(low+high)}{2}\rfloor$可知，==右子树结点数-左子树结点数$=0$或$1$==

+ 折半查找判定树一定是一个**平衡二叉树**

+ 折半查找判定树也是一个二叉查找树

    + 左<中<右

+ 失败结点$=n+1$

    + 即成功结点的空链域结点数

+ 只有最下面一层不满，==元素个数为$n$时树高与完全二叉树相等$h=\lceil\log_2(n+1)\rceil$。==
    + 向上取整
    + 树高不包含失败结点

+ 根据折半查找判定树可以计算对应的$ASL$

    + 查找成功的$ASL$=($\sum\limits_{i=1}^n$第$i$层的成功结点数$\times i$)$\div$​成功结点总数
        $$
        ASL_{成功}=\frac{1}{n} \sum_{i=1}^{n} l_{i}=\frac{1}{n}\left(1 \times 1+2 \times 2+\cdots+h \times 2^{h-1}\right)=\frac{n+1}{n} \log _{2}(n+1)-1 \approx \log _{2}(n+1)-1
        $$
    + 查找失败的$ASL$=($\sum\limits_{i=1}^n$第$i$层的失败结点数$\times i$)$\div$失败结点总数。

+ ==折半查找判定树的中序序列应该是一个有序序列。==

[性能分析]:

+   $ASL$查找成功查找失败都一定小于折半查找树的树高，时间复杂度为$O(\log_2n)$。

+ 查找的折半查找判定树和排序的二叉查找树都是同样的二叉逻辑结构
    + 但是不同的是折半查找判定树是已知完整序列，所以总是从中间开始，时间性能为固定的$O(\log_2n)$
    + 而二叉查找树的构造是根据输入来的，如果输入的序列正好是从中间切分的，则时间性能为最好的$O(\log_2n)$
    + 如果输入的序列恰好有序，则为单枝树，时间性能为最坏的$O(n)$。

### 分块查找

+   分块查找又称为索引顺序查找，它吸取了顺序查找和折半查找各自的优点，既有动态结构，又适于快速查找。 
+   需要对数据进行一定的排序，不一定全部是顺序的，但是要求在一个区间内是满足一定条件的，即块内无序，块间有序
    +   即$n$块内的元素全部小于$n+1$块内的任意元素。
    +   将查找表分割为若干子块，其中分割的块数和每块里的数据个数都是不定的。

#### 分块查找结构

除了数据，再建立一个索引表，索引表中的每个元素含有各块的最大关键字和各块中的第一个元素的地址，索引表按关键字有序排列

![23July20-204239-1689856959-db15c085-0aea-4ab5-aafc-8b036597cefa](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202042338.png)

+   很明显这种定义方式定义了两个顺序结构，并且如果插入删除时需要大量移动元素，==所以可以采用链表的形式。==
    +   对链表进行查找时，
    +   定义一种最大元素结点，包含数据、指向后继最大元素结点的指针、指向分块内元素的指针
    +   定义一种块内元素结点，包含数据、指向后继分块内元素的指针
    +   但是这时候就无法折半查找，只能顺序查找。
+   所以总的来说分块查找还是一种静态查找，动态插入删除的效率较低。

#### 分块查找过程

在查找时先根据关键字遍历索引表，然后找到索引表的分块（可以顺序也可以折半），再到存储数据的顺序表的索引区间中查找。

若使用折半查找查找索引表的分块，若索引表中不存在目标关键字，则折半查找索引表最终会停在$low>high$，要在$low$所指向分块中查找。

![23July20-204805-1689857285-1a5a47c7-ad4b-4004-b5ca-3fd97a9d9ac0](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202048564.png)

+   原因
    +   在分块查找中，索引表是由每个块的最大关键字构成的，也称为块首元素
    +   通过折半查找，我们可以快速定位目标关键字所在的块，然后再在该块内使用顺序查找来找到具体的目标关键字。
    +   但是由于块首元素是目标关键字所在的块中最大元素,所以块首元素大于等于目标关键字
    +   所以$low>high$

#### 分块查找的效率

$ASL$查找成功失败的情况都十分复杂，所以一般不会考。

+   假设长度为$n$的查找表被均匀分为$b$块，每块$s$个元素
    +   假设索引查找和块内查找的平均查找长度$ASL$分别为$L_I$和$L_S$，则分块查找的平均查找长度为$ASL=L_I+L_S$。

+   使用顺序查找索引表
    +   则$L_I=\dfrac{(1+2+\cdots+b)}{b}=\dfrac{b+1}{2}$，$L_S=\dfrac{(1+2+\cdots+s)}{s}=\dfrac{s+1}{2}$
    +   所以$ASL=\dfrac{b+1}{2}+\dfrac{s+1}{2}=\dfrac{s^2+2s+n}{2s}$
        +   当$s=\sqrt{n}$时，$ASL_{min}=\sqrt{n}+1$。

+   使用折半查找索引表
    +   则$L_I=\lceil\log_2(b+1)\rceil$，$L_S=\dfrac{(1+2+\cdots+s)}{s}=\dfrac{s+1}{2}$
    +   所以$ASL=\lceil\log_2(b+1)\rceil+\dfrac{s+1}{2}$。

>   注：分块查找的时间复杂度
>
>   1. 在块内进行顺序查找：
>   顺序查找是在块内进行的查找方法。它需要逐个遍历块内的元素，直到找到目标关键字或遍历完整个块。因此，顺序查找的时间复杂度为$O(m)$，其中$ m $是块的大小（块内元素个数）。
>
>   2. 在索引表中进行折半查找：
>   折半查找是在索引表中进行的查找方法。索引表是有序的，通过折半查找可以快速定位目标关键字所在的块。折半查找的时间复杂度为$ O(\log_{2} n)$，其中$ n $是索引表的大小（索引表的元素个数）。
>
>   综合考虑分块查找的过程，首先使用折半查找在索引表中定位目标关键字所在的块，时间复杂度为$ O(\log_{2} n)$，然后再在定位到的块内使用顺序查找来找到具体的目标关键字，时间复杂度为 $O(m)$。因此，分块查找的总体时间复杂度为 $O(log_{2} n + m)$。
>
>   值得注意的是，分块查找适用于某些特定场景，当块的大小$ m $和索引表的大小$ n $合适时，分块查找可以在一定程度上提高查找效率。当数据量较大且块的大小和索引表的大小不合适时，分块查找的性能可能会受到影响，可能需要考虑其他更适合的查找算法。

## 二叉查找

![image-20230723104935226](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231049393.png)

### 二叉查找树

即$BST$，是一种用于排序的二叉树。

#### 二叉查找树定义

==二叉查找树，也称二叉排序树==，或者是一棵空树，或者是具有下列特性的二叉树

1.   若左子树非空，左子树上所有结点的关键字均小于根结点的关键字
2.   若右子树非空，右子树上所有结点的关键字均大于根结点的关键字
3.   左右子树又各是一棵二叉查找树。

+   ==中序遍历二叉查找树会得到一个递增的有序序列。==
    +   因为==左子树结点值 < 根结点值 < 右子树结点值==

>    二叉查找树中不存在两个结点具有相同的值

#### 二叉查找树查找

![23July20-211022-1689858622-ce258ee8-5c3e-422a-aa2b-9360487a5f14](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202110137.png)

[实现思路]:

1. 若树非空，目标值与根结点的值比较。
2. 若相等则查找成功，返回结点指针。
3. 若小于根结点，则在左子树上查找，否则在右子树上查找。
4. 遍历结束后仍没有找到则返回$NULL$。

[性能分析]:

+   时间复杂度
    +   递归查找的时间复杂度是$O(\lceil\log_2(n+1)\rceil)$
        +   其中$\lceil\log_2(n+1)\rceil$代表二叉树的高度。
    +   遍历查找的时间复杂度是$O(\log_2n)$
+   查找成功的平均查找长度$ASL$
    +   二叉树的平均查找长度为$O(\log_2n)$
    +   最坏情况是每个结点只有一个分支，平均查找长度为$O(n)$。

#### 二叉查找树插入及其构造

![23July23-132158-1690089718-d6e830fd-6583-454a-bead-bf1c7ba15cfb](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231322519.png)

+ 若原二叉查找树为空，就直接插入结点。
+ 否则，若关键字小于根结点值，插入左结点树。
+ 若关键字大于根结点值，插入右结点树。

![23July20-211426-1689858866-17d56254-3cf9-4efa-bd58-d43bd25c7193](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202114758.png)

+   从一棵空树出发，依次输入元素，将它们插入二叉排序树中的合适位置，即可构造一颗二叉排序树

![23July23-132321-1690089801-a6ca4fdc-623c-486e-877f-b5abba289a53](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231323155.png)

#### 二叉查找树删除

在二叉排序树中删除一个结点时，不能把以该结点为根的子树上的结点都删除，必须先把被删除结点从存储二叉排序树的链表上摘下，将因删除结点而断开的二叉链表重新链接起来，同时确保二叉排序树的性质不会丢失

删除操作的实现过程按$3$种情况来处理：

1.   若被删除结点$p$是叶子结点
     +   则直接删除，不会破坏二叉查找树的结构。
2.   若被删除结点只有一棵左子树或右子树
     +   则让该结点的子树称为该结点父结点的子树，来代替其的位置。
3.   若被删除结点有左子树和右子树
     +   则让其结点的直接后继或直接前驱替代该结点
         +   直接后继：中序排序该结点后一个结点，其右子树的最左下角结点，不一定是叶子结点
         +   直接前驱：中序排序该结点前一个结点，其左子树的最右下角结点，不一定是叶子结点
     +   并从二叉查找树中删除该结点的直接后继、直接前驱，这就变成了第一种或第二种情况。

![image-20230723132656538](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231326684.png)

二叉查找树删除或插入时得到的二叉查找树往往与原来的不同。

#### 二叉查找树查找效率

+   二叉查找树的查找效率主要取决于树的高度
    +   若是平衡二叉树，则平均查找长度为$O(\log_2n)$
    +   若最坏情况下只有一个单枝树，则平均查找长度为$O(n)$。
+   若按顺序输入关键字则会得到单枝树。
+   ==静态查找时使用顺序表进行二分查找，而动态查找时使用二叉查找树。==
    +   查找过程来看，二叉查找树和二分查找类似
        +   但是二分查找的判定树唯一
        +   而二叉查找树的查找不唯一，相关关键字插入顺序会极大影响到查找效率。

    +   从维护来看
        +   二叉查找树插入删除操作只需要移动指针，时间代价为$O(\log_2n)$
        +   而二分查找的对象是有序顺序表，代价是$O(n)$


### 平衡二叉树

为了限制判定树高增长过快，降低二叉查找树的性能，规定插入时要保证平衡。

平衡二叉树，即$AVL$树，树上任意一结点的左子树和右子树的高度之差不超过$1$。

树高即树的深度。

结点的平衡因子=左子树高-右子树高。

即平衡二叉树的平衡因子只可能为$-1,0,1$。

#### 平衡二叉树结点

$h$为平衡二叉树高度，$n_h$为构造此高度的平衡二叉树所需的最少结点数。

平衡二叉树最多结点数$2^h-1$。即该二叉树为满二叉树。

<span style="color:orange">注意：</span>平衡二叉树最少结点数（所有非叶结点的平衡因子均为$1$的）的递推公式为$n_0=0$，$n_1=1$，$n_2=2$，$n_h=1+n_{h-1}+n_{h-2}$。此时所有非叶结点的平衡因子均为$1$。

+   <span style="color:red">证明：</span>假设$T$为高度为$h$的平衡二叉树，其需要最少的结点数目为$F(h)$。
+   又假设$TL$，$TR$为$T$的左右子树，因此$TL$，$TR$也为平衡二叉树。
+   假设$FL$、$FR$为$TL$，$TR$的最少结点数，则$F(h)=FL+FR+1$。那么$FL$、$FR$到底等于多少呢？
+   由于$TL$，$TR$与$T$一样是平衡二叉树，又由于我们知道$T$的最少结点数是$F(h)$，其中$h$为$T$的高度，因此如果我们知道$TL$，$TR$的高度就可以知道$FL$、$FR$的值了。
+   由平衡二叉树的定义可以知道，$TL$和$TR$的高度要么相同，要么相差$1$，而当$TL$与$TR$高度相同（即都等于$h-1$）时，我们算出来的$F(h)$并不能保证最小，两边都是$h-1$明显比只有一边$h-2$的结点数更多。
+   因此只有当$TL$与$TR$高度相差一，即一个高度为$h-1$，一个高度为$h-2$时，计算出来的$F(h)$才能最小。
+   此时我们假设$TL$比$TR$高度要高$1$，即$TL$高度为$h-1$，$TR$高度为$h-2$，则有$F1=F(h-1)$，$F2=F(h-2)$。
+   ==因此得到结论：$F(h)=F(h-1)+F(h-2)+1$。==

![23July20-220804-1689862084-66be1a46-9980-4048-ab19-ac2fc1460a8a](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202208025.png)

>   图源《An algorithm for the organization of information》--- G. M. Adelson-Velsky 和 Evgenii Landis
>
>   平衡二叉树提出原文献

#### 平衡二叉树插入

+   插入新结点后，要保持二叉排序树的特性不变
    +   ==左<中<右==
+   若插入新结点导致不平衡，则需要调整平衡（具体调整方法见下）
    +   $LL$
    +   $RR$
    +   $LE$
    +   $RL$
+   调整平衡时最重要的是根据二叉查找树的大小关系算出从大到小的序列，然后把最中间的作为新根，向两侧作为左右子树。
+   即先找到插入路径上离插入结点最近的平衡因子的绝对值大于$1$的结点$A$，再对以$A$为根的子树，在保持二叉查找树特性的前提下调整各结点位置。
+   在插入一个结点时，查找路径上的所有结点都可能收到影响。
+   从插入点往回（从下往上）找到第一个不平衡的结点，调整以该结点为根的子树。

![23July20-212713-1689859633-0e3c3919-6737-4ed9-b052-4b555aac3b35](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202127361.png)

+   ==每次调整的都是最小不平衡子树。==

+   只要找到“最小不平衡子树”调整好后整棵树都会恢复平衡
    +   插入操作导致“最小不平衡子树”高度+1，经过调整后高恢复

#### 平衡二叉树删除

+   删除新结点后，要保持二叉排序树的特性不变
    +   ==左<中<右==
+   平衡二叉树和二叉查找树的删除操作情况一致，都分为四种情况：
    1.   删除叶子结点
    2.   删除的结点只有左/右子树
    3.   删除的结点既有左子树又有右子树
+   **平衡二叉树的删除结点步骤**
    1.   删除结点
         1.   删除叶子结点
              +   直接删除即可
         2.   删除的结点只有左/右子树
              +   用删除结点的子树顶替位置
         3.   删除的结点既有左子树又有右子树
              +   用删除结点的前驱/后继结点顶替
              +   并转换为前驱/后继结点的删除
    2.   从删除的结点开始向上寻找最小不平衡子树
         +   如果没有找到说明不存在不平衡，不需要调整
    3.   根据最小不平衡子树类型调整平衡二叉树
    4.   若不平衡向上传导继续`2.`
+   与插入操作类似，都是需要从下往上进行调整。

+   不同的是插入操作只对子树进行调整，而删除操作可能要对整个树进行调整。

+   在任意一棵非空二叉查找树$T_1$中，删除某结点$v$之后形成二叉查找树$T_2$，再将$v$插入$T_2$，形成二叉查找树$T_3$。
    + 若$v$是$T_1$的叶结点，则$T_1$与$T_3$相同。
    + 若$v$不是$T_1$的叶结点，则$T_1$与$T_3$不同（如果是一定不同则是错误的）


#### $LL-$右单旋转

从结点的左孩子的左子树中插入导致不平衡：

<img src="https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202143730.png" alt="image-20230720214323604" style="zoom:50%;" />

+   二叉排序树的特性：左子树结点值<根结点值<右子树结点值
    +   $BL<B<BR<A<AR$

由于在结点$A$的左孩子$B$的的左子树$BL$上插入了新结点，$A$的平衡因子由$1$变成了$2$，导致以$A$为根的子树失去了平衡，需要进行一次向右的旋转操作：

1.   将$A$的左孩子$B$向右上旋转代替$A$成为根结点
2.   将$A$结点向右下旋转为成$B$的右子树的根结点
3.   而$B$的原右子树则作为$A$结点的左子树。

![23July20-214911-1689860951-ac43862d-0a4e-4b65-ab33-13d6e33ba620](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202149154.png)

[实现思路]:

1.   `f->lchild=p->rchild;`
     +   让结点$A$的左子树指向$B$的右子树$BR$
2.   `p->rchild=f;`
     +   让结点$B$的右子树变为$A$
3.   `gf->lchild/rchild=p;`
     +   让结点$A$的原双亲`gf`的左/右孩子指向$B$

<video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July20-221932-1689862772-16ef0a4f-2da0-4155-8e86-dd4331461e40.mp4"></video>

#### $RR-$左单旋转

从结点的右孩子的右子树中插入导致不平衡：

![23July20-214437-1689860677-1fbe7b7f-af8d-4ccb-9e4d-8a0dd348a379](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202144647.png)

+   二叉排序树的特性：左子树结点值<根结点值<右子树结点值	
    +   $AL<A<BL<B<BR$

由于在结点$A$的右孩子$R$的右子树$R$上插入了新结点，$A$的平衡因子由$-1$减至$-2$，导致以$A$为根的子树失去平衡，需要一次向左的旋转操作：

1.   将$A$的右孩子$B$==向左上旋转==代替$A$成为根结点
2.   将$A$结点==向左下旋转==成为$B$的左子树的根结点
3.   而$B$的原左子树则作为$A$结点的右子树。

![23July20-214506-1689860706-7317c4a4-0bf2-448a-a73c-f344af96331d](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202145646.png)

实现思路：

1.   `f->rchild=p->lchild;`
     +   让结点$A$的右子树指向$B$的左子树$BR$
2.   `p->lchild=f;`
     +   让结点$B$的左子树变为$A$
3.   `gf->lchild/rchild=p;`
     +   让结点$A$的原双亲`gf`的左/右孩子指向$B$

<video src="https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202217683.mp4"/>

#### $LR-$先左后右双旋转

从结点的左孩子的右子树中插入导致不平衡：

![23July20-215611-1689861371-5986f2ef-41cd-4067-b0f2-f99a17df2039](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202156173.png)

将$BR$拆分为$C$和$CL$、$CR$，假设插入的是$CR$部分，插入$CL$也同理。

+   二叉排序树的特性：左子树结点值<根结点值<右子树结点值
    +   $BL<B<CL<C<CR<A<AR$

---

由于在$A$的左孩子$L$的右子树$R$上插入新结点，$A$的平衡因子由$1$增至$2$，导致以$A$为根的子树失去平衡，需要进行两次旋转操作：

1.   先左旋转
     +   先将$B$的右子树的根结点$C$向左上旋转提升到$B$结点的位置
2.   后右旋转
     +   然后再把该$C$结点向右上旋转提升到$A$结点的位置。

![23July20-215707-1689861427-a8805737-b4a5-4d6b-9e3e-d6e1be2d2adb](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202157521.png)

#### $RL-$先右后左双旋转

从结点的右孩子的左子树中插入导致不平衡：

![23July20-215815-1689861495-c9e8a8da-4fed-4717-af3a-1cfc8dafa752](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202158603.png)

+   二叉排序树的特性：左子树结点值<根结点值<右子树结点值
    +   $AL<A<CL<C<CR<B<BR$

由于在$A$的右孩子$R$的左子树$L$. 上插入新结点，$A$的平衡因子由$-1$减至$-2$，导致以$A$为根的子树失去平衡，需要进行两次旋转操作

1.   先右旋转
     +   先将$B$的左子树的根结点$C$向右上旋转提升到$B$结点的位置
2.   后左旋转
     +   然后再把该$C$结点向左上旋转提升到$A$结点的位置。

![23July20-215913-1689861553-fbe692eb-9671-4f96-b7aa-04305086af82](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202159513.png)

#### 平衡二叉树效率

查找时间复杂度就为$O(\log_{2}n)$而插入和删除也为$O(\log_{2}n)$。

这显然是比较理想的种动态查找表算法

### 红黑树

#### 考查形式

+   红黑树的定义、性质一一选择题
+   红黑树的插入/删除一一要能手绘插入过程（不太可能考代码，略复杂），删除操作也比较麻烦，也许不考

#### 红黑树定义

![img](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307212039332.png)

平衡二叉树在每次插入或删除操作的时候很容易破坏“平衡”特性，通常需要频繁调整树的形态，时间开销大，所以引入为弱化版相对平衡的二叉查找树——红黑树($Red\;Black \; Tree$)

红黑树是一种含有红黑结点并能自平衡的二叉排序树。它必须满足下面性质：

0.   二叉排序树
     +   ==左子树结点值$\le$根结点值$\le$右子树结点值==
     +   不存在两个结点具有相同的值
     +   ==红黑树是二叉排序树，不是平衡二叉树==
     +   红黑树是==自平衡==排序树，不是平衡二叉树
     
1. 每个结点或是红色，或是黑色的。
2. 根结点是黑色的。
3. 叶结点都是黑色的，保证红黑树的内部结点左右孩子均非空。
    +   叶结点为$NULL$结点 +
    +   和$n+1$个虚构的外部结点
4. 不存在两个相邻的红结点
    +   即==红结点的父结点和孩子结点均是黑色的==
    +   插入操作时一般只会破坏这个特性，只检查这个特性是否被破坏即可
5. 对每个结点，从该结点到任一叶结点的简单路径上，所含黑结点的数量相同。
    +   从某结点出发（不含该结点）到达任一空叶结点的路径上黑结点总数称为结点的黑高$bh$

所以定义某结点出发到达一个叶结点的任一简单路径上的黑结点总数（**不含**该目的结点）称为该结点的黑高（记为$bh$），根结点的黑高就是红黑树的黑高。

![23July21-200356-1689941036-9be4bac3-c1f2-435e-8068-cc270c6e4018](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307212004659.png)

#### 红黑树性质

1.   从根到叶结点的最长路径不大于最短路径的两倍。

     + 由性质`5.`，当从根到任一叶结点的简单路径最短时，这条路径必然全由黑结点构成（即第二层的结点）。

     + 由性质`4.`，当某条路径最长时，这条路径必然是由黑结点和红结点相间构成的，此时红结点和黑结点的数量相同（非第二层的其他所有结点）。


2.   有$n$个内部结点的红黑树的高度$h\leqslant2\log_2(n+1)$。
     + 若红黑树的总高度为$h$
     + 则根结点黑高$\geqslant\dfrac{h}{2}$，所以内部结点$n\geqslant2^{\frac{h}{2}-1}$（假设没有红结点）
     + 所以$h\leqslant2\log_2(n+1)$。


>   对于一棵动态查找树，如果插入和删除操作比较少，查找操作比较多，采用$AVL$树比较合适，否则采用红黑树更合适
>
>   但由于维护这种高度平衡所付出的代价比获得的效益大得多，红黑树的实际应用更广泛，$Cpp$中的$map$和$set$ 以及$Java$中的$TreeMap$和$TreeSet$就是用红黑树实现的

3.   红黑树查找、插入、删除的时间复杂度都是$O(\log_2n)$

     + 插入和删除
         + 由于红黑树的每次操作平均要旋转一次和变换颜色
         + 而普通二叉查找树如果平衡因子在指定范围内不会旋转
             + 如果要旋转则可能旋转多次
         + 所以红黑树比普通的二叉查找树效率要低一点，不过时间复杂度仍然是$O(\log_2n)$。
     + 普通查询
         + 没有使用到红黑树的性质，所以红黑树和二叉查找树的效率相同
         + 对于二叉平衡树而言，平衡树的效率更高。
     + 插入有序数据查询
         + 红黑树的查询效率就比二叉查找树要高了
         + 因为此时二叉查找树不是平衡树，它的时间复杂度$O(n)$。
             + 此时可能是单枝树

4.   根结点黑高为的红黑树，两部结点数（关键字）至少有$2^h-1$个

     +   ==内部结点数最少的情况为总共$h$层黑结点的满树形态==

     +   若根结点黑高为$h$，内部结点数（关键字）最少有$2^h-1$个

     +   当根结点黑高为$h$时，只有在满树形态时红黑树树高才会是$h$,否则树高$H>h$

         ![23July21-212541-1689945941-44bff19b-6a0e-483b-9501-c127a0e09992](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307212125681.png)

#### 红黑树插入概述

红黑树插入演示网站[Red/Black Tree Visualization (usfca.edu)](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html)

假定当前结点为$x$，其父结点为$p$，叔父结点（父结点的兄弟结点）为$u$，祖父结点为$g$。

![2392382-9ac3d6b69ef7ead3](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307212051599.webp)

红黑树的插入过程和二叉查找树的插入过程基本类似，从右至左，右上至下。不同之处在于，红黑树中插入新结点后需要进行调整（主要通过重新着色或旋转操作进行），以满足红黑树的性质。

红黑树插入操作思路

1.   先查找，确定插入位置（原理同二叉排序树），插入新结点
     1.   新结点是根一一染为<span style="background-color: black; color: white;">黑色</span>
     2.   新结点非根一一染为<span style="background-color: red; color: white;">红色</span>
          +   假设新插入的结点初始着为黑色，那么这个结点质在的路径比其他路径多出一个黑结点（几乎每次插入都破坏性质`5.`），调整起来也比较麻烦
          +   如果插入的结点是红色的，此时所有路径上的黑结点数量不变，仅在出现连续两个红结点时才需要调整，而且这种调整也比较简单。
2.   若插入新结点后依然满足红黑树定义，则插入结束
     +   若父结点是<span style="background-color: black; color: white;">黑色</span>的，则插入新结点后依然满足红黑树定义
3.   若插入新结点后不满足红黑树定义(即父结点是<span style="background-color: red; color: white;">红色</span>的)，需要调整，使其重新满足红黑树定义
     1.   叔叔结点为<span style="background-color: black; color: white;">黑色</span>：旋转+染色
          +   $LL$型：右单旋，父换爷+染色
              +   父亲结点和祖父结点分别染色
              +   插入之前是<span style="background-color: black; color: white;">黑色</span>就染色成<span style="background-color: red; color: white;">红色</span>
              +   插入之前是<span style="background-color: red; color: white;">红色</span>就染色成<span style="background-color: black; color: white;">黑色</span>
              +   下同
          +   $RR$型：左单旋，父换爷+染色
          +   $LR$型：左、右双旋，儿换爷+染色
          +   $RL$型：右、左双旋，儿换爷+染色
     2.   叔叔结点为<span style="background-color: red; color: white;">红色</span>：染色+变新
          +   叔父爷三个结点染色，祖父结点视为新结点
          +   新结点处理见上`1.1&1.2&2&3`，需要再次检查是否破坏了红黑树特性

以$1\sim7$的序列构建红黑树：

![红黑树插入](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307201951428.png)

#### 红黑树插入情景分析及处理

##### 红黑树为空树

最简单的一种情景，直接把插入结点作为根结点就行，但注意，根据红黑树性质2：根结点是黑色。还需要把插入结点设为黑色。

**处理：把插入结点作为根结点，并把结点设置为<span style="background-color: black; color: white;">黑色</span>**。

##### 插入结点的Key已存在

插入结点的Key已存在，既然红黑树总保持平衡，在插入前红黑树已经是平衡的，那么把插入结点设置为将要替代结点的颜色，再把结点的值更新就完成插入。

**处理：**

-   **把插入结点设为当前已存在结点的颜色**
-   **更新当前结点的值为插入结点的值**

##### 父结点为<span style="background-color: black; color: white;">黑色</span>

默认插入结点为红色，且父结点为黑，所以此时插入后不影响黑高和平衡，==从而不需要调整==。

插入$2$，只有一个父结点为根结点，没有叔结点，直接插入到$1$的右子树位置，没有其他变化。

##### 父结点为<span style="background-color: red; color: white;">红色</span>叔结点<span style="background-color: red; color: white;">红色</span>

由于插入结点为红色，父结点也为红色，不能出现连续的红色，所以就要变色。

这种情况是不需要进行旋转的。

由于父结点和叔结点都是红色，不能连续红色，所以将父结点和叔结点都涂为黑色

由于父结点和叔结点都是红色，所以其祖父结点一定是黑的，涂为黑色

此时将当前结点移动到祖父结点上，查看先祖结点的涂色是否正确，递归这个逻辑直到根结点，根结点一定是黑色的。

**处理：**

-   **将父结点P和叔结点S设置为黑色**
-   **将祖父结点PP设置为红色**
-   **把祖父结点PP视为新结点，看是否再次破坏了红黑树，破坏了就再来一次**

#### 红黑树删除概述

红黑树的播入操作容易导致连续的两个红结点，破坏性质④。而删除操作容易造成子树黑高的变化（删除黑结点会导致根结点到叶结点间的黑结点数量减少），破坏性质⑤。

所以红黑树删除要比红黑树插入更复杂。

删除过程也是先执行二叉查找树的删除方法。若待删结点有两个孩子，不能直接删除，而要找到该结点的中序后继（或前驱）填补，即右子树中最小的结点，然后转换为删除该后继结点。由于后继结点至多只有一个孩子，这样就转换为待删结点是叶结点或仅有一个孩子的情况。

+ 前驱结点为二叉查找树中序遍历中当前点的前一个结点，往往在其左子树的最右测。
+ 后继结点为二叉查找树中序遍历中当前点的后一个结点，往往在其右子树的最左测。

对于二叉查找树删除而言有三种情况：

1. 被删除结点无子结点， 直接删除。
2. 被删除结点只有一个子结点，用子结点替换要被删除的结点。
3. 被删除结点有两个子结点，可以用前驱结点或者后继结点进行替换删除操作。

第一种情况不用多考虑，融合后两种情况并结合红黑树特性分为三种情况。

以完成所有插入后的红黑树为例。

$x$为父亲的左儿子：

+ 兄弟为红色：将兄弟变成黑色，父结点变成红色；对父结点左旋，恢复左子树的黑色高度，左侄子成为新的兄弟。
+ 兄弟为黑色，左右侄子为黑色：兄弟变成红色，$x$指向父结点，继续进行调整。
+ 兄弟为黑色，右侄子为黑色（左侄子为红色）：左侄子变成黑色，兄弟变成红色；兄弟右旋，恢复右子树的黑色高度，左侄子成为新的兄弟。
+ 兄弟为黑色，右侄子为红色：兄弟变成父结点颜色，父结点和右侄子变成黑色；父结点左旋，$x$指向整棵树的根结点，结束循环

#### 填充结点红

即删除后用来填补该位置的结点为红色结点。

此时无论当前结点是什么颜色，删除后不影响黑高，所以不影响红黑树平衡。

1. 当前结点为红：删除$4$，后继结点$5$为红结点，所以直接$5$替换$4$的位置并变红，其他不变。
2. 当前结点为黑：删除$6$，后继结点$7$为红结点，所以直接$7$替换$6$的位置并变黑，其他不变。

#### 填充结点黑且为左结点

即删除后用来填补该位置的结点为黑色结点，且该结点为其父结点的左子结点。

1. 填充结点的兄弟结点是红结点：删除$2$，前驱结点$1$为黑结点，其兄弟结点$4$为红结点。
2. 填充结点的兄弟结点是黑色，同时兄弟结点的右子结点是红色的，左子结点任意颜色：删除$4$，前驱结点$3$为黑结点，其兄弟结点$6$为黑结点，$6$的右子结点$7$为红结点。

#### 填充结点黑且为右结点

即删除后用来填补该位置的结点为黑色结点，且该结点为其父结点的右子结点。

#### 红黑树与二叉查找树

由二叉查找树树的“高度平衡”，降低到红黑树的“任一结点左右子树的高度，相差不超过两倍”的“适度平衡”，也降低了动态操作时调整的频率。对于一棵动态查找树，如果插入和删除操作比较少，查找操作比较多，采用$AVL$树比较合适，否则采用红黑树更合适。

二叉查找树使用平衡因子来约束树高，而红黑树用黑高相同约束树高。

二叉查找树只要平衡因子不超过$1$的绝对值就不用每次调整树整体，而红黑树每次插入货删除都需要进行对树进行调整。

#### 红黑树与B树

红黑树在被发明之初被称为平衡二叉$B$树，所以红黑树与$B$树之间必然有联系。

+ 将红黑树的所有红色结点上移到和他们的父结点同一高度上组成一个$B$树结点，就会得到一棵四阶$B$树。
+ 红黑树的黑色结点个数与四阶$B$树的结点总个数相等。
+ 在所有的$B$树结点中，黑色结点是父结点，红色结点是子结点。黑色结点在中间，红色结点在两边。

## 树表查找

![image-20230723104959976](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307231050285.png)

### B树

即多路平衡查找树，要求掌握基本特定点、操作。

>   B树演示网站 [B-Tree Visualization (usfca.edu)](https://www.cs.usfca.edu/~galles/visualization/BTree.html)

#### B树定义

![23July21-213739-1689946659-9d6ae516-3b52-4af4-a591-52b932c3bbaf](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307212137200.png)

$B$树，又称多路平衡查找树，$B$树中所被允许的孩子个数的最大值称$B$树的阶，通常用$m$表示。一棵$m$阶$B$树或为空树或为满足如下特性的$m$叉树：

1.   树的每个结点至多包含$m$棵子树，至多包含$m-1$个关键字。
2.   若根结点不是终端结点，则至少有两颗子树，有一个关键字。
3.   除根结点以外的所有非叶结点至少有$\lceil\dfrac{m}{2}\rceil$棵子树
     +   即至少包含$\lceil\dfrac{m}{2}\rceil-1$个结点。
4.   所有叶结点都出现在同一个层次上且不带信息。
     +   可以视为外部结点或类似于折半查找判定树的查找失败结点
     +   ==实际上这些结点不存在==，指向这些结点的指针为空

为了保证$m$叉查找树中每个结点都能被有效利用，避免大量结点浪费导致树高过大，所以规定$m$叉查找树中，除了根结点以外（根结点最小为$1$）：

+ 任何结点至少有$\lceil\dfrac{m}{2}\rceil$个分叉，即至少包含$\lceil\dfrac{m}{2}\rceil-1$个结点。
+ 至少有两棵子树。
+ 数据最下面的为终端结点，查询失败的结点为叶结点。

为了保证$m$叉查找树是一棵平衡树，避免树偏重导致树高过大，所以==规定$m$叉查找树中任何一个结点，其所有子树的高度都要相同。==

非叶结点定义：$\{n,P_0,K_1,P_1,\cdots,K_n,P_n\}$。其中$K_i$为结点关键字，$K_1<K_2<\cdots<K_n$，$P_i$为指向子树根结点的指针。$P_{i-1}$所指子树所有结点的关键字均小于$K_i$，$P_i$所指子树的关键字均大于$K_i$。

#### B树性质

+ 子树数和关键字数
    + 根结点的子树数$\in [2,m]$，关键字数$\in [1,m-1]$
    + 其他结点的子树数$\in [\lceil\dfrac{m}{2}\rceil,m]$，关键字数$\in [\lceil\dfrac{m}{2}\rceil-1,m-1]$
+ 任意结点的每棵子树都是绝对平衡的
    + 即所有结点的平衡因子均等于$0$
+ 每个结点中的关键字是有序的
    + 子树$0<$关键字$1<$子树$1<$关键字$2<$子树$2<\cdots$
        + 非叶结点定义：$\{n,P_0,K_1,P_1,\cdots,K_n,P_n\}$。其中$K_i$为结点关键字，$K_1<K_2<\cdots<K_n$，$P_i$为指向子树根结点的指针。$P_{i-1}$所指子树所有结点的关键字均小于$K_i$，$P_i$所指子树的关键字均大于$K_i$。
+ $B$树最底端的失败的不存在的结点就是常说的叶子结点，而最底端的存在数据的结点就是终端结点
    + 一般的树的叶子结点和终端结点都是指最底端的有数据的结点
    + 实际上这些结点不存在
+ 携带数据的是内部结点，最底部的叶子结点也称为外部结点。
+ 具有$n$个关键字的$m$阶$B$树，应有$n+1$个叶结点。
    + 叶结点即查询失败的结点，对于$n$个关键字查找则可能的失败范围有$n+1$种。
+ 有$n$个非叶结点的$m$阶$B$树中至少包含$(n-1)(\lceil\dfrac{m}{2}\rceil-1)+1$个关键字。
    + 除根结点外的$n-1$个$m$阶$B$树中的每个非叶结点最少有$\lceil\dfrac{m}{2}\rceil-1$
    + 然后再加上根结点的一个，所以最少为$(n-1)(\lceil\dfrac{m}{2}\rceil-1)+1$个。

$B$树相关性质计算涉及多个单元，注意一定要区分：

+ 树高
    + 默认是不包含无数据的叶子结点/失败结点

+ 子树棵数。
+ 关键字。
+ 结点（结点数往往小于关键字数）。

#### B树树高与关键字

$B$树中的大部分操作所需的磁盘存取次数与$B$树的高度成正比。高度一般与关键字相关。

==计算$B$树高度不包括叶子结点==

对于含有$n$个关键字的$m$阶$B$树：

+ 最小高度：$h\geqslant\log_m(n+1)$
    + 让每个结点尽可能满
    + 有$m-1$个关键字，$m$个分叉
    + 则一共有$(m-1)(1+m+m^2+\cdots+m^{h-1})$个关键字
        + 其中由于每个结点都是满的，所以第一层有一个结点(根结点)，第二层有$m$个结点$,\cdots$
        + 即共有$(1+m+m^2+\cdots+m^{h-1})$个结点
        + 而$(m-1)$表示对于$m$阶$B$树每个结点至多包含$m-1$个关键字
    + 其中$n$小于等于这个值，从而求出$h\geqslant\log_m(n+1)$。
+ 最大高度：$h\leqslant\log_{\lceil\frac{m}{2}\rceil}\dfrac{n+1}{2}+1$
    + 让各层分叉尽可能少，即根结点只有两个分叉，其他结点只有$\lceil\dfrac{m}{2}\rceil$个分叉
        + 所以第一层$1$个，第二层$2$个，第$h$层$2(\lceil\dfrac{m}{2}\rceil)^{h-2}$个结点，而$h+1$层的叶子结点有$2(\lceil\dfrac{m}{2}\rceil)^{h-1}$个
        + 且$n$个关键字的$B$树必然有$n+1$个叶子结点，从而$n+1\geqslant2(\lceil\dfrac{m}{2}\rceil)^{h-1}$
        + 即$h\leqslant\log_{\lceil\frac{m}{2}\rceil}\dfrac{n+1}{2}+1$。
    + 让各层关键字尽可能少，记$k=\lceil\dfrac{m}{2}\rceil$
        + 第一层最少结点数和最少关键字为$1$
        + 第二层最少结点数为$2$，最少关键字为$2(k-1)$
        + 第三层最少结点数为$2k$，最少关键字为$2k(k-1)$
        + 第$h$层最少结点数为$2k^{h-2}$，最少关键字为$2k^{h-2}(k-1)$
        + 从而$h$层的$m$阶$B$树至少包含关键字总数$1+2(k-1)(k^0+k^1+\cdots+k^{h-2})=1+2(k^{h-1}-1)$
        + 若关键字总数小于这个值，则高度一定小于$h$
        + 所以$n\geqslant 1+2(k^{h-1}-1)$
        + 则$h\leqslant\log_{\lceil\frac{m}{2}\rceil}\dfrac{n+1}{2}+1$。

#### B树关键字与结点

树高与结点：

+ 对于已知树高和阶求最大最小关键字数量就是上面公式的逆运算
    + 已知树高可以求出关键字数量。
    + 对于高度为$h$的$m$阶$B$树
        + 最多有$$(m-1)(1+m+m^2+\cdots+m^{h-1})$$个结点
        + 最少有$1+2(\lceil\dfrac{m}{2}\rceil^{h-1}-1)$个结点
    
+ 根据关键字数量和结点之间的关系相除
    + 求最大高度就除以每结点最小关键字数，求最小高度就除以每结点最大关键字数。


关键字与结点：

+ 具有$n$个关键字的$m$阶$B$树，应有$n+1$个叶结点。

叶结点即查询失败的结点，对于$n$个关键字查找则可能的失败范围有$n+1$种。

+ 有$n$个非叶结点的$m$阶$B$树中至少包含$(n-1)(\lceil\dfrac{m}{2}\rceil-1)+1$个关键字。

除根结点外的$n-1$个$m$阶$B$树中的每个非叶结点最少有$\lceil\dfrac{m}{2}\rceil-1$，然后再加上根结点的一个，所以最少为$(n-1)(\lceil\dfrac{m}{2}\rceil-1)+1$个。

#### B树查找

$B$树的查找包含两个基本操作：

1. 在$B$树中找结点。
2. 在结点内找关键字。

由于$B$树常存储在磁盘上，因此前一个查找操作是在磁盘上进行的，而后一个查找操作是在内存中进行的，即在找到目标结点后，先将结点信息读入内存，然后在结点内采用顺序查找法或折半查找法。

在$B$树上查找到某个结点后，先在有序表中进行查找，若找到则查找成功，否则按照对应的指针信息到所指的子树中去查找，则说明树中没有对应的关键字，查找失败。

#### B树插入

+   新元素插入一定是插入到最底层的终端结点，使用$B$树的查找来确定插入位置

+   ==插入位置一定是最底层的某个非叶结点。==

+   若导致原结点关键字数量超过上限溢出（$m-1$个关键字）

    +   就从中间位置$\lceil\dfrac{m}{2}\rceil$（如果$m$为偶数则默认是$\lceil\dfrac{m}{2}\rceil-1$）分开

    +   将**左部分包含的关键字**放在原来结点

    +   **中间的一个结点**$\lceil\dfrac{m}{2}\rceil$或$\lceil\dfrac{m}{2}\rceil-1$插入到原结点的父结点上，并考虑在父结点的顺序对指针进行调整保证顺序

    +   **右部分包含的关键**字放在一个新结点并插入到原结点的父结点的后一个位置上

        +   而在原结点的父结点连接后的结点后移一个连接让位给分割出来的右半部分结点

        <video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July21-220413-1689948253-186a2efa-13b3-4498-9b09-edcae6186d97.mp4"></video>
+   若父结点插入时也溢出了

    +   则同理在父结点的中间进行分割，左半部分在原来父结点

    +   右半部分新建一个父结点，并把中间结点右边开始的所有连接移动到新父结点上

    +   中间的结点上移到祖父结点，如果没有就新建，然后建立两个指针分别指向原父结点和新父结点

        <video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July21-220748-1689948468-a4bf344a-2bd8-446d-892d-ab3d83fdad33.mp4"></video>

#### B树删除

+   若被删除关键字在终端结点，且结点关键字个数不低于下限，则直接删除该关键字，并移动后面的关键字

+   若被删除关键字在非终端结点，则用直接前驱或直接后继来替代被删除关键字，然后后面的元素直接前移：

    + 直接前驱：当前关键字左侧指针所指子树遍历到最右下的元素。

    + 直接后继：当前关键字右侧指针所指子树遍历到最左下的元素。

        <video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July21-221739-1689949059-bcd2454e-946e-4d3b-8f50-463d315dcdb3.mp4"></video>


+   若被删除关键字在终端结点，但是结点关键字个数删除后低于下限：

    + 右兄弟够借：若原结点右兄弟结点里的关键字在删除一个后高于下限，则可以用结点的后继以及后继的后继来顶替：
        1. 将原结点在父结点的连接的后一个关键字（后继元素）下移到原结点并放在最后面。
        
        2. 将原结点右兄弟结点的第一个关键字上移插入到下移的元素的空位。
        
        3. 原结点右兄弟结点里的关键字全部前移一位。
        
            <video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July21-221455-1689948895-a8cf3acc-996d-4f51-916f-7b108116cbf8.mp4"></video>

    + 左兄弟够借：若原结点里右兄弟的关键字在删除一个后低于下限，但是左兄弟的结点足够，则可以用结点的前驱以及前驱的前驱来顶替：
        1. 将原结点在父结点的连接的前一个关键字（前驱元素）下移到原结点并放在最前面，其余元素后移。
        
        2. 将原结点左兄弟结点的最后一个关键字上移插入到原结点父结点的连接的前面。
        
        3. 原结点左兄弟结点里的关键字全部前移一位。
        
            <video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July21-221331-1689948811-59e12879-a202-4ec0-b08a-218c3b9e45f9.mp4"></video>

    + 左右兄弟都不够借：若左右兄弟结点的关键字个数均等于下限值，则将关键字删除后与左或右兄弟结点以及父结点中的关键字进行合并：
        1. 将原结点的父结点连接后的关键字插入到原结点关键字最后面。
        2. 将原结点的左或右兄弟结点的关键字合并到原结点（前插或后插），并将连接也转移到原结点上。
        3. 若父结点的关键字个数又不满于下限，则父结点同样要于与它的兄弟父结点进行合并，并不断重复这个过程。
        4. 若父结点为空则删除父结点。（兄弟合并，父亲下沉）
        
        <video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July21-220937-1689948577-327960f7-de80-4cc3-b8a2-64738f451686.mp4"></video>


### B+树

$B+$树考的并不是很深。

可以联想接近分块查找

与分块查找的思想类似，是对$B$树的一种变型，多用于索引结构，例如数据库设计语言。

![23July21-222323-1689949403-144a205f-4dc6-49a5-89b1-15ea5d66f3d7](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307212223973.png)

>   B+树在数据库语言中有广泛的应用，特别是用于实现索引结构和辅助数据存储。以下是B+树在数据库语言中的主要应用：
>
>   1. 索引结构：数据库中常用的索引结构之一是B+树索引。B+树的自平衡特性和较低的高度使得它非常适合用作索引结构，可以快速定位到特定的数据记录。对于数据库中的表，可以使用B+树索引来加速对数据的查找和检索操作，从而提高数据库查询的效率。
>
>   2. 范围查询：B+树支持范围查询非常高效。在数据库中，经常需要根据某个字段的范围来检索数据，例如查找某个时间段内的所有记录或某个数值范围内的数据。B+树的有序性和平衡性使得范围查询的性能较好。
>
>   3. 多级索引：数据库中的表可能有多个字段需要建立索引。B+树的多级索引结构使得可以同时基于多个字段建立索引，从而满足不同查询的需求。
>
>   4. 磁盘存储优化：B+树的结点通常被组织成页，每个页的大小与磁盘页的大小相当。这样可以有效地利用磁盘预读特性，减少磁盘I/O次数，提高数据读取的效率。B+树的叶子结点构成了一个有序链表，可以很方便地进行顺序遍历。
>
>   5. 数据库索引的持久化：数据库通常需要将索引结构持久化到磁盘上，以便在系统重启后能够恢复索引数据。B+树的结点结构天然适合持久化存储，并且支持快速的恢复。
>

#### B+树定义

一个$m$阶的$B+$树需要满足以下条件：

1. 每个分支结点最多有$m$棵子树或孩子结点。
2. 为了保持绝对平衡，非叶根结点至少有两棵子树，其他每个分支结点至少有$\lceil\dfrac{m}{2}\rceil$棵子树
    +   不同于$B$树，$B+$树又重新将最下面的保存的数据定义为叶子结点
3. ==结点的子树个数与关键字个数相等==
    +   $B$树结点子为树个数与关键字个数加$1$
4. 所有叶结点包含所有关键字以及指向记录的指针，叶结点中将关键字按大小排序，并且相邻叶子结点按大小顺序相互连接起来
    +   所以$B+$树支持顺序查找。
5. 所有分支结点中仅包含其各子结点中关键字的最大值以及指向其子结点的指针
    +   即分支结点只是索引

#### B+树查找

+   ==无论查找成功与否，$B+$树的查找一定会走到最下面一层结点==
    +   因为对应的信息指针都在最下面的结点
    +   而$B$树查找可以停留在任何一层。
+   $B+$树可以遍历查找
    +   即从根结点出发，对比每个结点的关键字值
    +   若目标值小于当前关键字值且大于前一个关键字值，则从当前关键字的指针向下查找。
+   $B+$树可以顺序查找
    +   即在叶子结点的块之间定义指向后面叶子结点块的指针，从而能顺序查找。


#### B+树与B树区别

对于$m$阶$B+$树与$B$树：

|             &nbsp;              |                             B+树                             |                             B树                              |
| :-----------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 结点的$n$个关键字对应的子树个数 |                             $n$                              |                            $n+1$                             |
|        根结点的关键字数         |                           $[1,m]$                            |                          $[1,m-1]$                           |
|       其他结点的关键字数        |                $[\lceil\dfrac{m}{2}\rceil,m]$                |              $[\lceil\dfrac{m}{2}\rceil-1,m-1]$              |
|           关键字分布            |      叶子结点包含所有关键字，非叶结点包含部分重复关键字      |                    所有结点的关键字不重复                    |
|            结点作用             |            叶子结点包含信息，非叶子结点是索引作用            |                      所有结点都包含信息                      |
|          结点存储内容           | 叶子结点包含关键字与对应记录的存储地址，非叶子结点包含对应子树的最大关键字和指向该子树的指针 |            所有结点都包含关键字与对应记录存储地址            |
|            查找方式             |                    ==随机查找、顺序查找==                    |                         ==随机查找==                         |
|            查找位置             |   ==需要查找到叶子结点的最底层==才能判断是否查找成功或失败   |                  查找到数的任何地方都能判断                  |
|            查找速度             | ==非叶子结点不包含关键字对应记录的存储地址==，可以使一个磁盘块含有多格关键字，从而让树的阶数更大，==树更矮，读磁盘次数更少，查找更快== | ==所有结点都包含存储地址==，保存的关键字数量更少，树高更高，所以读写磁盘次数更多，查找更慢 |

## 散列表查找

线性表和树表中，数据位置与数据关键字无关，而散列表数据位置与关键字有关。

### 散列表定义

+   散列表又称哈希表，是一种数据结构，数据元素的关键字与其存储地址直接相关

+   ==一个散列结构是一块地址连续的存储空间。==

+   散列表建立了关键字和存储地址之间的一种直接映射关系


### 散列函数

关键字与地址通过散列函数（哈希函数）来实现映射。即记录位置=散列函数(记录关键字)数，记为$Hash(key)=Addr$。

+   这里的地址可以是数组下标、索引或内存地址等

设计散列函数的注意事项

1.   定义域必须涵盖所有可能出现的关键字
2.   值域不能超出散列表的地址范围
3.   尽可能减少冲突
     +   散列函数计算出来的地址应尽可能均匀分布在整个地址空间。
4.   散列函数应尽量简单，能够快速计算出任意一个关键字对应的散列地址。

常见的散列函数方法：

+ 直接定址法：可表示为$H(key) = key$ 或 $H(key) = a\times key + b$，其中$a$、$b$均为常数
    + 这种方法计算特别简单，并且==不会发生冲突==
    + 但当关键字分布不连续时，会出现很多空闲单元，会将造成大量存贮单元的浪费。
+ 除留余数法：$H(key)= key\mod p$，$p$一般是**不大**于表长的最大质数
    + 这种方法使用较多，关键是选取较理想的$p$值，使得每一个关键字通过该函数转换后映射到散列空间上任一地址的概率都相等，从而尽可能减少发生冲突的可能性
        + 一般情形下，取$p$为一个最接近或等于散列表表长$m$的素数较理想
        + 如果是合数则因为可以被多个数整除从而多个关键字余数相同造成冲突
+ 数字分析法：分析关键字的各个位的构成，截取其中若干位作为散列函数值，尽可能使关键字具有大的敏感度
    + 即最能进行区分的关键字位，这些位数都是连续的。
    + 适用于关键字集合已知，且关键字的某几个数码位分布均匀的场景
+ 平方取中法：先求关键字的平方值，然后在平方值中取中间几位为散列函数的值
    + 因为一个数平方后的中间几位和原数的每一位都相关
    + 因此，使用随机分布的关键字得到的记录的存储位置也是随机的
    + 适用于关键字的每位取值都不够均匀或均小于散列地址所需的位数。
+ 折叠法：将关键字分割成位数相同的几部分(最后一部分的位数可以不同)，然后取这几部分的叠加和(舍去进位)作为散列函数的值
    + 例如，假设关键字为某人身份证号码$430\,1046\,8101\,5355$，则可以用$4$位为一组进行叠加
    + 即有$5355+8101+1046+430=14932$，舍去高位，则有$H(430104681015355)=4932$。
+ 随机数法：对于存储位置给定随机数安排，查找起来会很麻烦。


### 映射冲突

一般情况下，设计出的散列函数很难是单射的，即通常情况下不同的关键字可能会无可避免对应到同一个存储位置，这样就造成了冲突（碰撞）。此时，发生冲突的关键字互为同义词。

#### 开放定址法

可存放新表项的空闲地址既向同义词开放也向非同义词开放。从发生冲突的那个单元开始，按照一定的次序，从哈希表中找出一个空闲的存储单元，把发生冲突的待插入关键字存储到该单元中，从而解决冲突。既指如果当前冲突，则将元素移动到其他空闲的地方。

$$
H_i=(H(key)+d_i)\mod m
$$

+ $i$表示发生第$i$次冲突，$i=1,2,\cdots,m-1$
+ $m$为散列表长度，类似于循环队列，==超出表长以后就循环到最左边。==
+ $d_i$为增量序列，是指发生第$i$次冲突的时候，$H(key)$偏移了多少位。

取定某一增量序列后，对应的处理方法就是确定的。通常有以下4种取法：

+ 线性探测法：$d_i=1,2,3,\cdots,m-1$
    + 线性探测法充分利用了哈希表的空间，但在解决一个冲突时，可能造成新的冲突。
    + 理想情况下，若散列表表长为$m$,则最多发生$m-1$次冲突即可“探测”完整个散列表。
    + 采用线性探测法，一定可以探测到散列表的每个位置
    + 只要散列表中有空闲位置，就一定可以插入成功

+ 二次（平方）探测法：$di=1,-1,2^2,-2^2\cdots,(\dfrac{m}{2})^2,-(\dfrac{m}{2})^2$
    + 对比线性探测法更不容易产生聚集问题
    + <span style="color:orange">注意：</span>散列表长度$m$必须是一个可以表示为$4j+3$的素数才能探测到所有位置。
    + 采用平方探测法，至少可以探测到散列表中一半的位置
    + 即便散列表中有空闲位置，也未必能插入成功

+ 伪随机探测法：定义$d_i$是一个伪随机数。
    + 采用伪随机序列法，是否能探测到散列表中全部位置，取决于伪随机序列的设计是否合理

+ 再散列法：$d_i=Hash_2(key)$，又称双散列法
    + 需要使用两个散列函数,当通过第一个散列函数$H(key)$得到的地址发生冲突时，则利用第二个散列函数$Hash_2(key)$计算该关键字的地址增量
    + 它的具体散列函数形式：
        $$
        H_i=(H(key)+i\times Hash_2(key))\%m
        $$
        初始探测位置$H=H(key)\%m$
    + $i$是冲突的次数，初始为$0$
    + 在再散列法中，最多经过$m-1$次探测就会遍历表中所有位置，回到$H_0$位置。
        + 双散列法未必能探测到散列表的所有位置。
        + 双散列法的探测覆盖率取决于第二个散列函数`hash2(key)`设计的是否合理。
        + 若`hash2(key)`计算得到的值与散列表表长m互质，就能保证双散列发可以探测所有单元
        + 即可以让表长$m$本身就是质数


**聚集**：同义和非同义关键字都堆积到一起。原因是选取不当的处理冲突的方法。对查找长度有直接的影响。

>    <span style="color:orange">注意：</span>在开放定址的情形下，不能随便物理删除表中的已有元素
>
>   因为若删除元素，则会截断其他具有相同散列地址的元素的查找地址
>
>   当其他元素进行查找操作时，检测到删除的空单元就会查找失败
>
>   因此，要删除一个元素时，可给它做一个删除标记，进行逻辑删除
>
>   但这样做的副作用是:执行多次删除后，表面上看起来散列表很满，实际上有许多位置未利用
>
>   因此需要定期维护散列表，要把删除标记的元素物理删除。

#### 链地址法

![23July24-122354-1690172634-e36c44d2-e9b2-4dbb-96e2-2df7adfcc1f4](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307241224103.png)

+   又称为拉链法或链接法，是把相互发生冲突的同义词用一个单链表链接起来，若干组同义词可以组成若干个单链表
+   思想类似于邻接表的基本思想，且这种方法适合于冲突比较严重的情况。
+   插入操作默认头插法
+   指针需要额外的空间，故当结点规模较小时，开放定址法较为节省空间，而若将节省的指针空间用来扩大散列表的规模，可使装填因子变小，这又减少了开放定址法中的冲突，从而提高平均查找速度。
+   每次冲突都要重新哈希，计算时间增加。

#### 公共溢出区法

为所有冲突的关键字记录建立一个公共的溢出区来存放。在查找时，对给定关键字通过散列函数计算出散列地址后，先与基本表的相应位置进行比对，如果相等，则查找成功；如果不相等，则到溢出表进行顺序查找。如果相对于基本表而言，在有冲突的数据很少的情况下，公共溢出区的结构对查找性能来说还是非常高的。

### 散列查找

#### 查找实现

先通过散列函数计算目标元素存储地址，然后根据解决冲突的方法进行下一步的查询。

如果使用拉链法通过散列函数计算得到存储地址为空，则可以直接代表查找失败，这时候一般定义查找长度这里不算。

而如果使用开放地址法计算得到空位置的时候，代表查找失败，但是这时候需要定义查找长度要算这个地址。

若散列函数设计得足够好，散列查找时间复杂度可以达到$O(1)$，即不存在冲突。

#### 查找效率

散列表的查找效率取决于三个因素：散列函数、处理冲突的方法和装填因子。

装填因子。散列表的装填因子一般记为$\alpha$​，定义为一个表的装满程度，即
$$
\alpha=\frac{n}{m}
$$
其中$n$表示表中记录数，$m$表示散列表长度

填装因子的取值范围通常是 $0 \leq \alpha \leq 1$。

装填因子代表一个散列表中的满余情况，越大则查找效率越低。

通常情况下，填装因子的取值在一个合理范围内（通常是0.7到0.8左右）可以保证散列表的较好性能。当填装因子过高时，可以进行散列表的动态扩容，增加散列表的大小，从而降低填装因子，保持散列表的高效性。动态扩容的过程会涉及到重新计算散列值和重新散列已有的数据项。

若只给出了装填因子$\alpha$​，则此时平均查找长度为：
$$
ASL=\dfrac{1}{2}(1+\dfrac{1}{1-\alpha})
$$

#### 查找长度计算

以题目为例：

现有长度为$11$且初始为空的散列表$HT$，散列函数是$H(key)=key\mod13$，采用线性探测处理冲突。将关键字序列${19, 14, 23, 01, 68, 20, 84, 27, 55, 11, 10, 79}$依次插入$HT$。

首先构造散列表：

![image-20230724122449439](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307241224552.png)

查找失败的平均查找长度：

+ 即获得一个新数据，在散列表中查找，发现找不到，那么如何判断找不到？
    + **当前位置为空**
    + 如比较到索引$12$后没有其他数据了，这时查询失败
        + 特别是有些题目数据存储位置是间断的，一定要注意到为空的位置就算查询失败

+ 首先由于模是模$13$，所以数据插入的范围是$0\sim12$，所以查找的范围是$0\sim 12$。
    + 本体中没有模后为$0$的数值，但是查找还是要比较

+ 比如查找一个值，模$13$后为$1$，从$0$开始一直对比到$12$这个位置，一共比较了$13$次。
    + 若查找一个值，模$13$后为$0$，比对$0$时直接查找失败
    + 需要特别注意的是，散列函数不可能计算出地址$12$

+ 同理一直到$12$结束，所以一共$1+13+12+11+10+9+8+7+6+5+4+3=89$次。
+ 由于是模$13$所以一共有比较$13$轮，所以最后平均查找长度为$\dfrac{89}{13}$。

计算查找成功的平均查找长度：

+ 计算成功的长度，就是记录下每个数值比较了几次找到可存储的空间。
+ 一共要对比$1\times 6+2+3\times 3+4+9=30$次。
+ 由于一共$12$​个数据，所以平均查找长度为
    $$
    ASL= \frac{(1\times 6+2+3\times 3+4+9)}{12} = 2.5
    $$
    

![image-20230724122513196](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307241225309.png)