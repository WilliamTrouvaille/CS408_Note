# EX-DS相关总结

## 强化归纳

### 栈总结

1.   顺序栈：栈顶指针初始化为$-1$，因为索引最小为$0$，注意题上所给栈顶指针：初值为$-1$时，栈顶指针指向当前元素；初值为$0$时，栈顶指针指的是当前元素的下一个位置

     1.   栈空：`S.top==-1`或`S.top==0`
     2.   栈满：`S.top==MaxSize-1`或`S.top==MaxSize`
     3.   栈长：`length=S.top+1`或`length=S.top`

2.   共享栈：栈底不变，让两个顺序栈（分别为0号和1号）共享一个一维数组，将两个栈的栈底设在数组两端，栈顶向共享空间延伸。

     1.   两个栈的栈顶指针都指向栈顶元素，`top0==-1`时$0$号栈为空，`top1==MaxSize`时$1$号栈为空；仅当两个栈顶指针相邻即`top1-top0==1`时，判断为栈满。当$0$号栈进栈时`top0`先加$1$再赋值，$1$号栈进栈时`top1`先减$1$再赋值；出栈时则刚好相反。 
     2.   栈满的条件：`top0+1==top1`

3.   链栈：链栈的操作与链表类似，入栈和出栈的操作都在链表的表头进行。需要注意的是，对于带头结点和不带头结点的链栈

4.   括号匹配：最后出现的左括号最先被匹配$LIFO$。思想如下：

     1.   自左至右扫描表达式，若遇左括号，则将左括号入栈
     2.   若遇右括号，则将其与栈顶的左括号进行匹配
          1.   若配对，则栈顶的左括号出栈，否则出现括号不匹配错误
          2.   如果需要匹配但是栈空说明有单独的左或右括号，也匹配失败
          3.   如果结束，栈为空则正常结束，否则不匹配。

5.    表达式求值

### 树性质总结

#### 树的性质

1.   结点数$=$总度数$+1$

     + 每个节点都会为树的总度数贡献一个度数，并且根节点没有父节点，所以需要额外加$ 1$
2.   度为$m$的树至少有$h+m-1$个结点。
3.   度为$m$的树以及$m$叉树的第$i(i\ge 1)$层至多有$m^{i-1}$个结点

     + 如完全二叉树
4.   树的度$m$代表至少一个结点度是为$m$，且一定是非空树，至少有$m+1$个结点；而$m$叉树指所有结点的度都小于等于$m$，可以是空树。
5.   高度为$h$的$m$叉树至少有$h$个结点，此时为单支树
6.   高度为$h$的$m$叉树至多有$\dfrac{m^h-1}{m-1}$个结点。
7.   具有$n$个结点的$m$叉树最小高度为$\lceil\log_m(n(m-1)+1)\rceil$

     + 若使高度最小应使所有结点都有$m$个孩子
     + 所以$\dfrac{m^{h-1}-1}{m-1}<n\leqslant\dfrac{m^h-1}{m-1}$
         + $\dfrac{m^{h-1}-1}{m-1}$表示前$h-1$层最多有几个结点
         + $\dfrac{m^h-1}{m-1}$表示前$h-1$层最多有几个结点

     + 从而得到$h-1<\log_m(n(m-1)+1)\leqslant h$
     + 即高度最小取值$h_{min}=\lceil\log_m(n(m-1)+1)\rceil$
         + $\lceil\cdots\rceil$是向上取整符号

#### 一般二叉树性质

1.   设非空二叉树中度为$0$、$1$和$2$的结点个数分别为$n_0$，$n_1$、$n_2$，则$n_0=n_2+1$

     + 即叶子结点比二分结点(度为$2$的结点)多一个
     + 假设树中结点的总数为$n$，则$n=n_0+n_1+n_2$
     + 又根据树的结点$=$总度数$+1$得到$n=n_1+2n_2+0n_0+1$
     + 联立得到结论
     + 拓展到任意一棵树，若结点数量为$n$，则边的数量为$n-1$
2.   二叉树的第$i$层至多有$2^{i-1}$个结点。
     + $m$叉树的第$i$层至多有$m^{i-1}$个结点。
3.   高度为$h(h\ge 1)$的二叉树至多有$2^h-1$个结点。

     + 高度为$h$的$m$叉树至多有$\dfrac{m^h-1}{m-1}$个结点。

#### 完全二叉树性质

1.   完全二叉树最多只有一个度为$1$的结点，度为$0$度为$2$的结点的个数和一定为奇数
2.   若完全二叉树有$2k$个结点，则必然$n_1=1$，$n_0=k$，$n_2=k-1$
     + 即结点个数为$2k$的完全二叉树只有一个度为$1$的结点，叶子结点个数为$k$，二分结点个数为$k-1$

3.   若完全二叉树有$2k-1$个结点，则必然$n_1=0$，$n_0=k$，$n_2=k-1$。
     + 即结点个数为$2k-1$的完全二叉树没有度为$1$的结点，叶子结点个数为$k$，二分结点个数为$k-1$
4.   具有$n$个结点的完全二叉树的高度$h=\lceil\log_2(n+1)\rceil$或$h=\lfloor\log_2n\rfloor+1$

     1.   高为$h$的满二叉树共有$2^h一1$个结点，高为$h-1$的满二叉树共有$2^{h-1}-1$个结点
          + $2^{h-1}-1<n\leqslant2^h-1$

          + $h-1<\log_{2}{(n+1)}\le h$

          + $h=\lceil\log_{2}(n+1)\rceil$

     2.   高为$h-1$的满二叉树共有$2^{h-1}-1$个结点，高为$h$的完全二叉树至少$2^{h-1}$个结点至多$2^{h-1}$个结点
          +   $2^{h-1}< n\le2^h$
          +   $h-1<\log_{2}{n}\le h$
          +   $h=\lfloor\log_2n\rfloor+1$
5.   最多只有一个度为$1$的结点，即$n_1=0/1$，且该结点只有左孩子而无右孩子
6.   $i\leqslant\lfloor\dfrac{n}{2}\rfloor$为分支结点，$i>\lfloor\dfrac{n}{2}\rfloor$为叶子结点。
     + 按层序编号后，一旦出现某结点（编号为$i$）为叶结点或只有左孩子，则编号大于$i$的结点均为叶结点
7.   按层序从$1$开始编号，结点$i$的左孩子为$2i$，右孩子为$2i+1$，父结点如果有为$\lfloor\dfrac{i}{2}\rfloor$。
8.   若$n$为奇数，则每个分支结点都有左孩子和右孩子；若$n$为偶数，则编号最大的分支结点（编号为$\dfrac{n}{2}$）只有左孩子，没有右孩子，其余分支结点左、右孩子都有。

#### 二叉平衡树性质

$h$为平衡二叉树高度，$n_h$为构造此高度的平衡二叉树所需的最少结点数。

1.   平衡二叉树最多结点数$2^h-1$，即该二叉树为满二叉树。
2.   平衡二叉树最少结点数（所有非叶结点的平衡因子均为$1$的）的递推公式为$n_0=0$，$n_1=1$，$n_2=2$，$n_h=1+n_{h-1}+n_{h-2}$，此时所有非叶结点的平衡因子均为$1$。
     +   <span style="color:red">证明：</span>假设$T$为高度为$h$的平衡二叉树，其需要最少的结点数目为$F(h)$。
     +   又假设$TL$，$TR$为$T$的左右子树，因此$TL$，$TR$也为平衡二叉树。
     +   假设$FL$、$FR$为$TL$，$TR$的最少结点数，则$F(h)=FL+FR+1$。那么$FL$、$FR$到底等于多少呢？
     +   由于$TL$，$TR$与$T$一样是平衡二叉树，又由于我们知道$T$的最少结点数是$F(h)$，其中$h$为$T$的高度，因此如果我们知道$TL$，$TR$的高度就可以知道$FL$、$FR$的值了。
     +   由平衡二叉树的定义可以知道，$TL$和$TR$的高度要么相同，要么相差$1$，而当$TL$与$TR$高度相同（即都等于$h-1$）时，我们算出来的$F(h)$并不能保证最小，两边都是$h-1$明显比只有一边$h-2$的结点数更多。
     +   因此只有当$TL$与$TR$高度相差一，即一个高度为$h-1$，一个高度为$h-2$时，计算出来的$F(h)$才能最小。
     +   此时我们假设$TL$比$TR$高度要高$1$，即$TL$高度为$h-1$，$TR$高度为$h-2$，则有$F1=F(h-1)$，$F2=F(h-2)$。
     +   因此得到结论：$F(h)=F(h-1)+F(h-2)+1$。

#### 二叉排序树性质

左子树上所有结点的关键字均小于根结点的关键字，右子树上所有结点的关键字均大于根结点的关键字，左右子树又各是一棵二叉排序树。

#### 森林和一般树转换二叉树的对应关系

假设森林为$F$，树为$T$，转换而来的二叉树为$B$。

1.   $T$有$n$个结点，叶子结点个数为$m$，则$B$中无右孩子的结点个数为$n-m+1$个。
     +   树转换为二叉树时，树的每个分支节结点的所有子结点的最右子结点无右孩子，根结点转换后也无右孩子。
     +   n个节点的树，有n-1个边
     +   由于叶子节点个数为x，此树有n-x个非叶结点
     +   每个非叶结点有且仅有一个长子，对应二叉树有n-x左向边
     +   右向边 = 总边数 - 左向边 = (n-1) - (n-x) = x-1
     +   总共有n个点,其中只有x-1个点有右孩子，剩下的n-x+1个点没有右孩子(即证)
2.   $F$有$n$个非终端结点，则$B$中无右孩子的结点有$n+1$个。
     +   根据森林与二叉树转换规则“左孩子右兄弟”，$B$中右指针域为空代结点没有兄弟结点。
     +   森林中每棵树的根结点从第二个开始依次连接到前一棵树的根的右孩子，因此最后一棵树的根结点的右指针为空，这里有一个。
     +   另外，每个非终端结点即代表有孩子，其所有孩子结点不论有多少个兄弟，在转换之后，最后一个孩子的右指针一定为空，故树$B$中右指针域为空的结点有$n+1$个。
3.   $F$有$n$条边、$m$个结点，则$F$包含$T$的个数为$m-n$。
     +   若有$n$条边，则如果全部组成最小的树每个需要两个结点，总共需要$2n$个结点，组成$n$根树。
     +   假定$2n>m$，则还差$2n-m$个结点才能两两成树，所以少的这些结点不能单独成树，导致有$2n-m$个结点只能跟其他现成的树组成结点大于二的树。
     +   所以此时只能组成$n-(2n-m)=m-n$棵树。

#### 哈夫曼树性质

1.   每个初始结点最终都会变成叶子结点，且权值越小到根结点的路径长度越长。
2.   哈夫曼树的结点总数为$2n-1$。
3.   构建哈夫曼树时，都是两个两个合在一起的，所以没有度为一的结点，即$n_1=0$。
4.   哈夫曼树不唯一，但是$WPL$必然最优。

#### 红黑树性质

>   红黑树定义
>
>   0.   二叉排序树
>        +   左子树结点值$\le$根结点值$\le$右子树结点值
>        +   不存在两个结点具有相同的值
>        +   红黑树是二叉排序树，不是平衡二叉树
>        +   红黑树是自平衡排序树，不是平衡二叉树
>
>   1.   每个结点或是红色，或是黑色的。
>   2.   根结点是黑色的。
>   3.   叶结点都是黑色的，保证红黑树的内部结点左右孩子均非空。
>        +   叶结点为$NULL$结点 
>        +   和$n+1$个虚构的外部结点
>   4.   不存在两个相邻的红结点
>        +   即红结点的父结点和孩子结点均是黑色的
>        +   插入操作时一般只会破坏这个特性，只检查这个特性是否被破坏即可
>   5.   对每个结点，从该结点到任一叶结点的简单路径上，所含黑结点的数量相同。
>        +   从某结点出发（不含该结点）到达任一空叶结点的路径上黑结点总数称为结点的黑高$bh$

---

1.   从根到叶结点的最长路径不大于最短路径的两倍。
+ 由定义`5.`，当从根到任一叶结点的简单路径最短时，这条路径必然全由黑结点构成（即第二层的结点）。
  
+ 由定义`4.`，当某条路径最长时，这条路径必然是由黑结点和红结点相间构成的，此时红结点和黑结点的数量相同（非第二层的其他所有结点）。


2.   有$n$个内部结点的红黑树的高度$h\leqslant2\log_2(n+1)$。
     + 若红黑树的总高度为$h$
     + 则根结点黑高$\geqslant\dfrac{h}{2}$，所以内部结点$n\geqslant2^{\frac{h}{2}-1}$（假设没有红结点）
     + 所以$h\leqslant2\log_2(n+1)$。

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

     +   内部结点数最少的情况为总共$h$层黑结点的满树形态

     +   若根结点黑高为$h$，内部结点数（关键字）最少有$2^h-1$个

     +   当根结点黑高为$h$时，只有在满树形态时红黑树树高才会是$h$,否则树高$H>h$

#### B树性质

1.   子树数和关键字数
     + 根结点的子树数$\in [2,m]$，关键字数$\in [1,m-1]$
     + 其他结点的子树数$\in [\lceil\dfrac{m}{2}\rceil,m]$，关键字数$\in [\lceil\dfrac{m}{2}\rceil-1,m-1]$
2.   任意结点的每棵子树都是绝对平衡的
     + 即所有结点的平衡因子均等于$0$
3.   每个结点中的关键字是有序的
     + 子树$0<$关键字$1<$子树$1<$关键字$2<$子树$2<\cdots$
         + 非叶结点定义：$\{n,P_0,K_1,P_1,\cdots,K_n,P_n\}$。其中$K_i$为结点关键字，$K_1<K_2<\cdots<K_n$，$P_i$为指向子树根结点的指针。$P_{i-1}$所指子树所有结点的关键字均小于$K_i$，$P_i$所指子树的关键字均大于$K_i$。
4.   $B$树最底端的失败的不存在的结点就是常说的叶子结点，而最底端的存在数据的结点就是终端结点
     + 一般的树的叶子结点和终端结点都是指最底端的有数据的结点
     + 实际上这些结点不存在
5.   携带数据的是内部结点，最底部的叶子结点也称为外部结点。
6.   具有$n$个关键字的$m$阶$B$树，应有$n+1$个叶结点。
     + 叶结点即查询失败的结点，对于$n$个关键字查找则可能的失败范围有$n+1$种。
7.   有$n$个非叶结点的$m$阶$B$树中至少包含$(n-1)(\lceil\dfrac{m}{2}\rceil-1)+1$个关键字。
     + 除根结点外的$n-1$个$m$阶$B$树中的每个非叶结点最少有$\lceil\dfrac{m}{2}\rceil-1$
     + 然后再加上根结点的一个，所以最少为$(n-1)(\lceil\dfrac{m}{2}\rceil-1)+1$个。
8.   最小高度：$h\geqslant\log_m(n+1)$
     + 让每个结点尽可能满
     + 有$m-1$个关键字，$m$个分叉
     + 则一共有$(m-1)(1+m+m^2+\cdots+m^{h-1})$个关键字
         + 其中由于每个结点都是满的，所以第一层有一个结点(根结点)，第二层有$m$个结点$,\cdots$
         + 即共有$(1+m+m^2+\cdots+m^{h-1})$个结点
         + 而$(m-1)$表示对于$m$阶$B$树每个结点至多包含$m-1$个关键字
     + 其中$n$小于等于这个值，从而求出$h\geqslant\log_m(n+1)$。
9.   最大高度：$h\leqslant\log_{\lceil\frac{m}{2}\rceil}\dfrac{n+1}{2}+1$
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
10.   对于高度为$h$的$m$阶$B$树
      + 最多有$$(m-1)(1+m+m^2+\cdots+m^{h-1})$$个结点
      + 最少有$1+2(\lceil\dfrac{m}{2}\rceil^{h-1}-1)$个结点
11.   具有$n$个关键字的$m$阶$B$树，应有$n+1$个叶结点。
      +   叶结点即查询失败的结点，对于$n$个关键字查找则可能的失败范围有$n+1$种。
12.   有$n$个非叶结点的$m$阶$B$树中至少包含$(n-1)(\lceil\dfrac{m}{2}\rceil-1)+1$个关键字。
      +   除根结点外的$n-1$个$m$阶$B$树中的每个非叶结点最少有$\lceil\dfrac{m}{2}\rceil-1$，然后再加上根结点的一个，所以最少为$(n-1)(\lceil\dfrac{m}{2}\rceil-1)+1$个。

### 图性质总结

图$G$由顶点集$V$和边集$E$组成，$|V|,|E|$分别表示顶点数和边数

#### 有向图

1.   有向完全图：有向图中任意两个顶点之间都存在方向相反的两条弧，即$\vert E\vert=\vert V\vert(\vert V\vert-1)$
2.   强连通有向图，其边的个数至少为$n$，即构成一个有向环
3.   有向图出度之和与入度之和均为$\vert E\vert$，即与边数相等
4.   强连通图：有向图中任意两个顶点之间都是强连通的。

     + 当一个强连通图包含至少三个节点时，它必定含有环
5.   对于$n$个顶点的有向图，每个顶点的度最大为$2n-2$。因为任意一个顶点可以与其他$n-1$个顶点有指向相反的两条边。
6.   对于$n$个顶点的有向图
     + 若其是强连通图，则最少需要$n$条边，即形成环路的情况
7.   邻接矩阵中，
     1.   第$i$个顶点的出度$=$第$i$行的非零元素个数
     2.   第$i$个顶点的入度$=$第$i$列的非零元素个数
     3.   第$i$个顶点的度$=$第$i$行的非零元素个数$+$第$i$列的非零元素个数。

#### 无向图

1.   无向完全图，无向图中任意两个顶点之间都存在边，即$\vert E\vert=\dfrac{|V|(|V|-1)}{2}$
2.   $\vert E\vert>n-1$时，一定存在回路
     +   若一个无向图有$n$个顶点和$n-1$条边，可以使它连通但没有环（即生成树）
     +   但若再加一条边，在不考虑重边的情形下，则必然会构成环
3.   一个有$|E|$条边的非连通无向图至少有$m$个顶点，求$m$

     +   考虑该非连通图最极端的情况，即它由一个无向完全图加一个独立的顶点构成，此时若再加一条边，则必然使图变成连通图
     +   由$\vert E\vert=\dfrac{|V|(|V|-1)}{2}$算出$\vert V\vert$
     +   加一个独立的顶点，即$m=\vert V\vert+1$
4.   连通无向图，其边的个数至少为$n-1$，即构成一棵树
5.   具有$\vert V\vert$个顶点的无向图，当有$m$条边时能确保是一个连通图，求$m$

     +   考虑该非连通图最极端的情况，即它由一个无向完全图加一个独立的顶点构成，此时若再加一条边，则必然使图变成连通图
     +   由$\vert E\vert=\dfrac{|V|(|V|-1)}{2}$算出$\vert E\vert$
     +   加一条边，即$m=\vert E\vert+1$
6.   由于每条边都与两个顶点关联，无向图的全部顶点的度的和等于$2\vert E\vert$，即边数的两倍
7.   对于$n$个顶点的无向图，每个顶点的度最大为$n-1$。因为任意一个顶点可以与其他$n-1$个顶点相联。默认是简单图，即不能自己连向自己。
8.   无向连通图的每个顶点的度都是$2$。
9.   连通图：无向图中任意两个顶点之间都是连通的。

     + 只含有一个顶点的图是连通的
     + 当一个连通图包含至少三个节点时，它必定含有环。
10.   对于$n$个顶点的无向图
      + 若其是连通图，则最少需要$n-1$条边
      + 若其是非连通图，则最多有$C_{n-1}^2$条边，此时$n-1$个顶点构成一个完全图
11.   对于$n$个顶点、$e$条边的无向图是一个森林，则一共有$n-e$棵树。
      +   设一共有$x$棵树，则只需要$x-1$条边就能将森林连接为一整棵树，所以边数$+1=$顶点数（树的性质），即$e+(x-1)+1=n$，解得$x=n-e$。
12.   邻接矩阵中，第$i$个顶点的度=第$i$行或第$i$列的非零元素个数。

#### 一般图

1.   图的顶点数为$\vert V\vert$，图的边数为$\vert E\vert$，当$\vert E\vert\ge\vert V\vert+1$时，图可能是连通的

2.   图的顶点数为$\vert V\vert$，图的边数为$\vert E\vert$​，当且仅当
     $$
     \vert E\vert\ge\dfrac{(|V|-1)(|V|-2)}{2}+1
     $$
     时，图才一定是连通的

     +   此时$\vert V\vert-1$个顶点构成一个完全图，若再加入一条边，则一定变成连通图

3.   图一定是非空的，即$V$一定是非空集。

4.   稀疏图：边数很少的图，一般$\vert E\vert<\vert V\vert\log\vert V\vert$

5.  稠密图：边数很多的图，一般$\vert E\vert>\vert V\vert\log\vert V\vert$

6.  若图的顶点为$\vert V\vert$，则其生成树包含$\vert V\vert-1$条边

7.  对于$n$个顶点的环，有$n$棵生成树。

    +   因为$n$个顶点的环的生成树的顶点为$n-1$，去掉任意一条边就能得到一棵生成树，环一共有$n$条边，所以可以去掉$n$条，得到$n$棵生成树。

8.  设图$G$的邻接矩阵为$A$，$A^n$的元素$A^n[i][j]$表示由顶点$i$到顶点$j$长度为$n$的路径数量。

9.  图的广度优先生成树的高度小于等于深度优先生成树的高度。

10.  深度优先遍历可以判断有向图中是否存在回路

11.  深度优先遍历可以得到逆拓扑有序

12.  对于无向图，调用$BFS$函数的次数等于连通分量数

13. 对于非带权图，使用$BFS$可以解决非带权图的单源最短路径问题，因为$BFS$按照距离有近到远

14. 对于同样一个图，基于邻接矩阵存储的遍历所得到的DFS序列和BFS序列是唯一的，基于邻接表存储的遍历所得到的DFS序列和BFS序列是不唯一的。

15. 广度优先生成树

    +   若图顶点为$n$个，则生成树边一共有$n-1$条
    +   若邻接矩阵存储则唯一，若邻接表存储则不唯一。

16. 深度优先生成树

    +   若图顶点为$n$个，则生成树边一共有$n-1$条

17. 最小生成树

    + 最小生成树边的权值总是唯一且最小的。
    + 如果没有权值相同的边，则最小生成树是唯一的。
        + $MST$唯一性定理：$MST$没有使用无向网中相同权值的边，那么$MST$一定唯一
            + 连通图的任意一个环中所包含的边的权值均不相同
        + 充要条件：不存在一条非最小生成树上的边，满足该边的权值与其两端顶点在最小生成树上的路径最小边权相等。
    + 最小生成树的边数=顶点数$-1$
        + 减去一条则不连通，增加一条则会出现回路。
    + 若一个连通图本身就是一棵树，则其最小生成树就是其本身。
    + 只用连通图才有生成树，非连通图只有生成森林。

18. Prim算法适用于边稠密图，Kruskal算法适用于边稀疏顶点多的图

19. 单源最短路径：$BFS$算法（无权图），$Dijkstra$迪杰斯特拉算法（无负权的带权图、无权图）。

20. 每对顶点间最短路径：$Floyd$弗洛伊德算法（带权图、无权图）。

    +   能解决带负权值的问题，但是不能解决带有负权回路的图，即有负权值的边组成回路，这种图可能没有最短路径。

21. 最短路径一定是简单路径（不存在环）。但是无论有没有环的有向图与是否存在最短路径无关。

22. $AOE$网中若有多条关键路径，则应加快包含所有关键路径上的关键活动才能缩短工期。

23. 关键路径是从源点到汇点路径长度量长的路径。

### 排序算法总结

![image-20230724194611557](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307241946664.png)

#### 稳定性

+ 稳定的排序
    1.   基数排序
    2.   冒泡排序
    3.   直接插入排序
    4.   折半插入排序
    5.   归并排序
    6.   桶排序
    7.   基数排序
+ 不稳定的排序
    1.   堆排序
    2.   快速排序
    3.   希尔排序
    4.   直接选择排序

#### 适用性

+ 适合链表的排序算法
    1.   冒泡排序
    2.   选择排序
    3.   直接插入排序
    4.   计数排序
    5.   基数排序
+ 不适合链表的排序算法
    1.   希尔排序
    2.   快速排序
    3.   归并排序
    4.   二分插入排序
         +   以及包含所有需要二分查找优化的排序算法
+ 可以用于链表排序但不建议使用的排序算法：**堆排序**。

#### 每个算法的特点

一趟排序定义为对尚未确定最终位置的所有元素进行一遍处理

1.   直接插入排序
     1.   在待排序的元素序列基本有序的前提下，直接插入排序效率最高的。
     2.   直接插入排序进行$n$躺后能保证前$n+1$个元素是有序的，但是不能保证其都在最终的位置上。
2.   二分插入排序：二分插入排序进行$n$趟后能保证前$n+1$个元素是有序的，但是不能保证其都在最终的位置上。
3.   希尔排序特性
     1.   希尔排序在最后一趟前都不能保证元素在最后的位置上。
     2.   希尔排序在最后一趟前都不能保证元素是有序的。
4.   冒泡排序特性：冒泡排序产生的序列全局有序，$n$趟排序后第$n$个元素到达最终的位置上，前$n$个或后$n$个位置的元素确定。
     +   冒泡排序在本身有序时为全排序算法交换次数最小，为0
5.   快速排序
     1.   所以如果初始序列是有序的或逆序的，则快速排序性能最差（速度最慢），退化到$O(n^2)$
          +   性能与分区处理顺序无关
     2.   假设每趟排序确定的元素都不挨在一起，第n趟排序会确定在$2^{n-1}$个最终位置的元素
     3.   快速排序不产生有序子序列。
     4.   枢轴元素到达的位置是不确定的，但是每次都会到其最终的位置上，第$n$趟至少有$n$个元素到最终位置上。
6.   选择排序：简单选择排序和堆排序
     +   选择排序算法的比较次数始终为$\dfrac{n(n-1)}{2}$，与序列状态无关。
7.   堆排序
     1.   堆的叶子结点范围是$\lfloor\log_2n\rfloor+1\sim n$
     2.   适合大量数据进行排序。
8.   归并排序
     1.   二路归并排序在内部排序算法中空间消耗最大
     2.   二路归并排序是一棵倒立的二叉树
     3.   对于$N$个元素进行$k$路归并排序时，排序的趟数$m$满足$k^m = N$\;从而$m =\log_kN$\;又考虑到$m$为整数，所以$m=\lceil\log_kN\rceil$
9.   基数排序
     1.   基数排序不是基于比较的排序算法，比较次数为0
     2.   只能对整数进行排序。
     3.   元素的移动次数与关键字的初始排列次序无关

### 内部排序算法的应用

1.    若$n$较小，可采用直接插入排序或简单选择排序
      +   由于直接插入排序所需的记录移动次数较简单选择排序的多，因而当记录本身信息量较大时，用简单选择排序较好。
2.    若文件的初始状态已按关键字基本有序，则选用直接插入或冒泡排序为宜。
3.    若$n$较大，则应采用时间复杂度为$O(log_2 n)$的排序方法：快速排序、堆排序或归并排序
      +   快速排序被认为是目前基于比较的内部排序方法中最好的方法，当待排序的关键字随机分布时，快速排序的平均时间最短
      +   堆排序所需的辅助空间少于快速排序，并且不会出现快速排序可能出现的最坏情况，这两种排序都是不稳定的
      +   若要求排序稳定且时间复杂度为$O(log_2 n)$，则可选用归并排序
      +   但本章介绍的从单个记录起进行两两归并的排序算法并不值得提倡，通常可以将它和直接插入排序结合在一起使用。先利用直接插入排序求得较长的有序子文件，然后两两归并
      +   直接插入排序是稳定的， 因此改进后的归并排序仍是稳定的
4.    在基于比较的排序方法中，每次比较两个关键字的大小之后，仅出现两种可能的转移， 因此可以用一棵二叉树来描述比较判定过程，由此可以证明
      +   当文件的$n$个关键字随机分布时，任何借助于“比较”的排序算法，至少需要$O(n\log_2 n)$的时间
5.    若$n$很大，记录的关键字位数较少且可以分解时，采用基数排序较好
6.    当记录本身信息量较大时，为避免耗费大量时间移动记录，可用链表作为存储结构。

## 数据结构及其操作总结

### 数组

#### 将一个数组前后翻转

![d6eefad9161c404ba2b7a72f8f2b7f4c.png](https://ucc.alicdn.com/pic/developer-ecology/e5679e21020d4d459d68227732124238.png)

```cpp
bool Delete_Min(int A[], n, &min) {
    if (!n) return false;    //数组长度为0，返回false
    int temp = INT_MAX, m;    //INT_MAX为int类型的最大值
    for (int i = 0; i < n; i++) {    //遍历数组，找到数组当前的最小元素
        if (A[i] < temp) {
            temp = A[i];    //更新数组最小值
            m = i;    //记录数组下标
        }
    }
    min = temp;    //min保存最小值
    A[m] = A[n - 1];    //m用数组中最后一个元素替换
    return true;
}
```

#### 删除数组中值为x的元素

![5d0005dde27f442cb56bea3cfae7c26a.png](https://ucc.alicdn.com/pic/developer-ecology/3955e7b5b3374b7190cd1deec5d21e71.png)

```cpp
int DeleteX(int A[], x, n){
    int i = 0, j = 0;
    for (i = 0; i < n; i++) {
        if (A[i] != x) {    //当前元素的值不为x
            A[j] = A[i];    //将其保存到数组下标为j的元素中
            j++;
        }
    }
    n = j;    
    return n;    //返回删除x后的数组元素个数
}
```

#### 将两个有序数组合并成一个有序数组

![9b3df95c179542c994fc309f9783494e.png](https://ucc.alicdn.com/pic/developer-ecology/29f915168e7c42d8872298bc0c9406df.png)

```cpp
int* Merge(int A[], B[], lenA, lenB) {
    int *C = (int*)malloc((lenA + lenB) * sizeof(int));
    int a = 0, b = 0, c = 0;
    for (c = 0; a < lenA && b < lenB; c++) {    //选择两个数组中的较小值放入数组C中
        if (A[a] <= B[b]) C[c] = A[a++];
        else C[c] = B[b++];
    }
    while (a < lenA) C[c++] = A[a++];    //将剩余数组放入C中
    while (b < lenB) C[c++] = B[b++];
    return C;
}
```

#### 真题

![bd99ceb964a943a8b2cf8fb35e95363e.png](https://ucc.alicdn.com/pic/developer-ecology/f1f29528f97e49629b521850a2496551.png)

```cpp
void ans(int A[], n, p){
    int B[n], i, j;
    for (i = 0, j = p; j < n; i++, j++) B[i] = A[j];    //数组后部分前移
    for (j = 0; j < p; i++, j++) B[i] = A[j];    //数组前部分后移
    for (i = 0; i < n; i++) cout << B[i];    //输出循环前移后的数组
}
```

![63e9f7fd08b448d08b08ae0714bc92d1.png](https://ucc.alicdn.com/pic/developer-ecology/1d77b89b86034d13b1620d4e35120e3e.png)

```cpp
int res(int A[], int B[], int n){
    int i, j, k, mid;
    for (i = 0, j = 0, k = 0; k < n; k++){
        if (A[i] <= B[j]) {    //当前A数组的元素小，保存A[i]
            mid = A[i];
            i++;    
        } else {    //当前B数组的元素小，保存B[j]
            mid = B[j];
            j++;
        }
    }
    return mid;
}
void Qsort(int A[], L, R){
    if (L >= R) return;    //当前数组区间<= 1，返回
    随机选择数组中一个元素和A[L]交换    //快速排序优化，使得基准元素的选取随机
    int key = A[L], i = L, j = R;    //选择A[L]作为基准元素，i和j分别为左右指针
    while(i < j) {
        while (i < j && key < A[j]) j--;
        while (i < j && A[i] <= key) i++;
        if (i < j) swap(A[i], A[j]);    //交换A[i]和A[j]
    }
    swap(A[i], A[L]);
    Qsort(A, L, i - 1);    //递归排序左区间
    Qsort(A, i + 1, R);    //递归排序右区间
}
 
int ans(int A[], int B[], int n) {
    int C[2n], i, j;
    for (i = 0; i < n; i++) C[i] = A[i];    //复制数组A和数组B的元素
    for (j = 0; j < n; i++, j++) C[i] = B[j];
    Qsort(C, 0, 2n - 1);    //对数组C进行快速排序
    return C[n - 1];    //返回中位数
}
```

![e24f9027d944449f9f267da69c9a9428.png](https://ucc.alicdn.com/pic/developer-ecology/63f78de6f77b4127bef2f5d7eadbf54c.png)

```cpp
int ans(int A[], n){
    int count[n];
    for (int i = 0; i < n; i++) count[i] = 0;    //初始化count数组
    //遍历A数组，其元素的值作为count数组下标的元素+1，表示该元素在A数组中出现次数
    for (int i = 0; i < n; i++) count[A[i]]++;   
    for (int i = 0; i < n; i++) {    //当前元素出现次数符合主元素定义
        if (count[i] > n / 2) return i;    //返回i，即该元素的值
    }
    return -1;    //没有元素符合主元素定义
}
```

![9fd06f5530b2430da410e14815da0fdd.png](https://ucc.alicdn.com/pic/developer-ecology/53d2687772284ef7b135b2610b6665cc.png)

```cpp
int ans(int A[], n) {
    bool B[n + 2];    //B用来标记数组中出现的正整数
    for (int i = 0; i < n; i++) B[i] = false;    //初始化B数组
    for (int i = 0; i < n; i++) {
        if (A[i] > 0) B[A[i]] = true;    //该元素大于0，则在B数组中标记为已经出现
    }
    for (int i = 1; i < n; i++) {
        if (B[i]  false) return i;    //返回数组B中第一个false的元素下标
    }
}
void Qsort(int A[], L, R) {
    if (L >= R) return; //数组区间<= 1，返回
    随机选择数组中一元素和A[L]交换;//快排优化，使得基准元素的选取随机
    int key = A[L], i = L, j = R;   //A[L]为基准元素，ij为左右指针
    while (i < j) {
        while (i < j && key < A[j]) j--;
        while (i < j && A[i] <= key) i++;
        if (i < j) Swap(A[i], A[j]);    //交换A[i]和A[j]
    }
    Swap(A[L], A[i]);
    Qsort(A, L, i - 1); //递归排序左区间
    Qsort(A, i + 1, R); //递归排序右区间
}
 
int ans(int A[], n) {
    Qsort(A, 0, n - 1); //快速排序
    int i = 0;
    while (A[i] <= 0) i++;  //找到数组中第一个大于0的元素
    if (n  i) return 1;   //数组中没有元素大于0，返回1
    else {
        if (A[i] != 1) return 1;    //第一个整数不是1，则返回1
        else {    //第一个整数为1，找到数组中正整数第一个间断点
            int j = i + 1;
            while (j < n) {
                if (a[j]  a[j - 1]) j++;    //相邻元素相等
                else if (a[j]  a[j - 1] + 1) j++;    //相邻元素是连续数
                else return A[j - 1] + 1;    //相邻元素是间断点
            }//while
        }//else
    }//else
}
```

![6ebfa00824c04488a0e1538e123a20f8.png](https://ucc.alicdn.com/pic/developer-ecology/d78bbb3d6e5a475da13757ccc2d0224c.png)

```cpp
int Dis(int a, b, c){
    int res = abs(a - b) + abs(a - c) + abs(b - c);    //计算绝对值
    return res;
}
 
int Ans(int A[], int B[], int C[], int n1, int n2, int n3){
    int min = INT_MAX, i, j, k, temp;    //min取整型的最大值
    for (int i = 0; i < n1; i++) {    //循环遍历数组A
        for (int j = 0; j < n2; j++) {    //循环遍历数组B
            for (int k = 0; k < n3; k++) {    //循环遍历数组C
                temp = Dis(A[i], B[j], C[k]);
                if (temp < min) min = temp;    //当前元素之间的距离更小，更新最小距离
            }//for
        }//for
    }//for
    return min;    //返回最小距离
}
```

### 链表

#### 链表的数据结构定义

##### 单链表

```cpp
typedef struct LNode {
    struct LNode *next;
    int data;
}LNode, *LinkList;
```

##### 双链表

```cpp
typedef struct LNode {
    struct LNode *prior, *next;
    int data;
}LNode, *LinkList;
```

#### 链表的操作

##### 头插法（插入到链表头）

```cpp
void HeadInsert(LinkList &L, int key) {
    LNode *p = (LNode*)malloc(sizeof(LNode));
    p->data = key;
    p->next = L->next;
    L->next = q;
}
```

##### 尾插法（插入到链表尾）

```cpp
void TailInsert(LinkList &L, int key) {
    LNode *q = (LNode*)malloc(sizeof(LNode);
    q->data = key;
    q->next = NULL;
    LNode *p = L->next, *pre = L;
    while (!p) {
        pre = p;
        p = p->next;
    }
    pre->next = q;
}
```

##### 链表逆置（头插法实现）

```cpp
void Reverse(LinkList &L) {
    LNode *p = L->next, *q = NULL;
    L->next = NULL;    //将L表断开
    while (!p) {
        q = p->next;    //q指向p的下一个结点
        p->next = L->next;    //头插法
        L->next = p;
        p = q;
    }
}
```

##### 链表的遍历

```cpp
LNode *p = L->next;
while (!p) {
    visit(p);
    p = p->next;
}
```

##### 链表的删除

```cpp
void Delete(LinkList &L, int &key) {
    LNode *p = L->next, *pre = L;
    移动p和pre到指定结点    //pre指向p的前驱结点
    key = p->data;    //key保存p的data领
    pre->next = p->next;    //pre的next指针指向p的后继节点
    free(p);
}
```

#### 链表算法题

##### 删除值为x的结点

```cpp
void DeleteX(LinkList &L, int x){
    LNode *p = L->next, *pre = L;
    while (p) {
        if (p->data  x) {    //当前元素值为x
            pre->next = p->next;
            free(p);
            p = pre->next;
        } else {    //当前元素值非x，p和pre向后移动
            p = p->next;
            pre = pre->next;
        }
    }
}
```

##### 单链表就地逆置

![99e7afe97aa04a699c22f929a0b6f198.png](https://ucc.alicdn.com/pic/developer-ecology/cf008a3081864eed851320ddd03c840b.png)

```cpp
void reverse(LinkList &L){
    LNode *p = L->next, *q;
    L->next = NULL;    //断链
    while (p) {
        q = p->next;    //q指向p的下一个结点
        p->next = L->next;    //头插法
        L->next = p;
        p = q;
    }
}
```

##### 将链表排序

![83649d0dd1504382b5f534f74b959ff0.png](https://ucc.alicdn.com/pic/developer-ecology/0ad80b8289804ac0896e0f83064c76ff.png)

```cpp
LNode *Sort(LinkList L) {
    LNode* p = (LNode*)malloc(sizeof(Lnode));
    p->next = NULL;
    LNode* t = L->next, * tpre = L, *min, *minpre, *r = p;
    int m = INT_MAX;
    while (t) {
        while (t) {    //遍历链表
            if (t->data < m) {  //更新最小值结点
                min = t;
                minpre = tpre;
                m = t->data;
            }//if
            tpre = t;
            t = t->next;
        }//while
        minpre->next = min->next;   //将min从L中删除
        r->next = min;  //将min插入p
        r = min;    //r后移
        m = INT_MAX;    //重新初始化
        t = L->next;
        tpre = L;
    }//while
    r->next = NULL;
    return p;
}
```

##### 拆分链表

![87bccb5146f44a919002d0af76a41f96.png](https://ucc.alicdn.com/pic/developer-ecology/e90ff6bfebfb4654a0024ed2e615fc0b.png)

```cpp
LNode* (LinkList &L) {
    LNode *p = (LNode*)malloc(sizeof(LNode);
    p->next = NULL;    //p为新链的头结点
    LNode *q = L->next, *t = NULL, *r = L;
    r->next = NULL;    //r结点始终指向L的最后一个结点
    while (q) {
        t = q->next;
        r->next = q;    //奇数结点尾插法
        r = q;
        q = t;
        t = q->next;
        q->next = p->next;    //偶数节点头插法
        p->next = q;
        q = t;
    }
    r->next = NULL;    //将r的next指针置空
    return p;
}
```

##### 删除链表中的重复元素

![897f91ea488e437280ebd1fddfc4c064.png](https://ucc.alicdn.com/pic/developer-ecology/c7447168a1df4a3ca4568dc3dc3b9890.png)

```cpp
void Delete(LinkList &L) {
    LNode *p = L->next;
    while (p) {
        LNode *post = p->next;    //post指向p的下一个结点
        while (post && post->data  p->data) {    //post存在并且值和p相等时
            LNode *temp = post;    //temp指向post
            post = post->next;    //post向后移动
            p->next = post;    //将p的下一个结点修改为post
            free(temp);
        }
        p = p->next;
    }
}
```

##### 将两个递增链表合并成一个递减链表

![31bcee4271af4b03b808c33a52267356.png](https://ucc.alicdn.com/pic/developer-ecology/285714f360c9427cb0aecad9257ab59d.png)

```cpp
void Merge(LinkList &L1, LinkList L2) {
    LNode *p = L1->next, *q = L2->next, *temp;
    L1->next = NULL;    //L1断链
    while (p && q) {
        if (p->data <= q->data) {    //当前p指向的元素更小
            temp = p->next;    //temp指向p的下一个结点
            p->next = L1->next;    //将p用头插法插入L1
            L1->next = p;
            p = temp;    //p指向temp
        } else {    //当前q指向的元素更小
            temp = q->next;
            q->next = L1->next;
            L1->next = q;
            q = temp;
        }
    }//while
    while (p) {    //将剩余节点插入L1
        temp = p->next;
        p->next = L1->next;
        L1->next = p;
        p = temp;
    }
    while (q) {
        temp = q->next;
        q->next = L1->next;
        L1->next = q;
        q = temp;
    }
    return;
}
```

##### 将两个递增链表合并为一个递增链表

```cpp
LNode *Merge(LinkList L1, LinkList L2) {
    LNode *p = L1->next, *q = L2->next, *r, *temp;
    LNode *L = (LNode*)malloc(sizeof(LNode));
    L->next = NULL;
    r = L;
    while (p && q) {
        if (p->data <= q->data) {    //当前p指向的结点小于等于q
            temp = p->next;
            r->next = p;    //p尾插法插入L中
            r = p;
            p = temp;
        } else {
            temp = q->next;
            r->next = q;
            r = q;
            q = temp;
        }
    }
    while (p) {    //插入剩余结点
        temp = p->next;
        r->next = p;
        r = p;
        p = temp;
    }
    while (q) {
        temp = q->next;
        r->next = q;
        r = q;
        q = temp;
    }
    r->next = NULL;    //将r的next指针置空
    return L;
}
```

##### 判断链表是否对称

![68d0f4f27e6d4417a2c3776c06645b4f.png](https://ucc.alicdn.com/pic/developer-ecology/0dd249a58f8a4f669dd53e007a2381ce.png)

```cpp
bool ans(LinkList L) {
    LNode* post = L->prior, * pre = L->next;    //前后指针
    //表中元素为奇数时，终止条件为两者移动到同一结点
    //表中元素为偶数时，终止条件为两者后指针的next指向前指针
    while (post != pre && post->next != pre) {    
        if (post->data != pre->data) return false;    //前后指针的指针域不相等
        pre = pre->next;    //前指针前移
        post = post->prior;    //后指针后移
    }
    //表对称
    return true;
}
bool ans(LinkList L) {
    LNode* p = L->next;
    int len = 0;    //记录表中的元素个数
    while (p != L) {
        p = p->next;
        len++;
    }
    int a = (int*)malloc(len * sizeof(int));    //定义跟链表结点个数相等的长度的数组
    len = 0;
    p = L->next
    while (p != L) {    //遍历链表，用数组保存链表中每个结点的值
        a[len] = p->next;
        p = p->next;
    }
    //遍历数组，前后指针指向元素的值不相等，返回false
    for (int i = 0, j = len - 1; i < j; i++, j--) {
        if (a[i] != a[j]) return false;
    }
    return true;
}
```

##### 依次输出链表中结点值最小的元素

![da5a120bd343499eb0f1dd383d35b0a1.png](https://ucc.alicdn.com/pic/developer-ecology/cfef721ffa074dbf896ef02ca1c10b0e.png)

```cpp
void DeleteMin(LinkList &L) {
    LNode *p = L->next, *ppre = L->next, *min, *minpre;
    while (L->next != L) {
        p = L->next;
        ppre = L;
        int tempMin = INT_MAX;    //当前最小值
        while (p != L) {
            if (p->data < tempMin) {    //当前结点值更小，更新最小结点
                min = p;
                minpre = ppre;
            }    //p向后移动
            ppre = p;
            p = p->next;
        }
        cout << min->data;    //输出最小结点的值
        minpre->next = min->next;    //删除min结点
        free(min);
    }//while
    free(L);    //删除头结点
}
```

#### 真题

![a822bd0933d74051bf109284bedcdd31.png](https://ucc.alicdn.com/pic/developer-ecology/002ad8290b744e4d892d20daa0589d5d.png)

```cpp
void ans(LinkList L, int k){
    LNode *p = L->link, *q = L->link;
    for (int i = 0; i < k; i++) p = p->link;    //p指针向后移动k个结点
    while (p) {
        p = p->link;
        q = q->link;
    }
    cout << q->data;
}
```

![0d293bb78b334ecf8ee9dfdc7392f538.png](https://ucc.alicdn.com/pic/developer-ecology/09252317536646d0b524d602b6648445.png)

```cpp
void ans(LinkList str1, LinkList str2) {
    LNode *p = str1->next, *q = str2->next;
    int len1 = 0, len2 = 0;
    while (p) {    //遍历str1，得到str1的长度
        len1++;
        p = p->next;
    }
    while (q) {    //遍历str2，得到str2的长度
        len2++;
        q = q->next;
    }
    int len = abs(len1 - len2);    //得到两表长度之差
    p = str1->next;    //重置pq指向第一个结点
    q = str2->next;
    if (len1 >= len2) {    //长表向前移动，使得两表剩余元素相等
        for (int i = 0; i < len; i++) p = p->next;
    }
    else {
        for (int i = 0; i < len; i++) q = q->next;
    }
    while (p) {    //遍历剩余结点，找到两者指向的第一个共同结点
        if (p  q) return p;
        p = p->next;
        q = q->next;
    }
    return NULL;    //两者没有共同后缀
}
```

![c0f0efeb281542c5863afd80a37ddea1.png](https://ucc.alicdn.com/pic/developer-ecology/f114e51e74ec41bfaba85208e8a06d54.png)

```cpp
void ans(LinkList &L){
    bool A[n + 1];    //长度为n + 1的数组，用来标记该数是否出现过
    for (int i = 0; i < n + 1; i++) A[i] = false;    //初始化A数组
    LNode *p = head->next, *pre = head;
    while (p) {
        int t = abs(p->data);    //取当前结点值的绝对值
        if (A[t]) {    //该值出现过，删除该结点
            LNode *r = p->next;
            pre->next = r;
            free(p);
            p = r;
        } else {    //该值没有出现过，在数组A中标记该值，p和pre向后移动
            A[t] = true;
            pre = p;
            p = p->next;
        }
    }//while
}
```

![6e9952902c0a498ab760546c23af4047.png](https://ucc.alicdn.com/pic/developer-ecology/a441f435d41547eca3cb564a48fc2cc8.png)

```cpp
void ans(NODE *L) {
    NODE* p = L->next, *f = L->next, *s = L->next, *q, *t;
    while (f->next->next && f->next) {  //找到前半链的最后一个结点
        f = f->next->next;    //快指针移动两个结点
        s = s->next;    //慢指针移动一个结点
    }
    q = s->next;    //q指向后半链的第一个结点
    s->next = NULL; //前半链后半链断开
    LNode* post = (NODE*)malloc(sizeof(NODE));
    post->next = NULL;
    while (q) { //后半链逆置
        t = q->next;
        q->next = post->next;
        post->next = q;
        q = t;
    }
    q = post->next; //q指向逆置后的后半链的第一个结点
    while (q) {
        r = q->next;    //r指向后半链的下一个结点
        t = p->next;    //t指向前半链下一个插入位置
        q->next = p->next;
        p->next = q;
        q = r;  //重置pq
        p = t;
    }
}
```

### 栈

#### 栈的数据结构定义

##### 顺序栈

```cpp
#define MAXSIZE 100
 
typedef struct Stack {
    int data[MAXSIZE];
    int top = -1;
}Stack;
```

##### 链栈

```cpp
typedef struct LStack {
    int data;
    struct LStack *next;
}SNode, *LStack;
```

#### 顺序栈的操作

##### 栈的初始化

```cpp
void InitStack (Stack &S) {
    S.top = -1;
}
```

##### 入栈

```cpp
bool Push(Stack &S, int key) {
    if (S.top  MAXSIZE - 1) return false;    //栈满
    S.data[++top] = key;
    return true;
}
```

##### 出栈

```cpp
bool Pop (Stack &S, int &key) {
    if (S.top  -1) return false;    //栈空
    key = S.data[top--];
    return true;
}
```

##### 判断栈空

```cpp
bool IsEmpty (Stack S) {
    if (S.top  -1) return true;
    else return false;
}
```

#### 链栈的基本操作

##### 初始化

```cpp
void InitStack (LStack &S) {
    SNode *s = (SNode*)malloc(Sizeof(SNode));
    S->next = NULL;
}
```

##### 入栈

```cpp
void Push (LStack &S, int key) {
    SNode *p = (SNode*)malloc(sizeof(SNode));
    p->data = key;
    p->next = S->next;    //头插法
    S->next = p;
}
```

##### 出栈

```cpp
bool Pop (LStack &S, int &key) {
    if (S->next  NULL) return false;    //栈空
    SNode *p = S->next;
    key = p->data;    //key保存栈顶元素的值
    S->next = p->next;
    free(p);
}
```

##### 判断栈空

```cpp
bool IsEmpty(LStack &S) {
    if (S->next  NULL) return true;
    else return false;
}
```

### 队列

#### 队列的数据结构定义

##### 顺序队列

```cpp
#define MAXSIZE 100
typedef struct Queue {
    int data[MAXSIZE];
    int front, rear;
}Queue;
```

##### 链式队列

```cpp
typedef struct LNode{
    struct LNode *next;
    int data;
}LNode;
 
typedef struct Queue{
    LNode *front, *rear;
}Queue;
```

#### 顺序队列的基本操作

##### 初始化

```cpp
void InitQueue(Queue &Q){
    Q.front = Q.rear = 0;
}
```

##### 入队

```cpp
bool EnQueue(Queue &Q, int key){
    if (Q.front  (Q.rear + 1) % MAXSIZE) return false;    //队满
    Q.data[rear] = key;
    Q.rear = (Q.rear + 1) % MAXSIZE;
    return true;
}
```

##### 出队

```cpp
bool DeQueue(Queue &Q, int &key){
    if (Q.rear  Q.front) return false;    //队空
    key = Q.front;
    Q.front = (Q.front + 1) % MAXSIZE;
    return true;
}
```

##### 判断队空

```cpp
bool IsEmpty(Queue Q){
    if (Q.front  Q.rear) return true;
    else return false;
}
```

#### 链式队列的基本操作

##### 初始化

```cpp
void InitQueue(Queue &Q){
    Q.front = Q.rear = (LNode*)maloc(sizeof(LNode));
    Q.front->next = NULL;
}
```

##### 入队

```cpp
void Queue(Queue &Q, int key){
    LNode *p = (LNode*)malloc(sizeof(LNode));    //申明一个新结点
    p->data = key;
    p->next = NULL;
    Q.rear->next = p;    //尾插法插入到rear后
    Q.rear = p;    //更新rear
}
```

##### 出队

```cpp
bool DeQueue(Queue &Q, int &key){
    if (Q.front  Q.rear) return false;    //队空
    LNode *p = Q.front->next;
    key = p->data;    //保存队首元素的数据
    Q.front->next = p->next;
    if (Q.rear  p) Q.rear = Q.front;    //队列中只有一个元素
    free(p);
    return true;
}
```

##### 判断队空

```cpp
bool IsEmpty(Queue Q){
    if (Q.rear  Q.front) return true;
    else return false;
}
```

### 树

#### 树的数据结构定义

##### 二叉树的链式存储

```cpp
typedef struct BiTree{
    sturct BiTree *lchild, *rchild;    //左右孩子指针
    int value;    //结点数据
}BiTNode, *BiTree;
```

##### 二叉树的顺序存储

```cpp
#define MAXSIZE 100
 
typedef struct TreeNode{
    int value;    //结点数据
    bool IsEmpty;    //该结点是否存在
}TreeNode;
 
void InitTree(TreeNode T[], int len){
    for (int i = 0; i < len; i++) T[i].IsEmpty = true;    //将该结点初始化为空结点
}
 
 
int main(){
    TreeNode T[MAXSIZE];    //申明一个长度为MAXSIZE的TreeNode数组
    InitTree(T);    //初始化树
    ...
}
```

##### 双亲表示法

```cpp
#define MAXSIZE 100    //树中最多结点数
 
typedef struct TreeNode{
    int data;    //结点数据
    int parent;    //该结点的双亲结点在数组的下标
}TreeNode;
 
typedef struct Tree{
    TreeNode T[MAXSIZE];    //长度为MAXSIZE的TreeNode类型的数组
    int treeNum;    //结点数
}Tree;
```

##### 孩子表示法

```cpp
#define MAXSIZE 100
 
//孩子结点
typedef struct Child{
    int index;    //该结点的编号
    struct Child *next;    //指向该结点的下一个孩子结点的指针
}Child;
 
//结点信息
typedef struct TreeNode{
    Child *firstTNode;    //指向该结点的第一个孩子结点的指针
    int data;    //该结点数据
}TreeNode;
 
TreeNode T[MAXSIZE];    //定义一个长度为MAXSIZE的树
```

##### 孩子兄弟表示法

```cpp
#define MAXSIZE 100
 
typedef struct CSNode{
    struct CSNode *firstChild, *nextSibling;    //指向第一个孩子和右兄弟节点
    int data;    //该结点数据
}CSNode;
```

##### 线索二叉树

```cpp
typedef struct ThreadNode{
    struct ThreadNode *lchild, *rchild;    //左右孩子指针
    int ltag, rtag;    //左右线索标志
    int data;    //结点数据    
}ThreadNode, *ThreadTree;
```

#### 二叉树的基本操作

##### 先序遍历

```cpp
void PreOrder(BiTree T){
    if (T) {
        visit(T);
        PreOrder(T->lchild);
        PreOrder(T->rchild);
    }
}
```

##### 中序遍历

```cpp
void InOrder(BiTree T){
    if (T) {
        InOrder(T->lchild);
        visit(T);
        InOrder(T->rchild);
    }
}
```

##### 后序遍历

```cpp
void PostOrder(BiTree T){
    if (T) {
        PostOrder(T->lchild);
        PostOrder(T->rchild);
        visit(T);
    }
}
typedef struct Stack{
    BiTNode *Node[MAXSIZE];
    int top;
}Stack;
 
void PostOrder(BiTree T){
    Stack S;
    InitStack(S);
    BiTNode *p, *pre;
    while (p || !IsEmpty(S)）{
        if (p) {    //往左下走到尽头
            Push(p);    //p入栈
            p = p->lchild;    //进入其左子树
        } else {
            GetTop(S, p);    //查看栈顶元素
            //栈顶元素的右孩子存在，并且不是上一个访问的结点
            if (p->rchild && p->rchild != pre) {
                p = p->rchild;    //进入栈顶元素的右子树
                Push(p);    //该结点入栈
                p = p->lchild;    //进入该结点左子树
            } else {    //栈顶元素的右孩子被访问过
                Pop(S, p);    //弹出栈顶元素
                visit(p);    //访问该结点
                pre = p;    //用pre标记该结点
                p = NULL;    //将p置空
            }//if
                
        }//if
    }//whil
}
```

##### 层次遍历

```cpp
void LevelOrder(BiTree T){
    InitQueue(Q);
    EnQueue(Q, T);
    BiTNode *p;
    while (!IsEmpty(Q)) {
        DeQueue(Q, p);
        visit(p);
        if (p->lchild) EnQueue(Q, p->lchild);
        if (p->rchild) EnQueue(Q, p->rchild);
    }
}
```

#### 二叉树算法题

##### 计算二叉树中双分支结点的个数

![cafb376f570b4fada2460af133fee7fa.png](https://ucc.alicdn.com/pic/developer-ecology/fe58cbb22be1451089e0d4f421fb4f0f.png)

```cpp
int count = 0;    //双分支结点个数
 
void PreOrder(BiTree T){
    if (T) {
        //当前结点的左右孩子都存在，count++
        if (T->lchild && T->rchild) count++;
        if (T->lchild) PreOrder(T->lchild);    //递归遍历左子树
        if (T->rchild) Preorder(T->rchild);    //递归遍历右子树
}
 
void ans(BiTree T) {
    PreOrder(T);    //先序遍历该树
    cout << count;    //输出双分支结点个数
}
```

##### 交换二叉树中所有左右子树

![a64f890e722b45f78fe588f23ecca770.png](https://ucc.alicdn.com/pic/developer-ecology/29f484a107f9425d931ee48d8c82403a.png)

```cpp
void PostOrder(BiTree &T){
    if (T) {
        PostOrder(T->lchild);
        PostOrder(T->rchild);
        BiTNode *t = T->lchild;
        T->lchild = T->rchild;
        T->rchild = t;
    }
}
 
void ans(BiTree &T){
    Post(Order(T));
    return;
}
```

##### 求先序遍历第k个元素

![6805b416eefa4dc9a7aa4ea3b053c7c2.png](https://ucc.alicdn.com/pic/developer-ecology/a56365e020874255960f2fc2851f6694.png)

```cpp
int t = 0;
int res = 0;
 
void PreOrder(BiTree T) {
    if (T) {
        t--;
        if (t  0) res = T->data;    //第k个结点，用res保存当前结点的值
        PreOrder(T->lchild);    //递归访问左子树
        PreOrder(T->rchild);    //递归访问右子树
    }
}
 
void ans(BiTree T, int k) {
    t = k;
    PreOrder(T);
    cout << res;    //输出第k个结点的值
}
```

##### 删去值为x的子树

![54aaa4bce6424c299e487ebbaa87ae16.png](https://ucc.alicdn.com/pic/developer-ecology/6e7c68c24258467982ad6912c3813bf8.png)

```cpp
int k;
 
void Delete(BiTree &T){    //后序遍历的方式删除结点
    if (T) {
        DeleteX(T->lchild);
        DeleteX(T->rchild);
        free(T);
    }
}
 
void PreOrder(BiTree &T) {
    if (T) {
        BiTNode *t;
        if (T->lchild->data  k) {    //左子树的值为x，删除左子树
            t = T->lchild;
            T->lchild = NULL;
            Delete(t);
        }
        if (T->rchild->data  k) {    //右子树的值为x，删除右子树
            t = t->rchild;
            T->rchild = NULL;
            Delete(t);
        }
        if (T->lchild) PreOrder(T->lchild);    //递归遍历左子树
        if (T->rchild) PreOrder(T->rchild);    //递归遍历右子树
    }//if
} 
 
 
void ans(BiTree &T, int x){
    k = x;
    if (T->data  x) {    //根节点的值为x，删除整个树并返回
        Delete(T);
        return;
    } else PreOrder(T);    //先序遍历x
}
void Delete(BiTree &T) {    //后序遍历，并删除结点
    if (T) {
        Delete(T->lchild);
        Delete(T->rchild);
        free(T);
    }
}
 
void LevelOrder(BiTree &T, int x){
    if (T->data  x) {    //根节点的值为x，删除整个树，并返回
        Delete(T);
        return;
    }
    Queue Q;
    InitQueue(Q);    //初始化队列
    BiTNode *p = T;
    EnQueue(Q, p);    //根节点入队
    while (!IsEmpty(Q)) {
        DeQueue(Q, p);
        if (p->lchild) {
            if (p->lchild->data  x) {
                BiTNode *q = p->lchild;
                p->lchild = NULL;    //左孩子指针置空
                Delete(q);    //以q为根节点的子树
            } else EnQueue(Q, p);
        }
        if (p->rchild) { {}
            if (p->rchild->data  x) {
                BiTNode *q = p->rchild;
                p->rchild = NULL;
                Delete(q);
            } else EnQueue(Q, p);
        }
    }//while
}
```

##### 查找二叉树中两个结点的公共祖先结点

![59da31125bff423a875a1b8ce85b8673.png](https://ucc.alicdn.com/pic/developer-ecology/299b6200ef5a448286679ea62db7933e.png)

```cpp
BiTNode *ans(BiTree ROOT, BiTNode *p, BiTNode *q) {
    Stack S, Sp, Sq;    //Sp和Sq分别用来保存p和q的祖先结点
    S.top = -1; //初始化队列
    BiTNode* t = ROOT, *pre = NULL;
    while (t || S.top >= 0) {
        if (t) {    //t结点非空
            S.data[++S.top] = t;    //t结点入队
            t = t->lchild;  //进入t的左子树
        }
        else {  //t结点为空
            t = S.data[S.top];  //查看栈顶元素
            //t的右子树存在，并且上一个访问的并不是其右子树
            if (t->rchild && t->rchild != pre) {    
                t = t->rchild;  //进入右子树
                S.data[++S.top] = t;    //入栈该结点
                t = t->rchild;  //进入左子树
            }
            else {  //右子树不存在，或者存在但是上一个访问的是右子树
                S.top--;    //出栈该结点，并访问
                if (t  p) {   //当前结点为p，保存栈中内容，即其所有祖先结点
                    for (int i = 0; i < S.top; i++) Sp.data[i] = S.data[i];
                    Sp.top = S.top;
                }
                if (t  q) {   //当前结点为q，保存栈中内容，即其所有祖先结点
                    for (int i = 0; i < S.top; i++) Sq.data[i] = S.data[i];
                    Sq.top = S.top;
                }
            }//if
        }//if
    }//while
    //自栈顶到栈顶分别遍历Sp和Sq找到最接近栈顶的相同祖先结点
    for (int i = Sp.top; i >= 0; i--) { 
        for (int j = Sq.top; i >= 0; j--) {
            if (Sp.data[i]  Sq.data[j]) return Sp.data[i];
        }
    }
    return NULL;    //无相同祖先顶点
}
```

##### 求二叉树的宽度![ccb1ae7ce764475ea764c119315612e4.png](https://ucc.alicdn.com/pic/developer-ecology/4a51f8d8033948fcb5e2bf0eea257214.png)

```cpp
typedef struct Queue{
    BiTNode *data[MAXSIZE];    //足够大的数组
    int front, rear;
}Queue;
 
int ans(BiTree T){
    if (!T) return 0;    //空树，返回0
    BiTNode *p = T;
    Queue Q;
    InitQueue(Q);    //初始化队列
    EnQueue(Q, p);    //将p入队
    //rear指向当前层的最后一个结点，count记录当前层的结点数，max记录最大结点数
    int last = Q.rear, count = 0, max = INT_MIN;    
    while (!IsEmpty(Q) {
        DeQueue(Q, p);
        count++;    //结点数+1
        if (p->lchild) EnQueue(Q, p->lchild);    //p的左子树存在，左子树入队
        if (p->rchild) EnQueue(Q, p->rchild);    //p的右孩子存在，右孩子入队
        if (last  Q.front) {    //当前结点是本层的最后一个节点
            last = Q.rear;    //last指向下一层的最后一个结点
            if (count > max) max = temp;    //更新最大结点数
            count = 0;    //结点数归零
        }
    }//while
    return max;
}
typedef struct Queue {  //足够大的非循环数组
    BiTNode *data[MAXSIZE]; //结点数组，保存每个结点
    int level[MAXSIZE]; //层数数组，记录每个结点的层数
    int front, rear;    //头尾指针
}Queue;
 
int ans(BiTree T) {
    BiTNode* p = T;
    Queue Q;
    Q.rear = Q.front = 0;   //初始化队列
    if (T) {
        Q.data[Q.rear] = T; //根节点入队
        Q.level[Q.rear] = 1;
        Q.rear++;
        while (Q.front < Q.rear) {
            p = Q.data[Q.front];    //出队队首元素
            int level = Q.level[Q.front];   //保存当前结点的层数
            Q.front++;
            if (p->lchild) {    //左孩子入队
                Q.data[Q.rear] = p->lchild;
                Q.level[Q.rear] = level + 1;
                Q.rear++;
            }
            if (p->rchild) {    //右孩子入队
                Q.data[Q.rear] = p->rchild;
                Q.level[Q.rear] = level + 1;
                Q.rear++;
            }
        }//while
        int max = INT_MIN, i = 0, level = 1;
        while (i <= Q.rear) {
            int count = 0;  //记录当前层的结点数
            while (i <= Q.rear && level  Q.level[i]) {
                count++;
                i++;
            }
            if (count > max) max = count;   //更新每层的最大结点数
            level = Q.level[i]; //更新层数，while循环结束时，i指向下一层的第一个结点
        }//while
        return max; //返回最大结点数
    }
    else return 0;  //空树，返回0
}
```

##### 求二叉树的高度

```cpp
int Get_Heigh(BiTree T) {
    int front = 0, rear = 0;    //前后指针
    BiTNode* p = T;
    BiTNode *data[MAXSIZE]; //足够大的队列，元素是二叉树结点
    data[rear++] = p;   //根节点入队
    int last = rear, level = 0; //rear标记本层最后一个结点, level记录当前层数
    while (front < rear) {  //循环直到队空
        p = data[front++];  //出队队首结点
        if (p->lchild) data[Q.rear++] = p->lchild;  //左右孩子入队
        if (p->rchild) data[Q.rear++] = p->rchild;
        if (last  front) {    //当前结点为本层的最后一个结点
            last = rear;    //标记下层的最后一个结点
            level++;    //层数+1
        }
    }//while
    return level;
}
int Get_High(BiTree T){
    if (!T) return 0;    //空树返回0
    else {
        int hl = Get_High(T->lchild);    //递归求左右子树高度
        int hr = Get_High(T->rchild);
        int maxH = max(hl, hr) + 1;    //树高等于左右子树更高的那个+1
        return maxH;
    }
}
```

##### 排序二叉树的判定

```cpp
int pre = INT_MIN;  //标记上一个结点的值，初始值为INT类型的最小值
 
int JudgeBST(BiTree T) {
    if (!T) return 1;   //空树，是排序二叉树
    else {
        int l = JudgeBST(T->lchild);    //判断左子树
        //当前值小于等于pre的值，或左子树不满足排序二叉树定义，返回0
        if (T->data <= pre|| l  0) return 0;
        pre = T->data;  //更新pre为当前结点值
        int r = JudgeBST(T->rchild);    //判断右子树
        return r;
    }
}
int A[n];   //足够大的数组，保存每个节点的值
int count = 0;  //记录结点个数
 
void InOrder(BiTree T) {
    if (T) {
        InOrder(T->lchild); 
        A[count++] = T->data;   //记录当前结点值，并且count+1
        InOrder(T->rchild);
    }
}
 
bool ans(BiTree T) {
    if (!T) return true;    //空树为排序二叉树
    else if (!T->lchild && !T->rchild) return true; //只有根节点，是排序二叉树
    else {
        InOrder(T); //中序遍历二叉树，并且记录每个结点的值
        for (int i = 0; i < count - 1; i++) {
            if (A[i] >= A[i + 1]) return false; //非排序二叉树
        }
        return true;    //排序二叉树
    }
}
```

##### 平衡二叉树的判定

```cpp
int Get_Height(BiTree T) {
    if (!T) return 0;
    else {
        int hl = Get_Height(T->lchild); //递归求左子树高度
        int hr = Get_Height(T->rchild); //递归求右子树高度
        int maxH = max(hl, hr) + 1; //树高为左右子树更高的那个 + 1
        return maxH;    
    }
}
 
bool JudgeBalance(BiTree T) {
    if (!T) return true;    //空树为平衡二叉树
    else {
        int hl = Get_Height(T->lchild); //左子树高度
        int hr = Get_Height(T->rchild); //右子树高度
        //当前结点的左右平衡因子小于等于1，递归判断其左右子树是否满足平衡二叉树
        if (abs(hl - hr) <= 1) {
            return JudgeBalance(T->lchild) && JudgeBalance(T->rchild);
        }
        //当前节点左右平衡因子大于1，则已不满足平衡二叉树，无需判断左右子树，返回false
        else return false;
    }
}
```

##### 完全二叉树的判定

```cpp
bool JudgeComplete(BiTree T) {
    BiTNode* data[MAXSIZE]; //足够大的队列
    int front = 0, rear = 0;    //头尾指针
    BiTNode* p = T; 
    data[rear++] = T;   //根节点入队
    while (front < rear) {  //循环直到队空
        p = data[front++];  //出队队首元素
        if (p) {    //p结点存在，入队左右孩子
            data[rear++] = p->lchild;
            data[rear++] = p->rchild;
        }
        else {  //p结点不存在，出队剩余元素
            while (front < rear) {
                p = data[front++];
                if p  NULL return false;  //当前元素为空，则为非完全二叉树
            }
        }
    }
    return true;
}
```

#### 真题

![6f2f5d0118df435db4b92a0872d5d5ce.png](https://ucc.alicdn.com/pic/developer-ecology/71468bc79184427c92cee88049035c67.png)

```cpp
int WPL = 0;
 
void InOrder(BiTree T, int deep){
    if (T) {
        if (!T->left && !T->right) {    //叶子结点
            WPL = WPL + T.weight * deep;    //更新WPL
        }
        if (T->left) InOrder(T->left, deep + 1);
        if (T->right) InOrder(T->right, deep + 1);
    }
}
 
int ans(BiTree T){
    InOrder(T, 0);
    return WPL;
}
int Get_WPL(BiTree root) {
    BiTNode* data[MAXSIZE]; //足够大的非循环数组
    BiTNode* p = root;
    int f = 0, r = 0, level = 0, WPL = 0, last = 0; 
    data[r++] = p;  //根节点入队
    last = r;   //last标记本层的最后一个元素
    while (f < r) {
        p = data[f++];  //队首元素出队
        //该结点为叶子结点，计算WPL
        if (!p->lchild && !p->rchild) WPL = WPL + level * p->weight;
        if (p->lchild) data[r++] = p->left; //左右孩子入队
        if (p->rchild) data[r++] = p->right;
        if (last  f) {    //该结点为本层的最后一个结点
            last = r;   //更新last和level
            level++;
        }
    }
    return WPL;
}
```

![f184da9060ed4f348f5a5207d442c7f6.png](https://ucc.alicdn.com/pic/developer-ecology/bd565a18a78a4fc590c28f96d81cd669.png)

```cpp
void InOrder(BiTree T, int deep){
    if (T) {
        if (deep > 1 && (T->lchild || T->rchild)) cout << '(';    //分支节点打印左括号
        if (T->lchild) InOrder(T->lchild, deep + 1);
        cout << T->data;
        if (T->rchild) InOrder(T->rchild, deep + 1);
        if (deep > 1 && (T->lchild || T->rchild)) cout << ')';    //分支节点打印右括号
    }
}
 
void ans(BiTree T){
    InOrder(T, 1);
}
```

### 图

#### 图的数据结构定义

##### 邻接矩阵

```cpp
#define MAXVEX 100
typedef struct Graph {
    int data[MAXVEX];    //一维数组，存放顶点数据
    int edge[MAXVEX][MAXVEX];    //二维数组，存放边数据（权值）
    int vexNum, edgeNum;    //顶点总数和边总数
}Graph;
```

##### 邻接表

```cpp
#define MAXVEX 100
typedef struct edgeNode {    //边
    struct edgeNode *next;    //指向下一条邻接边的指针
    int weight;    //该邻接边权值
    int adjVex;    //该邻接边指向的顶点编号
}edgeNode;
 
typedef struct vexNode {    //顶点
    edgeNode *firstEdge;    ///指向该顶点的第一条邻接边
    int vexData;    //该顶点数据
}vexNode;
 
typedef struct Graph {    //图
    int vexNum, edgeNum;    //顶点数，边数
    vexNode vex[MAXVEX];    //vexNode类型的一维数组vex
}Graph;
```

#### 图的遍历

##### 深度优先遍历

```cpp
#define MAXVEX 100
bool visited[MAXVEX];    //visited数组记录该顶点是否被访问过
 
void DFSTraverse (Graph G) {
    for (int i = 0; i < G.vexNum; i++) {
        visited[i] = false;    //初始化visited数组
    }
    for (int i = 0; i < G.vexNum; i++) {
        if (!visited[i]) DFS (G, i);    //当前顶点未被访问过，则访问
    }
}
 
void DFS (Graph G, int v) {
    visit (v);    //访问顶点v（具体操作）
    visited[v] = true;    //更新visited数组
    for (int w = FirstNeighbor (G, v); w >= 0; w = NextNeighbor (G, v, w)){
        if (!visited[w]) DFS(G, w);    //递归调用DFS
    }
}
```

##### 广度优先遍历

```cpp
#define MAXVEX 100
Queue Q;
bool visited[MAXVEX];
 
void BFSTraverse (Graph G) {
    for (int i = 0; i < G.vexNum; i++) {    //初始化visited数组
        visited[i] = false;    
    }
    InitQueue Q;    //初始化队列Q
    for (int i = 0; i < G.vexNum; i++) {    //遍历图
        if (!visited[i]) BFS(G, i);
    }
}
 
void BFS (Graph G, int v) {
    visit(v);    //访问该顶点（具体操作）
    visited[v] = true;    //更新visited数组
    EnQueue(Q, v);    //将v结点入队
    int w;
    while(!IsEmpty(Q)) {
        DeQueue(Q, v);    //队首元素出队
        for (w = FirstNeighbor(G, v); w >= 0; w = NextNeighbor(G, v, w)) {
            if (!visited[w]) {    //顶点未被访问过
                visit(w);
                visited[w] = true;
                EnQueue(Q, w);
            }
        }//for
    }//while
}
```

##### 单源最短路径

```cpp
#define MAXVEX 100
bool visited[MAXVEX];
int dis[MAXVEX];
Queue Q;
 
void Min_Dis (Graph G, int v) {
    for (int i = 0; i < G.vexNum; i++) {    //初始化visited数组和dis数组
        visited[i] = false;
        dis[i] = INT_MAX;
    }
    visited[v] = true;
    dis[v] = 0;
    InitQueue(Q);
    EnQueue(Q, v);
    int w;
    while (!IsEmpty(Q)) {
        DeQueue(Q, v);
        for (w = FisrtNeighbor(G, v); w >= 0; w = NextNeighbor(G, v, w) {
            if (!visited[w]) {
                visited[w] = true;
                dis[w] = dis[v] + 1;
            }
        }//for
    }//while
}
```

#### 真题

![fa4a011572674952bb92acaee9c519c5.png](https://ucc.alicdn.com/pic/developer-ecology/35c416525ffc43958b0a9e00f4359ae2.png)

```cpp
int IsExistEL(MGraph G){
    int count = 0;    //记录该图中度为奇数的顶点个数
    int i, j;
    for (i = 0; i < G.numVertices; i++){    //行遍历邻接矩阵
        int degree = 0;
        for (j = 0; j < G.numVertices; j++){    //列遍历当前行
            if (Edge[i][j] > 0) degree++;    //当前数组元素不为0，则度+1
        }
        if (degree % 2) count++;    //当前顶点的度为奇数，count++
    }
    if (count  0 || count  2) return 1;    //奇数顶点个数为0或者2，有EL路径
    else return 0;    //奇数顶点个数不为0或者2，没有EL路径
}
```

### 排序

#### 快速排序

![image-20231007193737798](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202310071937159.png)

#### 归并排序

![image-20231007193752924](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202310071937365.png)

### 查找

#### 折半查找

![image-20231007193809273](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202310071938561.png)

